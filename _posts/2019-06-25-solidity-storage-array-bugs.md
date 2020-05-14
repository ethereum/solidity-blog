---
layout: post
published: true
title: 'Solidity Storage Array Bugs'
date: '2019-06-25'
author: Solidity and Security Team
category: Security Alerts
---
_This post was originally published on the [Ethereum blog](https://blog.ethereum.org/2019/06/25/solidity-storage-array-bugs/)._

This blog post is about two bugs connected to storage arrays which are otherwise unrelated. Both have been present in the compiler for a long time and have only been discovered now even though a contract containing them should very likely show malfunctions in tests.

[Daenam Kim](https://www.linkedin.com/in/daenamkim/) with help from [Nguyen Pham](https://www.linkedin.com/in/nguyen-pham-635748161/), both from [Curvegrid](https://www.curvegrid.com/) discovered an issue where invalid data is stored in connection with arrays of signed integers.

This bug has been present since Solidity 0.4.7 and we consider it the more serious of the two. If these arrays use negative integers in a certain situation, it will cause data corruption and thus the bug should be easy to detect.

Through the Ethereum bug bounty program, we received a report about a flaw within the new experimental ABI encoder (referred to as ABIEncoderV2). The new ABI encoder is still marked as experimental, but we nevertheless think that this deserves a prominent announcement since it is already used on mainnet.
Credits to Ming Chuan Lin (of https://www.secondstate.io) for both discovering and fixing the bug!

The [0.5.10 release](https://github.com/ethereum/solidity/releases/tag/v0.5.10) contains the fixes to the bugs.
At the moment, we do not plan to publish a fix to the legacy 0.4.x series of Solidity, but we might if there is popular demand.

Both bugs should be easily visible in tests that touch the relevant code paths.

Details about the two bugs can be found below.

# Signed Integer Array Bug

## Who should be concerned

If you have deployed contracts which use signed integer arrays in storage and either directly assign

 - a literal array with at least one negative value in it (``x = [-1, -2, -3];``) or
 - an existing array of a _different_ signed integer type

to it, this will lead to data corruption in the storage array.

Contracts that only assign individual array elements (i.e. with ``x[2] = -1;``) are not affected.

## How to check if contract is vulnerable

If you use signed integer arrays in storage, try to run tests where you use negative values. The effect should be that the actual value stored is positive instead of negative.

If you have a contract that meets these conditions, and want to verify whether the contract is indeed vulnerable, you can reach out to us via security@ethereum.org.

## Technical details

Storage arrays can be assigned from arrays of different type. During this copy and assignment operation, a type conversion is performed on each of the elements. In addition to the conversion, especially if the signed integer type is shorter than 256 bits, certain bits of the value have to be zeroed out in preparation for storing multiple values in the same storage slot.

Which bits to zero out was incorrectly determined from the source and not the target type. This leads to too many bits being zeroed out. In particular, the sign bit will be zero which makes the value positive.

# ABIEncoderV2 Array Bug

## Who should be concerned

If you have deployed contracts which use the experimental ABI encoder V2, then those might be affected. This means that only contracts which use the following directive within the source code can be affected:


    pragma experimental ABIEncoderV2;


Additionally, there are a number of requirements for the bug to trigger. See technical details further below for more information. 

## How to check if contract is vulnerable

The bug only manifests itself when all of the following conditions are met:
* Storage data involving arrays or structs is sent directly to an external function call, to ``abi.encode`` or to event data without prior assignment to a local (memory) variable AND
* this data either contains an array of structs or an array of statically-sized arrays (i.e. at least two-dimensional).

In addition to that, in the following situation, your code is NOT affected:
* if you only return such data and do not use it in ``abi.encode``, external calls or event data.

## Possible consequences

Naturally, any bug can have wildly varying consequences depending on the program control flow, but we expect that this is more likely to lead to malfunction than exploitability. 

The bug, when triggered, will under certain circumstances send corrupt parameters on method invocations to other contracts. 


## Technical details

During the encoding process, the experimental ABI encoder does not properly advance to the next element in an array in case the elements occupy more than a single slot in storage.

This is only the case for elements that are structs or statically-sized arrays. Arrays of dynamically-sized arrays or of elementary datatypes are not affected.

The specific effect you will see is that data is "shifted" in the encoded array: If you have an array of type ``uint[2][]`` and it contains the data
``[[1, 2], [3, 4], [5, 6]]``, then it will be encoded as ``[[1, 2], [2, 3], [3, 4]]`` because the encoder only advances by a single slot between elements instead of two.

This post was jointly composed by @axic, @chriseth, @holiman.
