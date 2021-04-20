---
layout: post
published: true
title:  "Solidity ABI Decoder Bug For Multi-Dimensional Memory Arrays"
date:   2021-04-21
author: Solidity Team
category: Security Alerts
---


On April 5th, 2021, a bug in the Solidity ABI decoder v2 was reported by
John Toman of the Certora development team. Certora's bug disclosure post
can be found here: [Memory Isolation Violation in Deserialization Code](https://www.certora.com/blog/deserialization.html).

The bug is fixed with [Solidity version 0.8.4](https://github.com/ethereum/solidity/releases/tag/v0.8.4)
released on April 21st, 2021. **The bug is present in all prior versions of ABI coder v2.**

We assigned the bug a severity level of "very low", mainly due to the
fact that it is very hard to exploit the bug.

We are very grateful to John for his continuing efforts to discover bugs in the Solidity compiler.

## Who Should Be Concerned

You could be affected if you are using ABI coder v2 (which is the default starting from 0.8.0) and
especially the function ``abi.decode`` on memory (as opposed to calldata) arrays
with multi-dimensional arrays or arrays that contain structs.

The impact of the bug is that if the encoded data is specially crafted,
then the decoded value can depend on values in memory outside of the data to be decoded.

This means that two calls to ``abi.decode`` with the same data can result in different values.

It is not possible to exploit this bug in a way that results in invalid memory write
operations.

Using ``abi.decode`` on calldata byte arrays or the implicit use of ``abi.decode``
for function parameters is unaffected.

## Details of the Bug

The ABI specification uses pointers to data areas for everything that is dynamically-sized.
When decoding data from memory (instead of calldata), the ABI decoder did not properly validate
some of these pointers. More specifically, it was possible to use large values for the pointers
inside arrays such that computing the offset resulted in an undetected overflow.
This would lead to these pointers targeting areas in memory outside of the actual area to be decoded.
This way, it was possible for ``abi.decode`` to return values taken from other areas in memory.
