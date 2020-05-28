---
layout: post
published: true
title:  "Solidity Memory Array Creation Overflow Bug"
date:   2020-04-06
author: Solidity Team
category: Security Alerts
---

On the 28th of March, a bug in the Solidity code generator was reported through the
<a href="https://bounty.ethereum.org/">Ethereum Foundation Bounty program</a>,
by John Toman of <a href="https://www.certora.com/">Certora</a>. The bug is fixed with version 0.6.5,
released on 2020-04-06.
The bug is present in all prior versions of Solidity.

We assigned a severity level of "low" because we found the bug to be uncommon and at the same time hard to exploit.

## Who should be concerned

If you have deployed a contract which allocates a memory array of user-supplied length,
but does not copy or iterate over it, this can lead to memory corruption.
In particular, a sufficiently large length during the allocation of the memory array
will result in subsequent memory allocations to overlap with the memory region of the array.
However, the length of the array is correctly stored, so copying or iterating over the array
(which is what all contracts we screened do just after the allocation)
will terminate and revert the transation with out-of-gas in these cases.

## How to check if contract is vulnerable

If you allocate dynamic memory arrays using ``new T[](length)`` with a length that depends
on user input, you may be vulnerable to this bug. If it is possible to supply a sufficiently
large length, subsequent memory allocations will have overlapping memory regions and operations
that use memory scratch space can invalidate the contents of the array.
If you traverse or copy the array after creating it, however,
the transaction will revert with out-of-gas, since the array length is stored correctly.

Safe example:

```solidity
    contract C {
        function f(uint length) public {
            uint[] memory x = new uint[](length);
            // This is safe because the loop will run out of gas.
            for (uint i = 0; i < length; ++i) {
                x[i] = i**2;
            }
            ...
        }
    }
```

Another safe example:

```solidity
    contract C {
        uint[] x;
        function f(uint length) public {
            // This is safe because the copy from memory to storage will
            // iterate over the whole array and go out-of-gas.
            x = new uint[](length);
            ...
        }
    }
```

Potentially vulnerable example:

```solidity
    contract C {
        function f(uint length, AnotherContract c, uint index) public {
            uint[] memory x = new uint[](length);
            uint[] memory y = new uint[](4);
            y[0] = c.g(x[index]);
            y[1] = c.g(x[index + 1]);
            y[2] = c.g(x[index + 2]);
            y[3] = c.g(x[index + 3]);
            c.h(y);
        }
    }
```

## Technical details

Solidity did not enforce an upper length limit for dynamic array allocations at runtime.
While the length of the array is stored directly as is, the amount of memory to allocate,
i.e. the increment for the "free memory pointer", is calculated as 32 times the array length.
Since this multiplication was not protected against overflows, a sufficiently large length
can cause an overflow, resulting in only a small amount of memory to be actually allocated
(i.e. the free memory pointer will only be incremented by the overflowed size).
Subsequent memory allocations will therefore use memory regions that overlap with
the originally allocated array.
Starting from Solidity version 0.6.5 the maximum allocation size for dynamic memory arrays
is ``2**64-1``. Attempting to allocate larger arrays now directly reverts.
This effectively prevents such overflows from occurring.
