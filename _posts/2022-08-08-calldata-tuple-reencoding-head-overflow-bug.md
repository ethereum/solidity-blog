---
layout: post
published: true
title: 'Head Overflow Bug in Calldata Tuple ABI-Reencoding'
date: '2022-08-08'
author: Solidity Team
category: Security Alerts
---

On July 5, 2022, Chance Hudson ([@vimwitch](https://github.com/vimwitch)) from the Ethereum Foundation discovered a bug in the Solidity code generator.

The earliest affected version of the compiler is 0.5.8, which introduced ABI-reencoding of calldata arrays and structs.
[Solidity version 0.8.16](https://github.com/ethereum/solidity/releases/tag/v0.8.16), released on August 08, 2022, provides a fix.

We assigned the bug a severity of "medium".

## Which Contracts are Affected?

The effects of the bug manifest when a contract performs ABI-encoding of a tuple that meets **all** of the following conditions:
1. The last component of the tuple is a (potentially nested) statically-sized calldata array with the most base type being either `uint` or `bytes32`.
    E.g. `bytes32[10]` or `uint[2][2][2]`.
2. The tuple contains at least one dynamic component.
    E.g. `bytes` or a struct containing a dynamic array.
3. The code is using ABI coder v2, which is the default since Solidity 0.8.0.

Note that structs are represented as tuples in the ABI.
Parameter and return value lists of external functions, events and errors are also implicitly treated as tuples.

The bug can be triggered by explicit use of the `abi.encode...` family of functions as well as the ABI-encoding performed
implicitly for external functions, events and errors by the compiler.

The bug is independent of the use of the optimizer and affects both the legacy and the IR-based compilation pipelines.

## Technical Details

The bug is the result of overly eager cleanup performed by the compiler when copying calldata arrays to memory during ABI-encoding.
Arrays in memory always occupy a multiple of 32 bytes and when the base type does not fill the whole word,
the unused space is guaranteed to be zeroed and remain clean after all high-level Solidity operations.
In the affected cases the compiler would emit code that incorrectly cleaned the end of an array stored in the last component of a tuple,
zeroing 32 bytes belonging to the first dynamic component of the tuple.

### Aggressive Array Cleanup

When the input for the ABI encoder comes from calldata, large parts of it can simply be copied to the output.
The encoder, however, must ensure that the unused spaces between elements are actually
[cleaned](https://docs.soliditylang.org/en/latest/internals/variable_cleanup.html).

Depending on the types, more or less work may be needed and the encoder has several code paths for handling them.
One of them was designed for (possibly multi-dimensional) arrays of simple value types that fill the whole word,
i.e. `uint` and `bytes32`, as well as byte arrays, i.e. `bytes` and `string`.
In this case, due to tight packing of array elements, only the area past the array may need cleanup and that
area is never larger than 32 bytes.

The cleanup was being performed using a single `mstore()` instruction, which always writes exactly 32 bytes.
This was not considered harmful because of the order in which the encoder writes tuple components.
The space after any given tuple component was assumed to be safe to write because it would eventually be
overwritten with the data belonging to the next component.

The cleanup is necessary only in the byte array case, but due to the shared code path was also being
performed for `uint` and `bytes32`.
This is completely redundant, but was overlooked in the initial implementation and not discovered
until now, likely because it does not produce clearly visible side-effects in simple cases.
The redundant instruction may even be removed by the optimizer in newer versions of the compiler.

In order to explain why the assumption turned out to be incorrect, let us take a closer look at
the layout of an ABI-encoded tuple.

### Layout and Encoding Order of Static and Dynamic Tuples

[The ABI encoding of a tuple](https://docs.soliditylang.org/en/latest/abi-spec.html#formal-specification-of-the-encoding)
consists of two areas - the statically encoded *head* and the dynamically encoded *tail*.
Every static component is placed directly in the head area while for dynamic ones, the head contains only the offset
of the location within the tail where the data is stored.
This layout ensures that component locations within the head are fixed and any of them can be accessed without the need to decode all
the components that precede it.

#### Static Encoding Example

Let us consider the following example that encodes a tuple with no dynamic components:
```solidity
struct S {
    address x;
    uint[3] y;
}

contract C {
    function f(bool a, S calldata b, bytes32[2] calldata c) public {}
}
```

An example call to `C.f()` with the following input:

```solidity
f(true, S(0x1111111111222222222233333333334444444444, [uint(11), 12, 13]), [bytes32("a"), "b"])
```

would encode its arguments as follows:
```
0x0000000000000000000000000000000000000000000000000000000000000001 a
0x0000000000000000000000001111111111222222222233333333334444444444 b.x
0x000000000000000000000000000000000000000000000000000000000000000b b.y[0]
0x000000000000000000000000000000000000000000000000000000000000000c b.y[1]
0x000000000000000000000000000000000000000000000000000000000000000d b.y[2]
0x6100000000000000000000000000000000000000000000000000000000000000 c[0]
0x6200000000000000000000000000000000000000000000000000000000000000 c[1]
```

The encoded tuple has no tail and is laid out in the following way:
```
|---------------------------------------------------|
|                        HEAD                       |
|------------+-------------------------+------------|
| value of a |        value of b       | value of c |
|    bool    |             S           | bytes32[2] |
|            |-------------------------|            |
|            |          HEAD of S      |            |
|            |------------+------------|            |
|            | value of x | value of y |            |
|            |   address  |   uint[3]  |            |
|------------+------------+------------+------------|
|     1      |      2     |     3      |      4     |
|------------+------------+------------+------------|
```

The numbers at the bottom indicate the order in which the components are written by the encoder.

#### Dynamic Encoding Example

The example below uses a very similar tuple but with one of the static fields replaced by a dynamic one:
```solidity
struct T {
    bytes x;
    uint[3] y;
}

contract D {
    function f(bool a, T calldata b, bytes32[2] calldata c) public { }
}
```

In this case a call to `D.f()` with the following input:
```solidity
f(true, T("abcd", [uint(11), 12, 13]), [bytes32("a"), "b"])
```
results in a vastly different encoding:
```
0x0000000000000000000000000000000000000000000000000000000000000001 a
0x0000000000000000000000000000000000000000000000000000000000000080 offset of b
0x6100000000000000000000000000000000000000000000000000000000000000 c[0]
0x6200000000000000000000000000000000000000000000000000000000000000 c[1]
0x0000000000000000000000000000000000000000000000000000000000000080 offset of b.x
0x000000000000000000000000000000000000000000000000000000000000000b b.y[0]
0x000000000000000000000000000000000000000000000000000000000000000c b.y[1]
0x000000000000000000000000000000000000000000000000000000000000000d b.y[2]
0x0000000000000000000000000000000000000000000000000000000000000004 b.x (length field)
0x6162636400000000000000000000000000000000000000000000000000000000 b.x (data)
```

The introduction of a dynamic type resulted in `T` itself becoming a dynamic type as well.
The new encoding has two tail areas, at different nesting levels:

```
|---------------------------------------||----------------------------------------|
|                  HEAD                 ||                 TAIL                   |
|------------+-------------+------------||----------------------------------------|
| value of a | offset of b | value of c ||              value of b                |
|    bool    |    uint     | bytes32[2] ||                   T                    |
|            |             |            ||--------------------------||------------|
|            |             |            ||          HEAD of T       || TAIL of T  |
|            |             |            ||-------------+------------||------------|
|            |             |            || offset of x | value of y || value of x |
|            |             |            ||    uint     |   uint[3]  ||    bytes   |
|------------+-------------+------------||-------------+------------||------------|
|     1      |      2      |      6     ||      3      |     5      ||      4     |
|------------+-------------+------------||-------------+------------||------------|
```

The encoding order was also affected.
This is because for each component, the encoder writes both the head and the tail before moving on to the next one.
This results in `c` being written after `b.x`, even though the former precedes the latter in the encoding.
A similar situation happens inside `T` - when `y` is being written the data belonging to the tail of `x` is already in place.

## Effects of the Bug

The aggressive cleanup combined with the non-linear encoding order of dynamic types resulted in a bug where
the initial 32 bytes of the tail could be overwritten during the cleanup of the last head component.

The dynamic tuple example above meets all the criteria for the bug so introducing ABI reencoding would trigger it:

```solidity
struct T {
    bytes x;
    uint[3] y;
}

contract E {
    function f(bool a, T calldata b, bytes32[2] calldata c)
        public
        returns (bool, T calldata, bytes32[2] calldata)
    {
        return (a, b, c);
    }
}
```

Even though `E.f()` is meant to simply return the same input it receives, due to the bug it would produce faulty encoding:

```
0x0000000000000000000000000000000000000000000000000000000000000001 a
0x0000000000000000000000000000000000000000000000000000000000000080 offset of b
0x6100000000000000000000000000000000000000000000000000000000000000 c[0]
0x6200000000000000000000000000000000000000000000000000000000000000 c[1]
0x0000000000000000000000000000000000000000000000000000000000000000 offset of b.x      // Wrong value
0x000000000000000000000000000000000000000000000000000000000000000b b.y[0]
0x000000000000000000000000000000000000000000000000000000000000000c b.y[1]
0x000000000000000000000000000000000000000000000000000000000000000d b.y[2]
0x0000000000000000000000000000000000000000000000000000000000000000 b.x (length field) // Wrong value
0x6162636400000000000000000000000000000000000000000000000000000000 b.x (data)
```

Here, the cleanup performed for `b.y` would overwrite the length field of `b.x`.
Similarly, the cleanup of `c` would overwrite the offset of `b.x`.

Note that the above is not the most minimal example that reproduces the bug.
To better illustrate the consequences, this case has two nested tuples and the overwrite happens twice
but it is possible to reproduce it even with a single tuple with two components.

The case with a static tuple was not affected because, in absence of the tail, the encoding order is linear.
The encoder could still write past the end of the whole area reserved for the encoding but the compiler would be able
to ensure that the memory past it had not been allocated for another purpose, making the cleanup perfectly safe.

The bug only affected the encoding of tuples in the presence of a ``calldata`` array because that was the only situation where
the routine that performs the aggressive cleanup was used.

The routine is a part of the IR-based code generator and runs regardless of the use of the optimizer,
which means that both optimized and unoptimized code is affected.

Since the legacy code generation pipeline does not have its own ABI coder v2 implementation and uses
the IR-based generator for ABI encoding, the bug can be triggered when using either pipeline.
It does not, however, affect ABI coder v1.

## Impact

The bug is very easy to trigger in contrived situations but requires a combination of multiple factors to manifest.
This seems to lower the likelihood of it being triggered, given that it remained undetected through several major releases.
It may, however, have gone unnoticed in the encoded data of events or errors.

Intentionally triggering the bug when calling a function is not exploitable because the caller can always just directly pass
modified input to the function being called.
Plausible attacks against affected contracts can only occur in situations where the contract accepts a static calldata
array as a part of its input and forwards it to another external call without modifying the content.

One of the scenarios that make the bug more likely is batching of orders:

```solidity
struct Order {
    User buyer;
    User seller;
    uint[2] values;
}

struct User {
    address account;
    string name;
}

contract Batcher {
    IMarket immutable market;

    function batchTransaction(Order[] calldata orders) external {
        for (uint i = 0; i < orders.length; ++i) {
            // batchTransaction() can see the original value of buyer.account and act on it.
            // The check here will not fail unless the caller actually sets the address to zero.
            require(orders[i].buyer.account != address(0));

            // Inside transact() buyer.account always becomes zero due to the bug.
            market.transact(orders[i].buyer, orders[i].seller, orders[i].values);
        }
    }
}

interface IMarket {
    function transact(User memory, User memory, uint[2] memory) external;
}
```
