---
layout: post
published: true
title: 'eth2 quick update no. 2'
date: '2019-10-31'
author: Danny Ryan
---

![Eth wallpaper](https://blog.ethereum.org/img/2019/10/kumiko_background.jpeg)

Welcome to the second installment of _eth2 quick update_.

tldr;

* Spec release of [`v0.9.0 -- Tonkatsu`](https://github.com/ethereum/eth2.0-spec-tests/releases/tag/v0.9.0) to ensure Phase 0 development can continue unimpeded.
* Work continues in ironing out the details of the modified Phase 1 proposal.
* Quiet client development focused on `eth1 -> eth2` infrastructure, general hardening for production, and optimizations.

## `Tonkatsu` Release

As promised on the latest [eth2 call](https://www.youtube.com/watch?v=DXGeC7cg71Y), we pushed things forward to release [`v0.9.0` release -- Tonkatsu](https://github.com/ethereum/eth2.0-spec-tests/releases/tag/v0.9.0). This release is largely simplifying with respect to Phase 0. The goal here is to remove any portions of Phase 0 that are opinionated about Phase 1 to ensure Phase 0 development can continue unimpeded regardless of the [work-in-progress modified sharding proposal](https://github.com/ethereum/eth2.0-specs/pull/1427).

Read the [release notes](https://github.com/ethereum/eth2.0-spec-tests/releases/tag/v0.9.0) for more info.

## Ongoing Phase 1 Redesign

As mentioned in the last [_eth2 quick update_](https://blog.ethereum.org/2019/10/23/eth2-quick-update/), we are almost certainly taking a new and simpler direction for Phase 1. The [new sharding proposal](https://notes.ethereum.org/KbEyHiaSRQW_KS7dDK0OFw) facilitates "crosslinks" for all shards at each slot. This drastically simplifies communication between shards and will result in a much better and simpler developer/user experience come Phase 2.


![sharding-status-quo](https://blog.ethereum.org/img/2019/10/sharding-status-quo.png)

<i>Previous cross-shard communication (approximate)</i></center>

![sharding-new-proposal](https://blog.ethereum.org/img/2019/10/sharding-new-proposal.png)


To support this new proposal, the total shard count to start must be reduced from 1024 to the new estimate of 64, with the intention to scale up the number of shards over time (~10 years) as standard resources available to consumer laptops increases. The following are the primary reasons for the requisite reduction in total shards:

* Each shard induces an attestation load on the network and beacon chain at each slot rather than at each epoch
* Each committee must be of a [minimum safe](https://medium.com/@chihchengliang/minimum-committee-size-explained-67047111fa20) number of validators. If there are too many committees per epoch due to high shard count, then there couldn't possibly be enough 32-ETH validators to safely allocate enough to each committee 

Much of the EF research team's focus in the past few weeks has been around vetting and ironing out the details of this new proposal. For more details, check out the [work-in-progress PR](https://github.com/ethereum/eth2.0-specs/pull/1427) or some of the [Phase 1 issues](https://github.com/ethereum/eth2.0-specs/issues?q=is%3Aopen+is%3Aissue+label%3A%22phase+1%22).


## Quiet, yet effective client development

Eth2 clients continue to quietly develop. As discussed on the latest [eth2 call](https://www.youtube.com/watch?v=DXGeC7cg71Y), effort is being put into handling deposits from eth1, generally hardening clients for production, optimization of state transition and BLS implementations, cross-client fuzzing, networking monitoring tooling, and more! Larger single client testnets are in the works as well as continued cross-client experimentation.

Now that `v0.9.0` has been released, clients are updating their state transition logic to pass the new test vectors and are introducing the [simple attestation aggregation strategy](https://github.com/ethereum/eth2.0-specs/blob/master/specs/validator/0_beacon-chain-validator.md#attestation-aggregation).
