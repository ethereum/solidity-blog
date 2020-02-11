---
layout: post
published: true
title: 'eth2 quick update no. 3'
date: '2019-11-08'
author: Danny Ryan
category: 'Research & Development'
---

![Eth wallpaper](https://blog.ethereum.org/img/2019/10/kumiko_background.jpeg)

Welcome to the third installment of _eth2 quick update_.

tldr;

* Harden fork choice defences in response to audits
* Introducing [challenges.ethereum.org](https://challenges.ethereum.org/)
* Herumi grant for a super-fast BLS implementation

## Fork choice defenses

This week, we hardened the defenses of the Phase 0 fork choice rule. Much of our time is spent analyzing eth2 to ensure that the system can remain stable even under adverse conditions (e.g. a large attacker, a major network outage or partition, etc). With any large engineering project, the aim is to find as many potential issues and harden against these scenarios prior to release. This is why we've engaged with many external teams and researchers to perform audits, economic analysis, and formal verification of eth2, but at the same time as we move toward release, we must remain vigilant and flexible to respond to unforseen attacks in production (e.g. the Shanghai DoS attacks of 2016).

Helping in this effort, a consensus researcher, Ryuya Nakamura, has analyzed eth2's consensus algorithm (Casper FFG) and fork choice rule (LMD GHOST) to better determine how these two pieces of the protocol can in some scenarios be exploited. He has a number of excellent write-ups on [ethresearch.ch](https://ethresear.ch) discussing in detail some attack vectors and recommended solutions. Check out [Decoy-flip-flop attack on LMD GHOST](https://ethresear.ch/t/decoy-flip-flop-attack-on-lmd-ghost/6001), [Analysis of bouncing attack on FFG](https://ethresear.ch/t/analysis-of-bouncing-attack-on-ffg/6113), and [Prevention of bounding attack on FFG](https://ethresear.ch/t/prevention-of-bouncing-attack-on-ffg/6114) for some informative and fun reads.

Fortunately, the recommended solutions to the decoy-flip-flop and bouncing attacks are both simple modifications to the fork choice specification of Phase 0. The decoy-flip-flop attack is solved by only considering validator attestations from the current and previous epoch when _adding_ attestations to the view of the fork choice. You can see the PR making this change to the spec [here](https://github.com/ethereum/eth2.0-specs/pull/1466). The bounce attack is solved by only updating the latest justified checkpoint within the fork choice during the first `k` slots of an epoch. After `k` slots, you defer the inclusion of a new justified checkpoint into the fork choice until the next epoch boundary. You can check out this modification [here](https://github.com/ethereum/eth2.0-specs/pull/1465).

A number of other components of the Phase 0 specification are out for audit currently. Although we don't expect anything major, we do expect to a small number of additional post-audit suggested changes to make it to PRs in the coming months.

## challenges.ethereum.org

We just released [challenges.ethereum.org](https://challenges.ethereum.org/). This site is a single source for all of the current bounties hosted by the Ethereum Foundation and/or related to Ethereum research and development. The bounties range from finding vulnerabilities in existing Ethereum infrastructure to finding collisions in new hash functions. Something for everyone :)

We will continue building out EF's bounty program in the coming months so check back for more opportunities.

## Herumi BLS grant

We've given out a grant to Shigeo Mitsunari, the maintainer of the super-fast Herumi [pairing library](https://github.com/herumi/mcl) and [BLS signature library](https://github.com/herumi/bls). This grant is to bring the libraries up to spec with the new BLS standard ([new hash-to-G2](https://tools.ietf.org/html/draft-irtf-cfrg-hash-to-curve-05), constant-time signing, etc) as well as to help accomplish some more practical items for eth2 usability (Rust integration, fuzzing work, etc).

In addition to the feature component of the grant, there is also a performance component. The Herumi library is already 2-3x faster than the next best BLS option, but because signature verification is one of the main bottlenecks in eth2, any additional gains here would be excellent, enabling eth2 to run under even higher load and in more adverse scenarios. Shigeo will spend some additional time and effort further optimizing the library to ensure it runs _as fast as possible_ 🚀.
