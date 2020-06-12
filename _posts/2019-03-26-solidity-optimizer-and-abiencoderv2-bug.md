---
layout: post
published: true
title: 'Solidity Optimizer and ABIEncoderV2 Bugs'
date: '2019-03-26'
author: Solidity and Security Team
category: Security Alerts
---
_This post was originally published on the [Ethereum blog](https://blog.ethereum.org/2019/03/26/solidity-optimizer-and-abiencoderv2-bug/)._

Through the Ethereum bug bounty program, we received a report about a flaw within the new experimental ABI encoder (referred to as ABIEncoderV2). Upon investigation, it was found that the component suffers from a few different variations of the same type. The first part of this announcement explains this bug in detail. The new ABI encoder is still marked as experimental, but we nevertheless think that this deserves a prominent announcement since it is already used on mainnet.


Additionally, two low-impact bugs in the optimizer have been identified over the past two weeks, one of which was fixed with Solidity v0.5.6. Both were introduced with version 0.5.5. See the second part of this announcement for details.


The [0.5.7 release](https://github.com/ethereum/solidity/releases/tag/v0.5.7) contains the fixes to all bugs explained in this blog post.


All the bugs mentioned here should be easily visible in tests that touch the relevant code paths, at least when run with all combinations of zero and nonzero values.


Credits to Melonport team (Travis Jacobs & Jenna Zenk) and the Melon Council (Nick Munoz-McDonald, Martin Lundfall, Matt di Ferrante & Adam Kolar), who reported this via the Ethereum bug bounty program!

# Who should be concerned 


If you have deployed contracts which use the experimental ABI encoder V2, then those might be affected. This means that only contracts which use the following directive within the source code can be affected:


    pragma experimental ABIEncoderV2;


Additionally, there are a number of requirements for the bug to trigger. See technical details further below for more information. 


As far as we can tell, there are about 2500 contracts live on mainnet that use the experimental ABIEncoderV2. It is not clear how many of them contain the bug.


# How to check if contract is vulnerable


The bug only manifests itself when all of the following conditions are met:
* Storage data involving arrays or structs is sent directly to an external function call, to ``abi.encode`` or to event data without prior assignment to a local (memory) variable AND
* there is an array that contains elements with size less than 32 bytes or a struct that has elements that share a storage slot or members of type ``bytesNN`` shorter than 32 bytes.

In addition to that, in the following situations, your code is NOT affected:
* if all your structs or arrays only use ``uint256`` or ``int256`` types
* if you only use integer types (that may be shorter) and only encode at most one array at a time
* if you only return such data and do not use it in ``abi.encode``, external calls or event data.


If you have a contract that meets these conditions, and want to verify whether the contract is indeed vulnerable, you can reach out to us via security@ethereum.org.


# How to prevent these types of flaws in the future


In order to be conservative about changes, the experimental ABI encoder has been available only when explicitly enabled, to allow people to interact with it and test it without putting too much trust in it before it is considered stable. 


We do our best to ensure high quality, and have recently started working on 'semantic' fuzzing of certain parts on [OSS-Fuzz](https://github.com/google/oss-fuzz) (we have previously crash-fuzzed the compiler, but that did not test compiler correctness).


For developers -- bugs within the Solidity compiler are difficult to detect with tools like vulnerability detectors, since tools which operate on source code or AST-representations do not detect flaws that are introduced only into the compiled bytecode.


The best way to protect against these types of flaws is to have a rigorous set of end-to-end tests for your contracts (verifying all code paths), since bugs in a compiler very likely are not "silent" and instead manifest in invalid data.


# Possible consequences


Naturally, any bug can have wildly varying consequences depending on the program control flow, but we expect that this is more likely to lead to malfunction than exploitability. 


The bug, when triggered, will under certain circumstances send corrupt parameters on method invocations to other contracts. 


# Timeline


#### 2019-03-16:  
* Report via bug bounty, about corruption caused when reading from arrays of booleans directly from storage into ABI encoder.

#### 2019-03-16 to 2019-03-21:
* Investigation of root cause, analysis of affected contracts. An unexpectedly high count of contracts compiled with the experimental encoder were found deployed on mainnet, many without verified source-code.
* Investigation of bug found more ways to trigger the bug, e.g. using structs. Furthermore, an array overflow bug was found in the same routine.
* A handful of contracts found on Github were checked, and none were found to be affected. 
* A bugfix to the ABI encoder was made.

#### 2019-03-20:
* Decision to make information public.
* Reasoning: It would not be feasible to detect all vulnerable contracts and reach out to all authors in a timely manner, and it would be good to prevent further proliferation of vulnerable contracts on mainnet.

#### 2019-03-26:
* New compiler release, version 0.5.7.
* This post released.


# Technical details




## Background

The Contract ABI is a specification how data can be exchanged with contracts from the outside (a Dapp) or when interacting between contracts. It supports a variety of types of data, including simple values like numbers, bytes and strings, as well as more complex data types, including arrays and structs.


When a contract receives input data, it must decode that (this is done by the "ABI decoder") and prior to returning data or sending data to another contract, it must encode it (this is done by the "ABI encoder"). The Solidity compiler generates these two pieces of code for each defined function in a contract (and also for ``abi.encode`` and ``abi.decode``). In the Solidity compiler the subsystem generating the encoder and decoder is called the "ABI encoder".


In mid-2017 the Solidity team started to work on a fresh implementation named "ABI encoder V2" with the goal of having a more flexible, safe, performant and auditable code generator. This experimental code generator, when explicitly enabled, has been offered to users since the end of 2017 with the 0.4.19 release.


## The flaw


The experimental ABI encoder does not handle non-integer values shorter than 32 bytes properly. This applies to ``bytesNN`` types, ``bool``, ``enum`` and other types when they are part of an array or a struct and encoded directly from storage. This means these storage references have to be used directly inside `abi.encode(...)`, as arguments in external function calls or in event data without prior assignment to a local variable. Using ``return`` does not trigger the bug. The types ``bytesNN`` and ``bool`` will result in corrupted data while ``enum`` might lead to an invalid ``revert``.


Furthermore, arrays with elements shorter than 32 bytes may not be handled correctly even if the base type is an integer type. Encoding such arrays in the way described above can lead to other data in the encoding being overwritten if the number of elements encoded is not a multiple of the number of elements that fit a single slot. If nothing follows the array in the encoding (note that dynamically-sized arrays are always encoded after statically-sized arrays with statically-sized content), or if only a single array is encoded, no other data is overwritten.


# Two unrelated bugs


Unrelated to the ABI encoder issue explained above, two bugs have been found in the optimiser. Both have been introduced with 0.5.5 (released on 5th of March). They are unlikely to occur in code generated by the compiler, unless inline assembly is used.


These two bugs have been identified through the recent addition of Solidity to [OSS-Fuzz](https://github.com/google/oss-fuzz) - a security toolkit for finding discrepancies or issues in a variety of projects. For Solidity we have included multiple different fuzzers testing different aspects of the compiler.


1. The optimizer turns opcode sequences like ``((x << a) << b))``, where ``a`` and ``b`` are compile-time constants, into ``(x << (a + b))`` while not handling overflow in the addition properly.
2. The optimizer incorrectly handles the ``byte`` opcode if the constant 31 is used as second argument. This can happen when performing index access on ``bytesNN`` types with a compile-time constant value (not index) of 31 or when using the byte opcode in inline assembly.


This post was jointly composed by [@axic](https://github.com/axic), [@chriseth](https://github.com/chriseth), [@holiman](https://github.com/holiman).
