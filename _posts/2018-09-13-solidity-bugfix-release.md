---
layout: post
published: true
title: 'Solidity Bugfix Release'
date: '2018-09-13'
author: Solidity Team
category: Security Alerts
---
_This post was originally published on the [Ethereum blog](https://blog.ethereum.org/2018/09/13/solidity-bugfix-release/)._

The latest [version 0.4.25 release of Solidity](https://github.com/ethereum/solidity/releases/tag/v0.4.25) fixes
two important bugs.
Another important bug has already been fixed in version 0.4.22 but it was only discovered recently that the bug existed.

Note that the Ethereum Foundation runs a [bounty program](https://bounty.ethereum.org/) for the code generator part of Solidity.

## Cleanup of Exponent in Exponentiation

* Likelihood of occurrence: very low
* Exploitability: high
* Discoverability by tests: low
* Fixed in version: 0.4.25

**Summary:** Using short types in the exponent of an exponentiation operation can lead to invalid results.

The Solidity language allows integer types that are shorter than 256 bits, even though the Ethereum Virtual Machine
only knows types of exactly 256 bits. Because of that, higher order bits need to be set to zero from time to time.
For many operations, it is not relevant whether those bits are set to zero or not (addition is one example).
Because of that, the Solidity compiler delays this cleanup until it is needed in order to save gas.

In the very special circumstance that the exponent of the ``**`` operator has a type that is shorter
than 256 bits, but not shorter than the type of the base and contains dirty higher order bits,
this can lead to an incorrect result. Note that literal exponents like in ``x ** 2`` as well as
the case where the type of the base is ``uint256`` or ``int256`` are unaffected.

Note that a function parameter can have dirty higher order bits if called by a malicious entity,
and the same is true for data returned from functions of contracts deployed by malicious entities.

After having screened a large number of contracts, we deem this bug to affect only a very tiny number of
smart contracts, if any at all, because the regular uses of the exponentiation operator do not lead to the bug.

This bug was found by [nweller](https://github.com/nweller).


## Memory Corruption in Multi-Dimensional Array Decoder

* Likelihood of occurrence: low
* Exploitability: medium
* Discoverability by tests: high
* Introduced in version: 0.1.4
* Fixed in version: 0.4.22

**Summary:** Calling functions of other contracts that return multi-dimensional fixed-size arrays results in memory corruption.

If Solidity code calls a function that returns a multi-dimensional fixed-size array,
the returned ABI-encoded data has to be converted to Solidity's internal representation
of arrays. In Solidity, multi-dimensional arrays are implemented as arrays of
memory pointers, while in the ABI, the data is encoded inline.
The decoder did not take this difference into account with the result that the returned
elements are interpreted as memory pointers and thus can cause memory
corruption if the return values are accessed. Calling functions with multi-dimensional
fixed-size array arguments is unaffected as is returning fixed-size arrays from function calls
if they are not used in a Solidity contract.
The bug is only in the component that decodes a multi-dimensional fixed-size array
that is returned from a function call from Solidity.

This bug was found by [jmahhh](https://github.com/jmahhh).

## Invalid Encoding of Structs in Events

* Likelihood of occurrence: low
* Exploitability: low
* Discoverability by tests: high
* Introduced in version: 0.4.17
* Fixed in version: 0.4.25

**Summary:** Structs as event parameters are not handled properly.

Structs were not meant to be supported as event parameters without the new ABI encoder.
The compiler did accept them nevertheless, but encoded their memory address instead of their actual value.
Even with the new ABI encoder, structs cannot be ``indexed`` event parameters.

Now, structs are properly disallowed for the old encoder and if they are indexed also for the new encoder.
