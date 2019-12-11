---
layout: post
published: true
title: 'The 1.x Files: a fast-sync'
date: '2019-12-10'
author: Griffin Ichiba Hotchkiss
---

![The 1.x Files](https://blog.ethereum.org/img/2019/12/the1xfiles_black.png "The truth is out there.")

# ETH 1.x: a fast sync

The [new direction of ETH 1.x research](https://medium.com/@pipermerriam/stateless-clients-a-new-direction-for-ethereum-1-x-e70d30dc27aa) has begun proper, with a focus on moving the current Ethereum chain towards the 'stateless client' paradigm, with the eventual target being a smooth transition into an Eth 2.0 Execution Environment.

The next call will be focused on collecting and organizing research topics and planning a more structured roadmap. The call is open for anyone to attend, and is scheduled for December 17th at 16:00 UTC -- if you would like to join, please DM Piper Merriam or James Hancock  on the [ethresear.ch forum](https://ethresear.ch/t/eth1-x-call-2-tuesday-dec-17th-next-steps-and-collecting-research-topics/6512).

This post is a re-cap of everything that's brought us to where we are now, and may be resource for anyone that may have recently joined the Ethereum community, missed the Ethereum 1.x discussions as they happened, or is in need of a little memory refresh.

In the spirit of `--sync-mode=fast`, we'll be  touching on most of the historical topics of research, and save the in-depth look into stateless clients and current research for a subsequent post.

Our story begins with a realization by core developers that the final phase of the Ethereum roadmap, "Serenity", would not be ready as early as originally hoped. With potentially many years before a full "Ethereum 2.0" roll-out, the current chain would need changes to ensure that larger problems that wouldn't render Ethereum in-operable before a comprehensive protocol upgrade could be delivered. Hence, "Ethereum 1.*x*" -- research into smaller, incremental upgrades to current Ethereum (1.0) -- was born with the task of prolonging the life of the chain for at least another 3-5 years, before a more dramatic upgrade to Serenity (Eth 2.0) arrives.

## What's the problem?

It's complicated. Unlike a security vulnerability or major design flaw, there is no single pressing issue that we can identify with Ethereum 1.0 and put forward focused resources in order to correct. Similarly, if things are left entirely un-touched, there will likely be no one dramatic event that causes the network to halt and catch fire 🔥.

Rather, the ETHpocalypse scenario arose from small, subtle degradations of performance and diminishing network health as a result of natural chain growth. Without 1.x efforts, over time Ethereum runs the risk of becoming more centralized as it becomes harder to run full nodes, slower as network latency increases and block verification gets harder due to state bloat, and ultimately too frustrating for end users and core developers alike as transaction throughput hits an upper limit and client improvements become harder to implement. The goal then was to avoid a death by a thousand cuts scenario that would take years to play out and be recognized too late by beginning to plan immeditely, beginning at Devcon4 in Prague (🦄 > 💀).

Broadly speaking, the issues at hand are all aspects of one fundamental and unremarkable reality: _The blockchain just keeps getting bigger,_ but there's some nuance here, and when we talk about "the size of the blockchain", we are really talking about the size of a few different sub-components, and more importantly about how their size affects the performance of the network.

Let's cover them one by one!

### Chain storage

>> _"If anyone so much as utters a word about &quot;storage costs of blockchain,&quot; just send them to the Amazon Black Friday web page. 8TB for $125. There are real problems blockchains face. Storage costs are not one of them.
--Emin Gün Sirer ([@el33th4xor](https://twitter.com/el33th4xor/status/1200477778463907841))_

Before a full node can become a first-class citizen of Ethereum, it must sync the entire history of the blockchain. The longer that history is, the more data there is to store. Currently, storage requirements are about 219 GB for a 'normal' full node in both parity and geth, and growing by 10-15 GB every month.

This isn't *too bad*, from an absolute cost-of-storage perspective. It has always been the vision of Ethereum to run entirely on consumer hardware, and excluding archive nodes (which require ~3.5 TB), under 500GB is well within a reasonable threshold, so running a full node won't be out-of-reach for another couple of years. The stronger argument to be made concerns the *marginal cost* of spinning up new full nodes: Increasing storage requirements and sync times lead to fewer full nodes, which leads to even longer syncing times, and fewer nodes still.

Over time, developers will lean more and more on  services like Infura, and the 'real' blockchain will be increasingly stuck up in the cloud, out of reach for average hobbyists, researchers, and casual developers.

### Block size and transaction throughput

A different aspect of growth is the size of individual blocks, and their relationship to total transaction throughput. Unlike Bitcoin, Ethereum does not explicitly limit the size of a block by memory, but enforces the block size through a *gas limit*. The gas limit in Ethereum effectively caps the number of transactions that can be included in a block, and is decided collectively by miners, with a vote to increase or decrease the gas limit dynamically. Recently, miners collectively agreed to increase the block gas limit to around 10 million gas units, making each block about 25% larger than it had been since Jan '18' -- and, by extension, boosting theoretical transaction throughput.

There is a trade-off between the block gas limit and the ability of miners to reach consensus on new blocks. Larger gas limits theoretically will increase the rate of block uncles (valid blocks that don't propagate to other miners quickly enough to be accepted by a majority). More data needs to be collected on what a 'safe' upper bound is for block sizes, but it's generally accepted that throughput gains to be had from increasing the gas limit are not going to be sufficient for Ethereum's growth in the next 5 years. Additionally, bigger block sizes accelerate the chain storage requirement problem.

### State size and Network Performance

Ethereum is a [state machine that moves forward one step with each block](https://medium.com/@preethikasireddy/how-does-ethereum-work-anyway-22d1df506369). At any given moment, the complete 'state' of Ethereum comprises the collective memories of all smart contracts deployed and running in the EVM, as well as the current status of all accounts and balances. When transactions are added to a block, they modify the state by changing the balances of accounts, deploying new smart contract code, or by causing a smart contract to execute some of its code.  

The total size of state currently weighs in on the order of 10GB. It stands to reason that the state grows proportionally with the total transaction volume on the network, so if we expect Ethereum to continue to gain mainstream adoption, that number could grow by an order of magnitude in the years to come.

A larger state affects all clients along two major points of performance:

* **Slower transaction processing due to limits of clients reading from state**. Processing a transaction requires reading the relevant part of the state stored in the client's database. The larger the state, the longer it takes to lookup the transaction. Importantly, in clients that use a trie structure to represent state (parity, geth, trinity), this slowdown is compounded by the underlying database lookup (in which the trie is implemented).
* **Slower block verification due to constructing new state from modifications**. Along the same lines of reasoning as above, when a new block is verified the changes to state must be re-computed by the client; this involves building a new state trie and computing a new root hash. Constructing a new state trie is more computationally intensive than a simple lookup, so this operation is more dramatically affected by state growth than processing a single transaction.

State-driven performance degradation is most worrying. Ethereum is a peer to peer network, which means that subtle changes can have cascading effects on network health. Furthermore, state storage and modification is one of the more difficult things to implement for client developer teams. Writing and maintaining clients is already hard enough, and state growth adds to that burden. As the state grows, the diversity and performance of clients will diminish, which is bad for everyone.

## What are the potential solutions?

Starting with the initial meeting in Prague, and continuing through 2019, various core developers, contributors, and magicians have gathered both on-line and IRL to discuss the best ways of extending the life of the 1.0 chain. Here are the most important proposals discussed and what they entail:

### Modest  optimizations and mitigations

* **More aggressive pruning**. One way to manage storage requirements is to actively delete pieces of the chain that are no longer needed, such as transaction receipts, logs, and older historical blocks. An agreed upon time period (3-9 months) of historical data would be kept by full nodes, and then deleted after it expired, effectively capping the total storage needed to run a node. Péter Szilágyi provided a [comprehensive overview](https://gist.github.com/karalabe/60be7bef184c8ec286fc7ee2b35b0b5b#decentralized-archives) of chain pruning effects for long-term viability. TL;DR -- there are trade-offs, and one unsolved requirement is that historical data be available (somewhere), *and* in lieu of full chain history, nodes must maintain proofs for deleted chain segments.
* **Block pre-announcement and state caching**. These relate to mitigating the effects of network latency. In block pre-announcement, the idea is that a miner announces a new block *before it is validated*, which gives listening clients a chance to guess at which parts of state will be affected and preemptively warn those caches for the next state. Similarly, clients could hold partial states in memory so that they don't have to start from scratch again if syncing the state fails. These optimizations are within reach currently, and variations on this theme are already employed by turbo-geth to improve performance.

### Big, hard-forking changes

* **Opcode re-pricing and ETH lockups** . Generally, this means simply tuning the costs of opcodes further discourage state growth. Broadly, this means increasing the cost of operations that grow state, and/or increasing the rewards for operations that shrink state. Refunds, however, are a bit tricky, because they must come from gas included with the transaction -- this means that transactions which *only* clear memory or destruct contracts can't actually receive proportional refunds. In order to have transactions that make more in gas than they spend, it would be possible to require contracts to lock up a bit of ETH when deployed, enough to cover those refunds.

* **State rent and 'eviction'**. More dramatic than the above opcode price changes, state rent concerns directly reducing the size of state by requiring that contracts pay a recurring fee proportional to their share of the state size. The contract would be deleted or halted until the fee is paid. This would be a major, breaking change to smart contracts and dapp developers, and would require more than one hard-fork to implement. It remains to date the most extensively discussed proposal in the category of 1.x, as well as the most controversial. Consequently, research into state rent on the 1.0 chain has been suspended.

## The new direction: ✨Stateless Clients✨

If it's the size of state causing the biggest problems for network health, the ultimate solution would be to do away with the need for state altogether. In a nutshell, a stateless client makes use of a *block witness*, which proves the validity of a given state change against the previous state. That is to say, rather than computing a complete state with each new block, clients simply compute the *changes* to state for a new block, and then prove that those changes are consistent with the previous block. Miners and some full nodes will still need to keep a full copy of state for witnesses to be generated from, and the need for block witnesses to be gossiped around the network introduces some new challenges for clients, but the potential benefits of this change are vast.

*Note: This is still very early stage research and shouldn't be regarded as an accepted part of the Ethereum roadmap or in any way 'proven' as a concept. Stateless clients have many major technical hurdles to overcome, all of which will be elucidated in subsequent updates as research continues.*

[The stateless client concept](https://ethresear.ch/t/the-stateless-client-concept/172) first appeared in the Ethereum landscape in a post by Vitalik in the context of sharding, but was also discussed later during Eth 1.x discussions; at the time it was thought too complex to implement. More recently, however, the stateless client concept has gained support as [Trinity's beam sync](https://medium.com/@jason.carver/intro-to-beam-sync-a0fd168be14a) demonstrates the feasibility of semi-statelessness for light clients.

Importantly, moving towards a stateless or semi-stateless paradigm is less disruptive to the existing network than something like state rent because it does not inherently create breaking changes for existing clients. Stateful nodes and stateless light clients can exist side-by-side, and the introduction of semi-stateless Ethereum offers more opportunity for experimentation with different client implementations. As icing on the layer-cake, shards on Eth 2.0 will almost certainly be stateless, which opens up a new path toward an eventual migration to Serenity when it's ready for the prime-time.

*We'll leave a deeper dive into stateless clients for another post. If you made it this far, you're now caught up with the current state of Ethereum 1.x research, and should be able to follow along and join in on new developments as they happen! Join us at ethresear.ch, or stay tuned here for the next edition of 'the 1.x files' :)*
