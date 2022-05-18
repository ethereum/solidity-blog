---
layout: post
published: true
title: 'Size Check Bug in Nested Calldata Array ABI-Reencoding'
date: '2022-05-17'
author: Solidity Team
category: Security Alerts
---

On April 7, 2022, a bug in the Solidity code generator was reported by
John Toman of the Certora development team. Certora's bug disclosure post can be found [here](https://medium.com/certora/incorrect-calldata-validation-in-inter-contract-communication-certora-bug-disclosure-8a467ce92ca3).

The bug is fixed with [Solidity version 0.8.14](https://github.com/ethereum/solidity/releases/tag/v0.8.14)
released on May 17, 2022. The bug was first introduced in Solidity version 0.5.8.

We assigned the bug a severity of "very low".

## Which Contracts are Affected?

You might be affected if you pass a nested array directly to another external
function call or use ``abi.encode`` on it.

If calldata is malformed in a certain way, the call might not revert as it should.
Instead, data with extra zeros at the end is passed on to the called contract or to the ``abi.encode`` function.

## Technical Details

Calldata validation for nested dynamic types is deferred until the first access
to the nested values. Such an access may for example be a copy to memory or an
index or member access to the outer type. While in most such accesses calldata
validation correctly checks that the data area of the nested array (see [the ABI
encoding specification](https://docs.soliditylang.org/en/develop/abi-spec.html#use-of-dynamic-types))
is completely contained in the passed calldata (i.e. in
the range ``[0, calldatasize()]``), this check may not be performed, when ABI
encoding such nested types again directly from calldata.

For instance, this can happen, if a value in calldata with a nested dynamic array is
passed to an external call, used in ``abi.encode`` or emitted as event.
In such cases, if the data area of the nested array extends beyond ``calldatasize()``,
ABI encoding it did not revert, but continued reading values from beyond
``calldatasize()`` (i.e. zero values).

For example, in this contract:
```solidity
contract C {
	event e(uint[][]);
	function g(uint[][] calldata) external { ... }
	function f(uint[][] calldata a) external pure {
		bytes memory data = abi.encode(a);
		this.g(a);
		emit e(a);
	}
}
```
A call to ``f`` with corrupted calldata, in which an element of ``a`` has a data area that extends
beyond ``calldatasize``, will not revert.

However, each of the following cases will properly validate against ``calldatasize`` and will revert
with similarly corrupted calldata. This is because the calldata array is only being decoded rather than
decoded and encoded back in a single operation. This is handled by a different code path in the compiler that
is unaffected by this bug:

```solidity
contract C {
	function f1(uint[][] calldata a) external pure {
		a[0]; // Where a[0] is the element that extends beyond ``calldatasize``.
	}
	uint[][] s;
	function f2(uint[][] calldata a) external pure {
		s = a;
	}
	function f2(uint[][] calldata a) external pure {
		uint[][] memory x = a;
	}
}
```
