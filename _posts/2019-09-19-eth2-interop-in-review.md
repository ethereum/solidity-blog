---
layout: post
published: true
title: 'Eth2 Interop in Review'
date: '2019-09-19'
author: Danny Ryan
---

![Eth wallpaper](https://blog.ethereum.org/img/2019/09/eth-wallpaper.jpg)

Last week, seven of the eight Eth2 clients under active development succeeded in marking the major milestone of moving from single-client to multi-client testnets at the "Interop Lock-in". With this exciting success in Eth2 development, we wanted to reflect on how this point was reached and on what it means to the Ethereum network and ecosystem.

Anyone following Ethereum over the past couple of years has likely become familiar with terms such as "Ethereum 2.0", "Eth2", or "Serenity". Each of these refer to substantial upgrades slated for the Ethereum protocol that have been envisioned in some form since before the network went live in 2015.

In the early years of Ethereum, groundbreaking research was accomplished in parallel to the original chain (Eth1) launching, while the massive growth of the Ethereum community that followed aided the initial adoption of decentralized applications. Still, the road from these early breakthroughs to a highly decentralized, yet scalable proof-of-stake blockchain has been long. Over the past 18 months though, research has finally stablized into a cohesive and complete vision for the coming major upgrades known as Eth2.

As research moved into specifications toward the end of 2018, [many teams](https://docs.ethhub.io/ethereum-roadmap/ethereum-2.0/eth2.0-teams/teams-building-eth2.0/) (client teams) from across the community stepped up to build out core implementations of the protocol (clients). Since then, there has been a dynamic play between specification and implementation. [Fort-nightly calls](https://github.com/ethereum/eth2.0-pm/) and a common [spec repository](https://github.com/ethereum/eth2.0-specs) organize communication and the sharing of ideas, but client teams have primarily worked in relative isolation, building and testing their implementations of the protocol. 

While the spec was a moving target, clients could only dig so deep into interoperability and optimizations, but once the Phase 0 specification of Eth2 was deemed ["frozen"](https://github.com/ethereum/eth2.0-specs/releases/tag/v0.8.0) on July 1, 2019, clients made tremendous progress and began to take concrete steps toward production.

## Interop

Joseph Delong from [Pegasys](https://pegasys.tech/) had the crazy idea of gathering members from each of the client engineering teams in a remote location for a week of interoperability work. The event was deemed the "Interop Lock-in" or as it was generally referred -- "Interop". With the spec freeze in sight and De**v**con on the horizon, Interop in September was an opportunity have all of these stakeholders work through initial interop-issues in person. 

The [primary purpose](https://github.com/ethereum/eth2.0-pm/blob/master/interop/lockin/goals-exercises.md) of the event was to have each participating client to achieve pair-wise interoperability with each other client in small test networks -- `Lighthouse <-> Artemis`, `Lodestar <-> Lighthouse`, `Lodestar <-> Artemis`, etc.

Participating client teams included:
* [Artemis](https://github.com/PegaSysEng/artemis)
* [Lighthouse](https://github.com/sigp/lighthouse)
* [Lodestar](https://github.com/ChainSafe/lodestar)
* [Harmony](https://github.com/harmony-dev/beacon-chain-java)
* [Nimbus](https://github.com/status-im/nim-beacon-chain)
* [Prysm](https://github.com/prysmaticlabs/prysm)
* [Trinity](https://github.com/ethereum/trinity)

Additional goals involved testing (1) larger networks in both node count and (2) validator count, (3) networks with 3+ clients, (4) enhancing tooling for monitoring and debugging Eth2 networks, and (5) other fun things like getting raspberry pis running and building fork visualizers. 

Leading up to the event, some goals seemed like a stretch, but teams worked diligently until the deadline and achieved amazing progress. By the end of the week, client teams far exceeded original expectations of having a few pair-wise networks, instead completing the entire pair-wise tests, building a small network of _all_ 7 participating clients, and more.

The following represent a glimpse into the highlights of the client successes, but is certainly not exhaustive:

### Multi-client testnets

![5-way eth2 testnet](https://blog.ethereum.org/img/2019/09/5-way-interop.jpg)

* All 7 participating clients achieved [pair-wise interoperability](https://twitter.com/paulhauner/status/1170952290469122048), and although an 8th, [Shasper](https://github.com/paritytech/shasper), was not able to be in attendence, they have [begun](https://twitter.com/sorpaas/status/1172350637847326722) to work through this milestone as well.
* Many larger testnets were formed between 3+ clients, 3+ nodes, and higher than minimal validator counts.
* [All 7 clients](https://twitter.com/JonnyRhea/status/1172233598109442049) in attendence were successfully run on a single network.
* All participating languages' libp2p implementations are now interoperable after debugging some minor issues.

### Network debugging and tools

![Client metrics](https://blog.ethereum.org/img/2019/09/trinity-dashboard.png "Client metrics dashboard")

* Some [consensus errors](https://github.com/djrtwo/interop-test-cases/) between clients were identified, debugged, and recorded as portions of the state transition that require increased test coverage.
* Command line tools were built to better debug ssz objects and state transitions ([zcli](https://github.com/protolambda/zcli), [pycli](https://github.com/djrtwo/pycli), and similar tools embedded within clients).
* Progress made on metrics dashboards, a fork visualizer, and other tools to better understand clients and networks
* Clients were packaged up into containers to perform large-scale network tests within the [Whiteblock](https://whiteblock.io/) genesis platform. 

### And then some

![Eth2 on a pi](https://blog.ethereum.org/img/2019/09/eth2-pi.jpg "Eth2 testnet on a Raspberry Pi")

* Client teams served as eachother's first alpha users resulting in extensive [build/run scripts](https://github.com/status-im/nim-beacon-chain/tree/interop/multinet) and related documentation.
* Isolated load tests with Nimbus and Lighthouse handled 2000+ validators on a single machine paired with similarly full nodes over LAN.
* Multiple clients were built and tested on a [small raspberry pi network](https://twitter.com/jcksie/status/1171886857908609024).

## And beyond

Interop marked a major inflection point for Eth2. There is still much work to accomplish before launch, but engineering efforts will increasingly be geared toward testnets, optimizations, and usability -- work that begins to shift this software into the hands of users.

So what's up next for client teams and eth2 development?

* Benchmarks and optimizations
* Test sync, stress test networks, etc
* Public and incentivized testnets
* Third party audits
* Polishing the validator user experience

Finally, we owe a special thank you to the [ConsenSys](consensys.net) team for helping to organize, host and provide resources that made Interop possible.
