---
layout: post
published: true
title: 'eth2 quick update no. 8'
date: '2020-02-04'
author: Danny Ryan
category: 'Research & Development'
---

![Eth wallpaper](https://blog.ethereum.org/img/2019/10/kumiko_background.jpeg)

Keep it coming

tldr;

* Audit and [formal verification](https://github.com/runtimeverification/verified-smart-contracts/blob/master/deposit/bytecode-verification/deposit-spec.ini) of deposit contract bytecode completed by [Runtime Verification](https://runtimeverification.com/)
* The word of the month is "optimization"
* Ongoing Phase 2 research driven by Quilt and eWASM. Welcome TXRX to the table
* [Whiteblock](https://whiteblock.io/) releases [gossipsub testing results](https://github.com/whiteblock/gossipsub-testing)
* Stacked spring! Conferences, hackathons, bounties, and more

## Runtime Verification audit and verification of deposit contract

Runtime Verification recently completed their audit and [formal verification](https://github.com/runtimeverification/verified-smart-contracts/blob/master/deposit/bytecode-verification/deposit-spec.ini) of the [eth2 deposit contract](https://github.com/ethereum/eth2.0-specs/blob/dev/deposit_contract/contracts/validator_registration.vy) bytecode. This is a significant milestone bringing us closer to the eth2 Phase 0 mainnet. Now that this work is complete, I ask for review and comment by the community. If there are gaps or errors in the formal specification, please post an issue on the [eth2 specs repo](https://github.com/ethereum/eth2.0-specs).

The formal semantics specified in the [K Framework](http://www.kframework.org) define the precise behaviors the EVM bytecode should exibit and proves that these behaviors hold. These include input validations, updates to the iterative merkle tree, logs, and more. Take a look [here](https://github.com/runtimeverification/verified-smart-contracts/blob/master/deposit/bytecode-verification/deposit-spec.ini.md) for a (semi)high-level discussion of what is specified, and dig in deeper [here](https://github.com/runtimeverification/verified-smart-contracts/blob/master/deposit/bytecode-verification/deposit-spec.ini) for the full formal K specification.

I want to thank Daejun Park (Runtime Verification) for leading the effort, and Martin Lundfall and Carl Beekhuizen for much feedback and review along the way.

Again, if this stuff is your cup of tea, now is the time to provide input and feedback on the formal verification -- please take a look.

## The word of the month is "optimization"

The past month has been all about optimizations.

Although a 10x optimization here and a 100x optimization there doesn't feel so tangible to the Ethereum community today, this phase of development is just as important as any other in getting us to the finish line.

### Beacon chain optimizations are critical

_(why can't we just max out our machines with the beacon chain)_

The beacon chain -- the core of eth2 -- is a requisite component for the rest of the sharded system. To sync any shard -- whether it be a single shard or many, a client _must_ sync the beacon chain. Thus, to be able to run the beacon chain and a handful of shards on a consumer machine, it is paramount that the beacon chain is relatively low in resource consumption even when high validator participation (~300k+ validators).

To this end, much of the effort of eth2 client teams in the past month has been dedicated to optimizations -- reducing resource requirements of phase 0, the beacon chain.

I'm pleased to report we're seeing fantastic progress. What follows is _not_ comprehensive, but is instead _just a glimpse_ to give you an idea of the work.

### Lighthouse runs 100k validators like a breeze

Lighthouse brought down their ~16k validator testnet a couple of weeks ago after an attestation gossip relay loop caused the nodes to essentially DoS themselves. Sigma Prime quickly patched this bug and looked to bigger and better things -- i.e. a 100k validator testnet! The past two weeks have been dedicated to optimizations to make this real-world scale testnet a reality. 

A goal of each progressive Lighthouse testnet is to ensure that thousands of validators can easily run on a small VPS provisioned with 2 CPUS and 8GB of RAM. Initial tests with 100k validators saw clients use a consistent 8GB of RAM, but after a few days of optimizations Paul was able to reduce this to a steady 2.5GB with some ideas to get it even lower soon. Lighthouse also made 70% gains in the hashing of state which along with BLS signature verification is proving to be the main computational bottleneck in eth2 clients. 


The new Lighthouse testnet launch is _imminent_. Pop into [their discord](https://discord.gg/cyAszAh) to follow progress 

### Prysmatic testnet still chugging and sync massively improved

A couple of weeks ago the current Prysm testnet [celebrated their 100,000th slot](https://twitter.com/terencechain/status/1220456877626220544) with over 28k validators validating. Today, the testnet passed slot 180k and has over 35k active validators. Keeping a public testnet going while at the same time cranking out updates, optimizations, stability patches, etc is quite a feat.

There is a ton of tangible progress ongoing in Prysm. I've spoken with a number of validators over the past few months and from their perspective, the client continues to markedly improve. One especially exciting item is improved sync speeds. The Prysmatic team optimized their client sync from ~0.3 blocks/second to more than 20 blocks/second. This greatly improves validator UX, allowing them to connect and start contributing to the network much faster.

Another exciting addition to the Prysm testnet is [alethio's](https://aleth.io/) new eth2 node monitor -- [eth2stats.io](https://eth2stats.io/). This is an opt-in service that allows nodes to aggregate stats in single place. This will allow us to better understand the state of testnets and ultimately eth2 mainnet.

Don't trust me! [Pull it down and try it out for yourself](https://prylabs.net/).

### Everyone loves `proto_array`

The core eth2 spec frequently (knowingly) specifies expected behavior non-optimally. The spec code is instead optimized for readability of intention rather than for performance.

A spec describes correct behavior of a system, while an algorithm is a procedure for executing a specified behavior. Many different algorithms can faithfully implement the same specification. Thus the eth2 spec allows for a wide variety of different implementations of each component as client teams take into account any number of different tradeoffs (e.g. computational complexity, memory usage, implementation complexity, etc).

One such example is the [fork choice](https://github.com/ethereum/eth2.0-specs/blob/v0.10.0/specs/phase0/fork-choice.md) -- the spec used to find the head of the chain. The eth2 spec specifies the behavior using a naive algorithm to clearly show the moving parts and edge cases -- e.g. how to update weights when a new attestation comes in, what to do when a new block is finalized, etc. A direct implementation of the spec algorithm would never meet the production needs of eth2. Instead, client teams must think more deeply about the computational tradeoffs in the context of their client operation and implement a more sophisticated algorithm to meet those needs.

Lucky for client teams, about 12 months ago Protolambda implemented [a bunch of different fork choice algorithms](https://github.com/protolambda/lmd-ghost), documenting the benefits and tradeoffs of each. Recently, Paul from Sigma Prime observed a major bottleneck in Lighthouse's fork choice algorithm and went shopping for something new. He uncovered [`proto_array`](https://github.com/protolambda/lmd-ghost#array-based-stateful-dag-proto_array) in proto's old list.

It took some work to port `proto_array` to fit the most recent spec, but once integrated, `proto_array` proved "to run in orders of magnitude less time and perform significantly less database reads." After the initial integration into Lighthouse, it was quickly picked up by Prysmatic as well and is available in their most recent release. With this algorithm's clear advantages over alternatives, `proto_array` is quickly becoming a crowd favorite, and I fully expect to see some other teams pick it up soon!

## Ongoing Phase 2 research -- Quilt, eWASM, and now TXRX

Phase 2 of eth2 is the addition of state and execution into the sharded eth2 universe. Although some core principles are relatively defined (e.g. communication between shards via crosslinks and merkle proofs), the Phase 2 design landscape is still relatively wide open. Quilt (ConsenSys research team) and [eWASM](https://github.com/ewasm) (EF research team) have spent much of their efforts in the past year researching and better defining this wide open design space in parallel to the ongoing work to specify and build Phases 0 and 1.

To that end, there has been a flurry of recent activity of public calls, discussions, and ethresear.ch posts. There are some great resources to help get the lay of the land. The following is just a small sample:

* [Overview on State Provider Models](https://ethresear.ch/t/state-provider-models-in-ethereum-2-0/6750)
* [Open Questions wrt State Providers](https://ethresear.ch/t/remaining-questions-on-state-providers-and-stateless-networks-in-eth2/6585)
* [Moving ETH between shards: the problem statement](https://ethresear.ch/t/moving-eth-between-shards-the-problem-statement/6597)
* [Netted balance maps for cross-shard transfers](https://notes.ethereum.org/fkPBDSV_QiSePrrk5u-0Qg) \[and explainer for Vitalik's [meta-execution environment for ETH](https://ethresear.ch/t/an-even-simpler-meta-execution-environment-for-eth/6704)\]
* [Discussion of fee markets for Phase 1 shard data](https://ethresear.ch/t/phase-1-fee-market-and-eth1-eth2-bridging/6775)

In addition to Quilt and eWASM, the newly formed [TXRX](https://twitter.com/TXRXResearch) (ConsenSys research team) are dedicating a portion of their efforts toward Phase 2 research as well, initially focusing on better understanding cross-shard transaction complexity as well as researching and prototyping possible paths for the integration of eth1 into eth2.

All of the Phase 2 R&D is a relatively green field. There is a huge opportunity here to dig deep and make an impact. Throughout this year, expect more concrete specifications as well as developer playgrounds to sink your teeth into.

## Whiteblock releases libp2p gossipsub test results

This week, [Whiteblock](https://whiteblock.io/) released libp2p [gossipsub testing results](https://github.com/whiteblock/gossipsub-testing) as the culmination of a grant co-funded by [ConsenSys](consensys.net/) and the Ethereum Foundation. This work aims to validate the gossipsub algorithm for the uses of eth2 and to provide insight into the boundaries of performance to aid followup tests and algorithmic enhancements.

The tl;dr is that the results of this wave of testing look solid, but further tests should be performed to better observe how message propogation scales with network size. Check out the [full report](https://github.com/whiteblock/gossipsub-testing) detailing their methodology, topology, experiments, and results!

## Stacked Spring!

This Spring is stacked with exciting conferences, hackathons, eth2 bounties, and more! There will be a group of eth2 researchers and engineers at each of these events. Please come chat! We'd love to talk to you about engineering progress, validating on testnets, what to expect this year, and anything else that might be on your mind.

Now is a great time to get involved! Many clients are in the testnet phase so there are all sorts of tools to build, experiments to run, and fun to be had.

Here is a glimpse of the many events slated to have solid eth2 representation:

* [ETHDenver](https://www.ethdenver.com/) -- Denver, USA - Feb 14 to Feb 16 -- with eth2 specific bounties!
* [Stanford Blockchain Conference](https://cbr.stanford.edu/sbc20/) -- Stanford, USA - Feb 19 to Feb 21
* [Eth 222](https://www.eventbrite.com/e/eth-222-tickets-90667602239) -- Stanford, USA - Feb 22
* [ETHLondonUK](https://ethlondon.com/) -- London, UK - Feb 28 to March 1
* [ETHCC](https://ethcc.io/) -- Paris, France - March 3 to March 5
* [EDCON](https://www.edcon.io/) -- Vienna, Austria - April 3 to April 7
* [Ethereal Summit](https://www.etherealsummit.com/) -- New York, USA - May 8 to May 9
* [ETHNewYork](https://ethglobal.co) -- New York, USA - Dates TBD

🚀
