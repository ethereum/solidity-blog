---
layout: post
published: true
title:  "Solidity Empty Byte Array Copy Bug"
date:   2020-10-19
author: Solidity Team
category: Security Alerts
---

On October 14, 2020, a bug in the Solidity code generator was reported by
John Toman of the Certora development team. Certora's bug disclosure post can be found [here](https://www.certora.com/blog/corruptedStorage.html).

The bug is fixed with [Solidity version 0.7.4](https://github.com/ethereum/solidity/releases/tag/v0.7.4)
released on October 19, 2020. **The bug is present in all prior versions of Solidity.**

We assigned the bug a severity level of "medium".

## Who should be concerned

This bug can cause newly created elements of ``bytes`` or ``string`` arrays in storage
to be initialized by a non-zero value. For this to happen, the following three
actions have to take place:

 - You copy an **empty** ``bytes`` or ``string`` value from ``memory`` or ``calldata`` to storage.
 - You extend the storage value by modifying ``.length`` or using ``.push()`` (not ``.push(c)``).
 - You read the newly created byte array element without writing to it first.

Note that the last step can also be done implicitly by reading the whole byte array
or passing it on to somewhere.

The read operation can result in the new byte array elements being non-zero.

## Example of the bug

```
contract C {
    bytes data;
    function f() public returns (bytes memory) {
        // Empty byte array
        bytes memory t;
        // Store something else in memory after it
        uint[2] memory x;
        x[0] = type(uint).max;
        // Copy the empty byte array to storage,
        // this will copy too much from memory.
        data = t;
        // Create a new byte array element,
        // this will only update the length value.
        data.push();
        // Now, `data[0]` is `0xff` instead of `0`.
        return data;
    }
}

```
