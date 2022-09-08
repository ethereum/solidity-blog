---
layout: post
published: true
title: 'Storage Write Removal Bug On Conditional Early Termination'
date: '2022-09-08'
author: Solidity Team
category: Security Alerts
---
On September 5, 2022, a bug in Solidity's Yul optimizer was found by differential fuzzing.

The bug was introduced in version 0.8.13 and [Solidity version 0.8.17](https://github.com/ethereum/solidity/releases/tag/v0.8.17), released on September 08, 2022, provides a fix. The bug is significantly easier to trigger with optimized via-IR code generation, but can theoretically also occur in optimized legacy code generation.

We assigned the bug a severity of "medium/high".


## Who Should Be Concerned

If you're using optimized legacy code generation, you only need to be concerned, if you use large inline assembly blocks that contain user-defined assembly functions that involve the ``return(...)`` or ``stop()`` instructions. Furthermore only inline assembly blocks that do not interact with any surrounding Solidity variables can be affected.

If you're using optimized via-IR code generation, you are more likely to be affected.

You should carefully check your contract whenever it involves any code path that first writes to storage and then continues to an early termination using assembly ``return(...)`` or ``stop()``, but can also conditionally bypass this early termination. This includes cases in which the storage writes or the early termination occur in complex control flow or in arbitrarily nested function calls.

## Which Contracts are Affected?

A prerequisite for a contract to be affected is the use of ``return(...)`` or ``stop()`` in an inline assembly block (note that ``return(...)`` here refers to the builtin inline assembly function, not to the ``return`` statement of Solidity). Such an inline assembly call will not return from the current function, but instead result in an early successful (i.e. non-reverting) termination of the entire external EVM call.

If your contract doesn't contain any inline assembly block with ``return(...)`` or ``stop()`` statements, you're unaffected.

If it does and this early termination happens conditionally within a function, the optimizer could incorrectly remove storage writes before calls to the function.

More concretely, the bug can occur if a contract contains the following pattern:

1. A storage write. Note that the write may still be removed even if it happens only conditionally or within a call to a function that ends up being inlined.
2. A call to a function that conditionally terminates using inline assembly as described above, but also has a different code path that returns to the caller.
3. Any continuing control flow path does one of the following:
    - It overwrites the storage write in (1).
    - It reverts.

With legacy code generation, all of this, i.e. steps 1-3, need to happen in a single inline assembly block and the function call in (2) needs to be a call to a user-defined assembly function (see below for examples of affected Yul snippets). Also note that during legacy code generation, the Yul optimizer is only run on inline assembly snippets that do not refer to Solidity variables, further reducing the amount of potentially affected cases.

However, when using via-IR code generation the entire contract is first translated to Yul and then optimized as a whole. In this case, 1-3 can happen in Solidity code and only the function called in (2) needs to involve an inline assembly block calling ``return(...)`` or ``stop()`` (the compiler will not generate ``return(...)`` or ``stop()`` instructions that can trigger the bug without the use of inline assembly). Note that due to inlining, this inline assembly block can also merely occur in another nested function call, i.e. any call in (2) that, through any chain of nested calls, can both return to the caller and terminate via assembly is susceptible.

The following is a minimal example that will exhibit the bug if compiled via IR with enabled optimizer:

```solidity
contract C {
	uint public x;
	function f(bool a) public {
		x = 1; // This write is removed due to the bug.
		g(a);
		x = 2;
	}
	function g(bool a) internal {
		// The relevant part of this function is that it can
		// both return to the caller and terminate the transaction.
		// The bug will show its effects in the cases in which
		// the transaction is terminated (i.e. if a is false).
		// In this case the write x = 1 above will be missing.
		if (a) return;
		assembly { return(0,0) }
	}
}
```

Compiling the above via IR with enabled optimizer will result in ``f(false)`` incorrectly terminating the transaction without modifying ``x``.

For a concrete contract, the optimizer may still prevent the bug by inlining the function before running the problematic optimizer step, e.g. the following contract is almost identical, but unaffected, since ``g()`` will be inlined (in the snippet above the Solidity-level ``return;`` prevents easy inlining):

```solidity
contract C {
	uint public x;
	function f(bool a) public {
		x = 1;
		g(a);
		x = 2;
	}
	function g(bool a) internal {
		if (!a)
			assembly { return(0,0) }
	}
}
```

To check whether your contract is actually affected, you need to trigger the condition leading to the assembly ``return(...)`` or ``stop()`` and verify that all storage writes that should have happened in the code leading to it were correctly performed.

## Technical Details

The Yul optimizer step responsible for the bug is the Unused Store Eliminator (abbreviated as `S` in the [optimizer step sequence](https://docs.soliditylang.org/en/latest/yul.html#optimization-step-sequence)). It is meant to remove storage writes that it can determine to be redundant. A storage write is considered redundant, if in all code paths continuing after it either of the following happens:
- A subsequent write overwrites the value written in the initial write.
- The code path unconditionally reverts.

So similarly to above, the general pattern is:
1. a storage write (potentially within complex control flow), then
2. potentially unrelated code, and finally 
3. any continuing control flow path either overwrites the storage write in (1) with a different value or reverts.

The bug is due to the treatment of certain function calls in (2).

If a function call is performed between (1) and (3), the optimizer has to consider the control flow behaviour of the function call, e.g. whether control flow may continue beyond the call, whether the called function always reverts or always successfully terminates. However, in the case that the control flow conditionally can continue after the call to a function, but the function call may also terminate using the ``return(...)`` or ``stop()`` statements (see examples below), the optimizer incorrectly still behaved as if the control flow *always* continued after the function.

Hence the write in (1) could be removed, even though (2) can in fact terminate and is supposed to retain the storage write in (1).

So on the Yul level the issue manifests if the following sequence occurs within a Yul block during optimization:

1. A storage write occurs (e.g. ``sstore(0, 1)``, but the write may also occur within complex control flow like a single ``switch`` case).
2. A user-defined function is called and this function both has a control flow path that terminates the transaction and a control flow path out of the function.
3. Any continuing control flow path does either of the following:
    - It overwrites the storage slot written in (1), s.t. the storage write in (1) becomes redundant for this path (e.g. ``sstore(0, 2)``).
    - The control flow unconditionally reverts (e.g. ``revert(0, 0)``)

In this situation, the optimizer will treat the storage write in (1) as redundant and remove it, despite the fact that the called Yul function may successfully terminate the EVM call, in which case this storage write is in fact not redundant at all.

The following is a full example of an affected Yul snippet:

```yul
{
	function f() {
		if gt(calldatasize(), 4) { leave }
		return(0, 0)
	}
	sstore(0, 1) // This sstore will incorrectly be removed.
	f()
	sstore(0, 2)
}
```

In the absence of the bug, the first ``sstore`` would have been marked as required by the optimizer upon encountering the call to ``f()``, i.e. it should not be considered for removal regardless of what happens afterwards, since the control flow path of ``f()`` reaching the ``return(0, 0)`` already relies on the ``sstore`` happening.
However, the optimizer failed to mark it as such, marking the first ``sstore`` as redundant instead once it sees the second ``sstore`` overwriting the slot.

The issue only occurs in the presence of a call to a conditionally terminating Yul function that survives the preceding optimizer steps.
The following very similarly looking snippet is not affected, since the optimizer will inline the call to ``f()`` before performing
the buggy optimization step:

```yul
{
	function f() {
		if iszero(lt(calldatasize(), 4)) { return(0,0) }
	}
	sstore(0, 1)
	f()
	sstore(0, 2)
}
```

In this case, ``f()`` will be inlined, resulting in

```yul
{
	sstore(0, 1)
	if iszero(lt(calldatasize(), 4)) { return(0,0) }
	sstore(0, 2)
}
```

In which case both ``sstore``s are correctly retained.

However, note that the optimizer determines the properties in question transitively, so the following is still affected as well, even if we assume that ``emptyReturn`` is not inlined:

```yul
{
	function emptyReturn() { return(0, 0) }
	function f() {
		if iszero(lt(calldatasize(), 4)) { emptyReturn() }
	}
	sstore(0, 1)
	f()
	sstore(0, 2)
}
```
