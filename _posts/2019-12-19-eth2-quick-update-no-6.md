---
layout: post
published: true
title: 'eth2 quick update no. 6'
date: '2019-12-19'
author: Danny Ryan
---

![Eth wallpaper](https://blog.ethereum.org/img/2019/10/kumiko_background.jpeg)

Welcome to this week's _eth2 quick update_! A handful of new eth2 testnets have come online and last week I had them [all running on my laptop](https://twitter.com/dannyryan/status/1204518958621233153). There is still plenty of work to be done, but this is getting real :).

tldr;

* Announcing comprehensive Phase 0 spec audit by [Least Authority](https://leastauthority.com)
* Sigma Prime [launched public Lighthouse testnet](https://lighthouse.sigmaprime.io/update-19.html) with 16k+ validators and mainnet spec configuration, pushing the envelope on eth2 scale
* Nimbus [integrates](https://github.com/status-im/nim-beacon-chain/pull/632) native [nim-libp2p](https://github.com/status-im/nim-libp2p) to be launched in testnet reboot next week
* beaconcha.in [adds support](https://twitter.com/etherchain_org/status/1207278115954810884) for [Lighthouse](https://lighthouse.beaconcha.in/) in open source block explorer
* Eth2 spec version [`v0.9.3` released](https://github.com/ethereum/eth2.0-specs/releases/tag/v0.9.3), stabilizing core spec. `v0.10.0` release scheduled for January to integrate [new BLS standards](https://github.com/ethereum/eth2.0-specs/pull/1532)

## Comprehensive Phase 0 audit to be conducted by Least Authority

We are excited to announce an upcoming eth2 spec audit to be conducted by [Least Authority](https://leastauthority.com/). This is a comprehensive security audit of the [eth2 core Phase 0 specifications](https://github.com/ethereum/eth2.0-specs) focusing on critical items such as Denial of Service (DoS) attacks, resource misuse attacks that could lead to unintended forks/adversarial chains, network related attacks, any attacks that impact funds, and more.

Least Authority is a leading expert in mechanism & incentive design, decentralized/distributed systems architecture, and analysis of blockchain systems. The Ethereum Foundation has previously engaged with Least Authority on work in the past such as an [audit of discv5](https://leastauthority.com/static/publications/LeastAuthority-Node-Discovery-Protocol-Audit-Report.pdf) and an [analysis of gas economics and proof of work](https://github.com/LeastAuthority/ethereum-analyses/blob/master/README.md), and we are excited for this opportunity to work with them again. A comprehensive security audit as critical to a successful launch of Phase 0, and we eagerly await the initial report to be released in early February.

## Lighthouse launches testnet with mainnet configuration

Last week, [Sigma Prime](https://sigp.io/) launched their long-awaited [public Lighthouse testnet](https://lighthouse.sigmaprime.io/update-19.html). This testnet impressively *ran the mainnet spec configuration*, and at its peak was easily handling 16815 active validators!

![e2qu61](https://blog.ethereum.org/img/2019/12/e2qu61.jpg)
As stated by Sigma Prime at the launch of this net, "we're going to start trying to crash this testnet and I suspect we'll be successful", and successful they were. After recovering from non-finality of 100+ epochs due to two of their validator-heavy nodes going offline, the Sigma Prime team decided to take down the network to iron out some bugs, work in some new optimizations, and restart fresh _soon_. Check out the [postmortem here](https://lighthouse.sigmaprime.io/update-20.html). As discussed on today's eth2 call, the team intends to relaunch the testnet tomorrow, opening it up for public use after initial stability testing.

Remember to be an _active_ participant in these early testnets if you feel capable: If something isn't clear in the docs, let them know. If you have trouble compiling, open an issue. If there's a typo in the readme, fix it! This goes for all of the clients and for open source software in general. The more you actively give back in this process, the better things will be for everyone.

_P.S._ Sigma Prime is hiring a full-time __experienced Rust developer__ to work on Lighthouse. [Check it out](https://lighthouse.sigmaprime.io/hiring-dec-2019.html)!

## Nimbus integrates native nim-libp2p

Earlier this year, the Ethereum Foundation and Protocol Labs [co-funded a grant for Nimbus](https://blog.ethereum.org/2019/08/26/announcing-ethereum-foundation-and-co-funded-grants/) to create a native [Nim language](https://nim-lang.org/) implementation of libp2p. [This implementation](https://github.com/status-im/nim-libp2p) was to be integrated into the nimbus eth2 client as well as to become a p2p networking option for resource restricted devices.

![e2qu62](https://blog.ethereum.org/img/2019/12/e2qu62.jpg)

We are pleased to announce that Nimbus just recently [integrated this native implementation](https://github.com/status-im/nim-beacon-chain/pull/632) into their codebase and plan on restarting their testnet with it this coming week. This is a huge milestone both for Nimbus and also for the p2p community at large due to Nim's ability to compile succinct and efficient code for most computer architectures. _P2P all the things!_

## beaconcha.in adds support for Lighthouse

Bitfly's [_open source_ eth2 block explorer](https://github.com/gobitfly/eth2-beaconchain-explorer), beaconcha.in, just [added support](https://twitter.com/etherchain_org/status/1207278115954810884) for Lighthouse! You can give it a look [here](https://lighthouse.beaconcha.in/), but things are currently inactive until Lighthouse reboots their network.

We are super excited to see multiple client implementations being onboarded, aiding in creating common interfaces for external software to query and understand the internals of eth2. This and other tools will be crucial in monitoring, understanding, and interacting with upcoming testnets and eventually mainnet!

## Eth2 spec release schedule

We released eth2 spec verson [`v0.9.3` -- rm `signing_root`](https://github.com/ethereum/eth2.0-specs/releases/tag/v0.9.3) and have `v0.9.4` with a testing fix and network update in the queue. These are the last in the `v0.9.x` series of minor updates since the un-freezing of Phase 0 in October. These recent spec releases have been primarily focused on ensuring the Phase 0 Beacon Chain is flexible enough to accommodate the [new Phase 1 sharding design](https://notes.ethereum.org/KbEyHiaSRQW_KS7dDK0OFw), but some clean-ups, bug fixes, enhanced testing, and networking updates have also made it into the changelog. Many of these changes have been informed and driven by the continued client progress on testnets and general readying for mainnet.

The next scheduled release is for early January. This `v0.10.0` release will be dedicated to the integration of the [new BLS standards into the core eth2 specs](https://github.com/ethereum/eth2.0-specs/pull/1532). The `v0.10.x` release series with production-grade BLS is planned to be the target for the final testnets and ultimately for mainnet release. There is still work to be done, but there is an all-around heroic effort currently tackling it. Thank (or fund/tip/etc!) the client teams. These engineers are building something truly incredible for us all :rocket:.
