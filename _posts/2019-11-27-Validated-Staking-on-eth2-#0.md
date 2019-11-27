---
layout: post
published: true
title: 'Validated: Staking on eth2 #0'
date: '2019-11-27'
author: Carl Beekhuizen
---

![00-Merkle-4K](https://blog.ethereum.org/img/2019/11/00-Merkle-4K.png)

*Thanks to Danny and Joe for review.*

As the launch of the beacon chain grows nearer and eth2 becomes ever more final, the time has come to fast-sync the community with the latest on the inner workings of eth2 and on the concrete requirements, incentives and experience of being a validator. This article will provide a high-level overview of eth2 which will form the basis for a series on all aspects of eth2 relevant to validators.
eth2 has been in the works for a long time now and has improved dramatically over the years. What were initially separate sharding and Proof of Stake (PoS) efforts managed via smart contracts has transmogrified into a highly interconnected design which yields dramatic improvements regarding efficiency, scalability and security. 

## The phases

As parts of eth2 have become more interconnected, other pieces have been separated out into _phases_ to allow for better pipelining of the different aspects of eth2. At the time of writing, Phase 0 is nearing launch as developers put the finishing touches on the client software. Meanwhile, the specification for Phase 1 is being completed, and Phase 2 is under active R&D.

* **Phase 0** is concerned with the *beacon chain*, the core of eth2, which manages validators and the coordination of shards. The beacon chain is the source of ground truth from which all other aspects of eth2 are bootstrapped.
* **Phase 1** builds upon this by allowing data to be put into shards. The implementation complexity of this component is much smaller than the others as phase 0 lays most of the ground work for the shards.
* **Phase 2** adds execution to eth2 basically upgrading eth2 from a robust database to a fully decentralised computing platform.

## What exactly is Phase 0?

As mentioned previously, the beacon chain tracks the state of both the set of validators and the shards. In practice this means that if you (periodically) follow what is happening on the beacon chain, you will know enough to verify anything said to be happening within eth2. Trust, but verify.

In order for a PoS system to function, there needs to be consensus on who the validators are, and on what each of their stakes are in order to know how much their votes are worth, and to appropriately reward and/or punish them for their behaviour. The beacon chain also manages the sharding aspects of eth2 by assigning validator duties in the shards as well as tracking the current state of each shard.

Part of what differentiates eth2 from other PoS systems is the sheer number of validators that can participate in the protocol. In contrast to the 10s, 100s, and 1000s of participants that are possible in other systems, eth2 scales to hundreds of thousands or even millions of validators. This level of decentralisation is only possible due to the intermediate levels of consensus achieved by groups of validators called *committees*. The beacon chain uses the eponymous random beacon at its core to assign validators to committees which are tasked with evaluating what is and isn't a part of the beacon and shard chains. A committee's votes are then cryptographically aggregated into an _attestation_ meaning that verifying an entire committee's votes is only marginally more effort than checking a single vote. Therefore, to check the validity of the beacon chain, only a few aggregated signatures need to be considered to evaluate the votes of many validators.

The beacon chain also tracks the eth1 chain and the deposits thereupon so that new validators can join eth2 by sending 32 Ether to the deposit contract on eth1. As a result of the beacon chain voting on the eth1 chain, eth2 will, at some point in the future, enhance the security of eth1 by providing an economic guarantee that blocks that are a part of the canonical eth1 chain.

## Nodes vs. Clients
eth2 makes the distinction between beacon nodes and validator clients, and validators will need both in order to perform their duties. A *beacon node* (or just *node*) concerns itself with maintaining a view of the beacon chain as well as whichever shards may be needed by a user or validator. 

As their name suggests, *validator clients* (or just *clients*) handle the logic of a single validator. This is achieved by communicating with the beacon node to understand the current state of the chain, by attesting to and proposing blocks as well when appropriate, and finally by asking the beacon node to send this information on to its peers. 

> If you are not running a validator, a beacon node contains all of the information you need to trustlessly interact with eth2, much like a full node in eth1.

![carlimg1](https://blog.ethereum.org/img/2019/11/carlimg1.png)

Below are some of the many arguments for this separation:

* Each validator needs to be initiated with a deposit of exactly 32 Ether and therefore people who wish to stake more ETH will need to run multiple validator instances. The node-client separation allows such users to only run a single beacon node with multiple validators connected to it thereby reducing computation, memory, and storage requirements.
* By having validator nodes be separate modules, they will likely be more secure as it is easier to write, reason about, and audit smaller code modules.
* For users particularly worried about redundancy, multiple nodes can be run in parallel, thus reducing the chance of a validator going offline.
* Because validator clients can only interact with the rest of the eth2 network via a beacon node, and even then via a [restricted API](https://github.com/ethereum/eth2.0-APIs/blob/master/apis/validator/beacon-node-validator-api.md), the attack surface of a validator node is greatly reduced.
* For users who wish to interact with eth2, but don't want to be a validator, they need only operate a beacon node which will grant them access to the beacon chain and all the shards they require.

## Design Philosophy

The design philosophy of eth2 provides useful context for all the decisions made within eth2 and in many instances encapsulate the differences between eth2 and other protocols.

* **Protocol über alles:** With the acknowledgement that everything is a trade off, the protocol's safety and liveness take precedence over other design desiderata.
* **Hope for the best, but expect the worst:** eth2 assumes validators will be lazy, take bribes, and that they will try to attack the system unless they are otherwise incentivised not to. Furthermore, the network is assumed not to be entirely reliable and that catastrophic events could force large numbers of validators to go offline. For these reasons, eth2 should be capable of surviving World War 3.
* **Minimally viable complexity:** Wherever possible, eth2 has been simplified as this makes it easier to reason about, explain to others, audit, write bug free clients, and generally avoid edge cases.
* **Maximally decentralised:** Proof of stake protocols commonly compromise on the number of validators that can participate, eth2 is designed to scale to millions of validators while encouraging these validators to work independently of one another.
* **Expect the unexpected:** All components of eth2 are resistant to quantum computers or can be swapped out for those that are in the event of a quantum apocalypse.
* **By the people for the people:** eth2 must be able to run on a consumer laptop. The lower the barrier to entry, the more people who can participate which translates into a higher degree of decentralisation.

## Wrapping up

Now that you have the basics of eth2 under your belt, the next posts in this series will tackle the juicy details of what makes eth2 tick.
