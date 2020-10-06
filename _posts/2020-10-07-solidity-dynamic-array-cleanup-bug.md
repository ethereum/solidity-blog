---
layout: post
published: true
title:  "Solidity Dynamic Array Cleanup Bug"
date:   2020-10-07
author: Solidity Team
category: Security Alerts
---

On September 17, 2020, a bug in the Solidity code generator was found. The bug is fixed with [version 0.7.3](https://github.com/ethereum/solidity/releases/tag/v0.7.3)
released on October 7, 2020. **The bug is present in all prior versions of Solidity.**

We assigned the bug a severity level of "medium".

## Technical Details of the Bug
**Summary**: For a dynamically-sized storage-array with types of size at most 16 bytes,
assignments that require deleting slots did not zero out the deleted slots properly.

Consider a dynamically-sized array in storage whose base-type is small enough such that multiple
values can be packed into a single slot, such as `uint128[]`. Let us define its length to be `l`.
When this array gets assigned from another array with a smaller length, say `m`, the slots between
elements `m` and `l` have to be cleaned by zeroing them out. However, this cleaning was not
performed properly. Specifically, after the slot corresponding to `m`, only the first packed value
was cleaned up. If this array gets resized to a length larger than `m`, the indices corresponding to
the unclean parts of the slot contained the original value, instead of 0. The resizing here is
performed by assigning to the array `length`, by a `push()` or via inline assembly. 

An example of the bug is the following:

```solidity
// SPDX-License-Identifier: GPL-3.0
// The bug is present in all versions of solidity prior to 0.7.3
pragma solidity >=0.6.0 <0.7.3;
contract C {
    uint128[] x;
    function f() public {
        x.push(42); x.push(42); x.push(42); x.push(42);
        uint128[] memory y = new uint128[](1);
        y[0] = 23;
        // This will shrink the array x to one element.
        x = y;
        // Resizing the array to length 4.
        x.push(); x.push(); x.push();
        // After resizing the array, its contents are [23, 0, 0, 42],
        // instead of [23, 0, 0, 0]. Resizing can be also be done by
        // assigning to `.length` or by assigning to the `slot` member
        // inside inline assembly.
    }
}
```

## How to check if contract is vulnerable

If your contract does all of the following, try to run tests to see if your contract was relying on having elements in the resized part to be zero.
  1. The contract has a dynamically-sized array in storage whose base-type is of size at most 16 bytes.
  2. You perform an assignment to the aforementioned array by another array of smaller
     length (from either memory, storage or calldata).
  3. You increase the length of the aforementioned array without assigning a value to the new elements.
  4. You read from one of the new elements before assigning a new value.

Your contracts are not affected if you are only using `.push(<arg>)` or if you assign a value (even zero) to
the new elements after increasing the length of the array.
