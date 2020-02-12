---
layout: post
published: true
title: 'Validated, staking on eth2: #2 - Two ghosts in a trench coat'
date: '2020-02-12'
author: Carl Beekhuizen
category: 'Research & Development'
---

![00-Merkle-4K](https://blog.ethereum.org/img/2019/11/00-Merkle-4K.png)

*Special thanks to Sacha Yves Saint-Leger & Danny Ryan for review.*

In this installment, we'll discuss the consensus mechanisms behind eth2. Eth2 has a novel approach to deciding which block is the head of the chain, along with which blocks _are_ and _are not_ a part of the chain.

By using a hybrid between the two mechanisms, eth2 aims to have a consensus which, in addition to being rapid and safe when the network is behaving normally, remains safe even when it’s being attacked.

## A Trilemma

[FLP impossibility](https://groups.csail.mit.edu/tds/papers/Lynch/jacm85.pdf) is a core result in the field of distributed computation which states that in a distributed system it is not possible to **simultaneously** have safety, liveness, and full asynchrony unless some unreasonable assumptions can be made about your system.

*Safety* is the idea that decisions cannot be unmade whereas *liveness* captures the notion that new things can be decided. A protocol is *asynchronus* if there is no bound on how long a message may take to get delivered.

![FLP Trilemma](https://storage.googleapis.com/ethereum-hackmd/upload_01a6db9261c1dbb5bcf81da3ae34ffee.png)


If nodes could communicate reliably, always follow the protocol honestly and never crash, then consensus would be easy, but that is not how the world works. When these assumption don't hold, FLP Impossibility is the proof that at least one of: safety, liveness, or full asynchrony must be compromised.

## GHOSTs and their opinions on forks

Eth2 uses [Greedy Heaviest Observed Subtree (GHOST)](https://eprint.iacr.org/2013/881.pdf) as its fork-choice rule. GHOST selects the head of the chain by choosing the fork which has the most votes (it does this by considering all of the votes for each fork block and their respective child blocks).

[Put another way](https://vitalik.ca/general/2018/12/05/cbc_casper.html), each time there is a fork, GHOST chooses the side where more of the latest messages support that block’s subtree (i.e. more of the latest messages support either that block or one of its descendants). The algorithm does this until it reaches a block with no children.

GHOST has the benefit of reducing the efficacy of attacks during times of high network latency as well as minimizing the depth of chain reorgs when compared to the longest-chain rule. This is because while an attacker can keep building blocks efficiently on their own chain thereby making it the longest, GHOST would choose the other fork as there are more votes for it in total.

In particular, eth2 uses a variation of GHOST which has been adapted to a PoS context called Latest Message Driven GHOST (LMD-GHOST). The idea behind LMD-GHOST is that when calculating the head of the chain, one only considers the _latest_ vote made by each validator, and not any of the votes made in the past. This dramatically decreases the computation required when running GHOST, since the number of forks that need to be considered to execute the fork choice cannot be greater than the number of validators ($O(v)$ in Big O notation).

Under the rules of GHOST, validators/miners can always try to add a new block to the blockchain (liveness), and they can do this at any point in the chain’s history (asynchronous). Since it is live and fully asynchronous, thanks to our friend FLP, we know it can’t be safe.

![GHOST Favours liveness over safety](https://storage.googleapis.com/ethereum-hackmd/upload_fb52a9383b8a39ae63929bb4b1c9436e.png)


The lack of safety presents itself in the form of reorgs where a chain can suddenly switch between forks of abitrary depth. Obviously this is undesirable and eth1 deals with this by having users make assumptions about how long miners' blocks will take to be communicated with the rest of the network, this takes the form of waiting for $x$ confirmations. Eth2, by contrast, makes no such assumptions.

### The friendly finality gadget

A blockchain without any notion of safety is useless because no decisions could be reached and users could not agree on the state of the chain. Enter [Casper the Friendly Finality Gadget (Casper FFG)](https://arxiv.org/pdf/1710.09437.pdf). Casper FFG is a mechanism which favours safety over liveness when making decisions. This means that while the decisions it makes are final, under poor network conditions, it may not be able to decide on anything.

![](https://storage.googleapis.com/ethereum-hackmd/upload_84ad2520a6a3370e4e841a969a5f1f62.png)


FFG is a crypto-economic adaption of the classic [Practical Byzantine Fault Tolerent (PBFT)](http://pmg.csail.mit.edu/papers/osdi99.pdf) which has phases where nodes first indicate that they'd like to agree on something (*justification*) and then agree that they've seen each other agreeing (*finalisation*).

Eth2 does not try to justify and finalise every slot (the time when a block is expected to be produced), but instead only every 32 slots. Collectively, 32 slots is called an *epoch*. First, validators sign that they agree with all 32 blocks in an epoch. Then, if $\geq \frac{2}{3}$ do so, the block is justified. In a later epoch, validators get another chance to vote to indicate that they have seen the earlier justified epoch and if $\geq \frac{2}{3}$ do this, the epoch is finalised and is forever a part of the eth2 chain.

FFG employs a clever trick. Votes actually consist of two sub-votes, one for the epoch that is attemping to be justified and another for an earlier epoch that is to become finalised. This saves a lot of extra communication between nodes and helps to achieve the goal of scaling to millions of validators.

## Two ghosts in a trench coat

Consensus within eth2 relies on both LMD-GHOST – which adds new blocks and decides what the head of the chain is – and Casper FFG which makes the final decision on which blocks _are_ and _are not_ a part of the chain. GHOST’s favourable liveness properties allow new blocks to quickly and efficiently be added to the chain, while FFG follows behind to provide safety by finalising epochs.

<p align="center">
   <img width="350" src="https://storage.googleapis.com/ethereum-hackmd/upload_84663daf63bd4ea84dc8dacce31625d0.png">
</p>

The two protocols are merged by running GHOST from the last finalised block as decided upon by FFG. By construction, the last finalised block is always a part of the chain which means GHOST doesn't need to consider earlier blocks.

In the normal case when blocks are being produced and $\geq \frac{2}{3}$  validators are voting on them, these blocks are added to the head of the chain by GHOST, and not long after justified and finalised by FFG (which considers the last few epochs).

If there is an attack on the network and/or a large porportion of validators go offline, then GHOST continues adding new blocks. However, since GHOST is live, but not safe, it may change its mind about the head of the chain – this is because new blocks are continually added to the chain, which means nodes keep learning new information. FFG on the other hand, favours safety over liveness meaning that it stops finalising blocks until the network is stable enough for validators to vote consistently again.
