---
layout: post
published: true
title: 'eth2 quick update no. 5'
date: '2019-12-05'
author: Danny Ryan
category: 'Research & Development'
---

![Eth wallpaper](https://blog.ethereum.org/img/2019/10/kumiko_background.jpeg)

Welcome to this week's _eth2 quick update_!

tldr;

* Parity's [Shasper joins](https://twitter.com/sorpaas/status/1202651945430929409) Prysmatic's Sapphire testnet (the first public eth2 multi-client testnet)!
* Multiple eth2 block explorers launch -- Bitfly's [beaconcha.in](https://beaconcha.in/) and [Etherscan](https://beacon.etherscan.io/)
* First eth2 networking call to tackle _all_ the networkings -- [Ben's notes](https://hackmd.io/@benjaminion/BJ3YqrSTr) and [Mamy's notes](https://gist.github.com/mratsim/fef2b0a7c5a335ac6bc61c01592b3fea)
* First [eth2 phase2 community call](https://github.com/ethereum/eth2.0-pm/issues/103) to rally the effort 
* [Fix](https://github.com/ethereum/eth2.0-specs/pull/1495) to ensure fork choice head state matches FFG finality info
* [Presentation](https://www.youtube.com/watch?v=dMFgaeRdsfU&feature=youtu.be&t=1009), [specification](https://github.com/ethereum/eth2.0-specs/pull/1499), and [implementation](https://github.com/ethereum/py_ecc/pull/79) of the new BLS standards

## Shasper joins Prysmatic's testnet

Parity's eth2 client, [Shasper](https://github.com/paritytech/shasper), successfully [joined](https://twitter.com/sorpaas/status/1202651945430929409) Prysmatic's Sapphire Testnet marking the first _public_ multi-client eth2 testnet. This is the exciting start of _many_ multi-client testnets to come in the next month.

You can now pull down the Shasper codebase and with a few commands, and connect to the Sapphire testnet. If you want to give it a shot, follow the instructions [here](https://github.com/paritytech/shasper/pull/191).

## Eth2 block explorers launch

Not [one](https://beaconcha.in/) but [two](https://beacon.etherscan.io/) (!) eth2 block explorers recently launched. Both of these block explorers currently monitor [Prysmatic Labs' Sapphire Testnet](https://prylabs.net/), providing slot-by-slot info as validators build the beacon chain.

Bitfly launched their [beaconcha.in](https://beaconcha.in/) block explorer a couple of weeks ago and continue to add [exciting features](https://twitter.com/etherchain_org/status/1202506562746638336) by the day. Etherscan just launched their [block explorer](https://beacon.etherscan.io/) yesterday, and it, too, looks packed with cool features and data. Both will be great options to monitor Prysmatic's and other testnets as they come online.

We're excited to see more and more user/developer tooling being built around eth2 clients and testnets :)

## First eth2 networking call

Up until this point, we've relied upon github issues/pull-requests, adhoc chats, the main eth2 call, and in person meetings to organize the networking components of the eth2 spec. This has largely worked _well enough_, but researching and architecting a network to support a sharded blockchain protocol is an issue that increasingly deserves some more specific attention and resources.

To this end, we had our first [eth2 networking-specific call](https://github.com/ethereum/eth2.0-pm/issues/106) this week. Although this call is not live-streamed like the general eth2 call, it is public and open to all contributors. Thank you to the p2p networking experts across the various eth2 teams that pushed for this call. I was originally resistant due to the additional coordination overhead, but the first call proved very fruitful and I look forward the next.

As always Ben Edgington ([call notes](https://hackmd.io/@benjaminion/BJ3YqrSTr)) and Mamy Ratsimbazafy ([call notes](https://gist.github.com/mratsim/fef2b0a7c5a335ac6bc61c01592b3fea)) took excellent notes. We still have plenty to dig into, and our next call will be in approximately 2 weeks time.

## First eth2 phase 2 community call

Thank you Will Villanueva from the Quilt team for organizing the [first eth2 phase2 community call](https://github.com/ethereum/eth2.0-pm/issues/103). Similar to the new networking and light client calls, phase 2 has enough going on to warrant a regular call to keep the research and development more organized. 

This first call acted as a general update and survey of the various on-going threads across the many teams and individuals involved. You can read up on the notes [here](https://github.com/ethereum/eth2.0-pm/issues/103#issuecomment-561938586). Subsequent calls are intended to be deeper work sessions on specific technical issues.

A major goal for Phase 2 is to get through the initial wave of stateless protocol research and to use the findings to narrow the scope into a more concrete plan to execute in 2020. These calls are an excellent step forward toward this goal.

## Forkchoice state fix

Researchers at San Jose State University led by Yan X Zhang have been diligently working to formalize the joint properties of eth2's consensus mechanics (Casper FFG) and fork choice rule (LMD GHOST). Before publishing their work, they found a corner case in the interworkings of FFG+GHOST in which a branch of the block tree might include the latest justified/finalized blocks but _not_ actually show those blocks as justified/finalized in the on-chain state. To leave such "non-viable" branches in the fork choice can lead to situations in which a validator's vote might not be coherent with local finality info and might, in certain scenarios, lead to liveness failures. You can read more about this particular scenario and the proposed solution [here](https://notes.ethereum.org/@djrtwo/HynY6CthS).

To address this scenario, we have a [fix under review](https://github.com/ethereum/eth2.0-specs/pull/1495) in the specs repo. We expect this fix to be released within the week.

## Specification and implementation of the new BLS standards

The long-awaited BLS standards were recently presented for public comment at the [IETF Meeting 106](https://www.ietf.org/how/meetings/106/). You can check out the [presentation](https://www.youtube.com/watch?v=dMFgaeRdsfU&feature=youtu.be&t=1009) and [slides](https://datatracker.ietf.org/meeting/106/materials/slides-106-cfrg-update-on-draft-irtf-cfrg-hash-to-curve) for yourself. The presentation went as planned and the standard is expected to be adopted by a number of blockchain projects and (_eventually_) into IETF. To more semi-officially enshrine this standard before the long IETF process finsihes, I expect the EF and many other projects to more officially announce intended usage soon.

There are two draft pull-requests ([[1](https://github.com/ethereum/eth2.0-specs/pull/1398)] [[2](https://github.com/ethereum/eth2.0-specs/pull/1499)]) under review in the specs repo, as well as an implementation of the new standard [under review in `py_ecc`](https://github.com/ethereum/py_ecc/pull/79). Once review is complete, we will generate the new BLS test vectors for general consumption by eth2 clients. The intention is to switch testnets over to the modified BLS scheme come January.

The BLS standards also remove one of the final blockers for launching the eth2 deposit contract. Runtime Verification is currently finishing up their report on the formal verification and analysis of the deposit contract bytecode. This report is expected to be published by the end of the month for public review, after which we can _finally_ launch this thing 🚀. 
