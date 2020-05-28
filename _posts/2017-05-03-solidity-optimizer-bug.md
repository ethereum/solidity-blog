---
layout: post
published: true
title: 'Solidity Optimizer Bug'
date: '2017-05-03'
author: Martin Swende
category: Security Alerts
---
_This post was originally published on the [Ethereum blog](https://blog.ethereum.org/2017/05/03/solidity-optimizer-bug/)._

A bug in the Solidity optimizer was reported through the [Ethereum Foundation Bounty program](https://bounty.ethereum.org/), by Christoph Jentzsch. This bug is patched as of 2017-05-03, with the release of Solidity 0.4.11.

## Background

The bug in question concerned how the optimizer optimizes on constants in the byte code. By "byte code constants", we mean anything which is ``PUSH``ed on the stack (not to be confused with Solidity constants). For example, if the value ``0xfffffffffffffffffffffffffffffffffffffffffffffffe`` is ``PUSH``ed, then the optimizer can either do ``PUSH32 0xfffffffffffffffffffffffffffffffffffffffffffffffe``, or choose to encode this as ``PUSH1 1; NOT;``.

An error in the optimizer made optimizations of byte code constants fail for certain cases by producing a routine that did not properly recreate the original constant.

The behavior described in the reported bug was found in a contract in which one method ceased functioning when another - totally unrelated - method was added to the contract. After analysis, it was determined that a number of conditions must exist at once for the bug to trigger. Any combination of conditions that would trigger the bug would consistently have the following two conditions:

- The constant needs to start with ``0xFF...`` and end with a long series of zeroes (or vice versa).
- The same constant needs to be used in multiple locations, for the optimizer to choose to optimize this particular constant. Alternatively, it needs to be used in the constructor, which optimises for size rather than gas.

In addition to the two conditions above, there are further, more complicated conditions that are required.

## Analysis

This bug is present in all released versions of Solidity from at least as far back as summer 2015 to the present. Although the bug has been present since 2015, it seems very hard to trigger by “random” code:

We performed a static analysis of all contract code deployed on the blockchain, and found no occurrence of such an invalidly generated routine. Note, the fact that we have not found a bug in all the contract code does not guarantee the absence of such occurrences.

## Improvements

In order to provide better transparency and increased awareness of bugs in Solidity, we have started exporting information about Solidity-related vulnerabilities as JSON-files in the Solidity code repository([1](https://github.com/ethereum/solidity/blob/develop/docs/bugs.json), [2](https://github.com/ethereum/solidity/blob/develop/docs/bugs_by_version.json)). We hope that block explorers will integrate this information along with other contract-related information.

Etherscan has already implemented this, which can be seen [here](https://etherscan.io/address/0x83b5c924b74e0dc12386fa110c28faa1efedb07b#code) and [here](https://etherscan.io/contractsVerified).

Concerning the bug itself, we added a mini-EVM to the optimizer which verifies the correctness of each generated routine at compile time.

Furthermore, work has already started on a fully-specified and more high-level intermediate language. Future optimizer routines on this language will be much easier to understand and audit and it will replace the current optimizer.
