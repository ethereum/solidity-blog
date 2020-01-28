---
layout: post
published: true
title: 'The 1.x Files: The Stateless Ethereum Tech Tree'
date: '2020-01-28'
author: Griffin Ichiba Hotchkiss
---
![The 1.x Files](https://blog.ethereum.org/img/2019/12/the1xfiles_black.png "The truth is out there.")

I started to write a post that detailed a "roadmap" for Ethereum 1.x research and the path to stateless Ethereum, and realized that it's not actually a roadmap at all —— at least not in the sense we're used to seeing from something like a product or company. The 1.x team, although working toward a common goal, is an eclectic collection of developers and researchers *independently* tackling intricately related topics. Consequently, there is no "official" roadmap to speak of. It's not complete chaos though! There is an understood "order of operations"; some things must happen before others, certain solutions are mutually exclusive, and other work might be beneficial but non-essential.

So what's a better metaphor for the way we get to stateless Ethereum, if not a roadmap? It took me a little bit, but I think I have a good one: Stateless Ethereum is the 'full spec' in a [tech tree](https://en.wikipedia.org/wiki/Technology_tree).

Some readers might immediately understand this analogy. If you "get it", feel free to skip the next few paragraphs. But if you're not like me and don't ordinarily think about the world in terms of video games: A tech tree is a common mechanic in gaming that allows players to unlock and upgrade new spells, technologies, or skills that are sorted into a loose hierarchy or tree structure.

![KSP Tech Tree "yes, this is the real state of my campaign in Kerbal Space Program."](/img/2020/01/KSP_tree.png)

Usually there is some sort of XP (experience points) that can be "spent" to acquire elements in the tree ('spec'), which in turn unlock more advanced elements. Sometimes you need to acquire two un-related basic elements to access a third more advanced one; sometimes unlocking one basic skill opens up multiple new choices for the next upgrade. Half the fun as a player is choosing the right path in the tech trie that matches your ability, goals, and preferences (do you aim for full spec in Warrior, Thief, or Mage?).

That's, in surprisingly accurate terms, what we have in the 1.x research room: A loose hierarchy of technical subjects to work on, with limited time/expertise to invest in researching, implementing, and testing. Just as in a good RPG, experience points are finite: there's only so much that a handful of capable and motivated humans can accomplish in a year or two. Depending on the requirements of delivery, it might be wise to hold off on more ambitious or abstract upgrades in favor of a more direct path to the final spec. Everyone is aiming for the same end goal, but the path taken to get there will depend on which solutions end up being fully researched and employed.

Ok, so I'll present my rough drawing of the tree, talk a little about how it's arranged, and then briefly go into an explanation of each upgrade and how it relates to the whole. The final "full-spec" upgrade in the tech tree is "Stateless Ethereum". That is to say, a fully functioning Ethereum mainnet that supports full-state, partial-state, and zero-state nodes; that efficiently and reliably passes around witnesses and state information; and that is in principle ready to continue scaling until the bridge to Eth2.0 is built and ready to onboard the legacy chain.

![The Tech Tree](/img/2020/01/Stateless_tech_tree_v2.jpg)

*Note: As I said just above, this isn't an 'official' scheme of work. It's my best effort at collating and organizing the key features, milestones, and decisions that the 1x working group must settle on in order to make Stateless Ethereum a reality. Feedback is welcome, and updated/revised versions of this plan will be inevitable as research continues.*

You should read the diagram from left to right: purple elements presented on the left side are 'fundamental' and must be developed or decided upon before subsequent improvements further right. Elements with a greenish hue are colored so to indicate that they are in some sense "bonus" items -- desirable though not strictly necessary for transition, and maybe less concretely understood in the scope of research. The larger pink shapes represent essential milestones for Stateless Ethereum. All 4 major milestones must be "unlocked" before a full-scale transition to Stateless Ethereum can be enacted.

## The Witness Format

There has been a lot of talk about witnesses in the context of stateless Ethereum, so it should come as no surprise that the first major milestone that I'll bring up is a finalized witness format. This means deciding with some certainty the structure of the state trie and accompanying witnesses. The creation of a specification or reference implementation could be thought of as the point at which ETH 1.x research "levels up"; coalescing around a new representation of state will help to define and focus the work needed to be done to reach other milestones.

![Witness Format](/img/2020/01/tree_witness_format.jpg)

### Binary Trie (or "trie, trie again")

Switching Ethereum's state to a Binary Trie structure is key to getting witness sizes small enough to be gossiped around the network without running into bandwidth/latency issues. As outlined in the [last research call](https://blog.ethereum.org/2020/01/17/eth1x-files-digest-no-2/), getting to a Binary Trie will require a commitment to one of two mutually exclusive strategies:

* **Progressive**. Like [the Ship of Theseus](https://en.wikipedia.org/wiki/Ship_of_Theseus), the current hexary state trie woud be transformed piece-by-piece over a long period of time. Any transaction or EVM execution touching parts of state would by this strategy automatically encode changes to state into the new binary form. This implies the adoption of a 'hybrid' trie structure that will leave dormant parts of state in their current hexary representation. The process would effectively never complete, and would be complex for client developers to implement, but would for the most part insulate users and higher-layer developers from the changes happening under the hood in layer 0.

* **Clean-cut**. Perhaps more aligned with the significance of the underlying trie change, a clean-cut transition strategy would define an explicit time-line of transition over multiple hard forks, compute a fresh binary trie representation of the state at that time, then carry on in binary form once the new state has been computed. Although more straightforward from an implementation perspective, a clean-cut requires coordination from all node operators, and would almost certainly entail some (limited) disruption to the network, affecting developer and user experience during the transition. On the other hand, the process might provide some valuable insights for planning the more distant transition to Eth2.

Regardless of the transition strategy chosen, a binary trie is the basis for the witness structure, i.e. the order and hierarchy of hashes that make up the state trie. Without further optimization, rough calculations (January 2020) put witness sizes in the ballpark of ~300-1,400 kB, down from ~800-3,400 kB in the hexary trie structure.

### Code Chunking (merkleization)

One major component of a witness is accompanying code. Without code chunking, A transaction that contained a contract call would require the full bytecode of that contract in order to verify its codeHash. That could be a lot of data, depending on the contract. Code 'merkleization' is a method of splitting up contract bytecode so that *only the portion of the code called* is required to generate and verify a witness for the transaction. This is one technique of dramatically reducing the average size of witnesses. There are two ways to split up contract code, and for the moment it is not clear the two are mutually exclusive.

* **"Static" chunking**. Breaking contract code up into fixed sizes on the order of 32 bytes. For the merkleized code to run correctly, static chunks also would need to include some extra meta-data along with each chunk.
* **"Dynamic" chunking**. Breaking contract code up into chunks based on the content of the code itself, cleaving at specific instructions (JUMPDEST) contained therein.

At first blush, the "static" approach in code chunking seems preferable to avoid leaky abstractions, i.e. to prevent the content of the merkleized code from affecting the lower-level chunking, as might happen in the "dynamic" case. That said, both options have yet to be thoroughly tested and therefore both remain in consideration.

### ZK witness compression

About 70% of a witness is hashes. It might be possible to use a ZK-STARK proofing technique to compress and verify those intermediate hashes. As with a lot of zero-knowledge stuff these days, exactly *how* that would work, or even *that it would work at all* is not well-defined or easily answered. So this is in some sense a side-quest, or non-essential upgrade to the main tech development tree.

## EVM Semantics

We've touched briefly on "leaky abstraction" avoidance, and it is most relevant for this milestone, so I'm going to take a little detour here to explain why the concept is important. The EVM is an **abstracted** component part of the bigger Ethereum protocol. In theory, details about what is going on inside the EVM should have no effect *at all* on how the larger system behaves, and changes to the system outside of the abstraction should have no effect at all on anything within it.

In reality, however, there are certain aspects of the protocol that do directly affect things inside the EVM. These manifest plainly in gas costs. A smart contract (inside the EVM abstraction) has exposed to it, among other things, gas costs of various stack operations (outside the EVM abstraction) through the `GAS` opcode. A change in gas scheduling might directly affect the performance of certain contracts, but it depends on the context and how the contract makes use of the information to which it has access.

Because of the 'leaks', changes to gas scheduling and EVM execution need to be made carefully, as they could have unintended effects on smart contracts. This is just a reality that must be dealt with; it's very difficult to design systems with zero abstraction leakage, and in any event the 1.x researchers don't have the luxury of redesigning anything from the ground up -- They need to work within today's Ethereum protocol, which is just a wee bit leaky in the ol' virtual state machine abstraction.

Returning to the main topic: The introduction of witnesses **will** require changes to gas scheduling. Witnesses need to be generated and propagated across the network, and that activity needs to be accounted for in EVM operations. The topics tied to this milestone have to do with what those costs and incentives are, how they are estimated, and how they will be implemented with minimal impact on higher layers.

![EVM Semantics](/img/2020/01/tree_EVM.jpg)

### Witness Indexing / Gas accounting

There is likely much more nuance to this section than can reasonably fit in a few sentences; I'm sure we'll dive a bit deeper at a later date. For now, understand that every transaction will be responsible for a small part of the full block's witness. Generating a block's witness involves some computation that will be performed by the block's miner, and therefore will need to have an associated gas cost, paid for by the transaction's sender.  

Because multiple transactions might touch *the same part of the state*, it's not clear the best way to estimate the gas costs for witness production at the point of transaction broadcast. If transaction owners pay the full cost of witness production, we can imagine situations in which the same part of a block witness might be paid for many times over by 'overlapping' transactions. This isn't obviously a bad thing, mind you, but it introduces real changes to gas incentives that need to be better understood.

Whatever the associated gas costs are, the witnesses themselves will need to become a part of the Ethereum protocol, and likely will need to incorporated as a standard part of each block, perhaps with something as straightforward as a `witnessHash` included in each block header.

### UNGAS / Versionless Ethereum

This is a class of upgrades mostly orthogonal to Stateless Ethereum that have to do with gas costs in the EVM, and patching up those abstraction leaks I mentioned. UNGAS is short for "unobservable gas", and it is a modification that would explicitly disallow contracts from using the `GAS` opcode, to prohibit any assumptions about gas cost from being made by smart contract developers. UNGAS is part of a number of suggestions from the [Ethereum core paper](https://ethereum.corepaper.org/#proposals) to patch up some of those leaks, making *all future changes* to gas scheduling easier to implement, including and especially changes related to witnesses and Stateless Ethereum.

## State Availability

Stateless Ethereum is not going to do away with state entirely. Rather, it will make state an optional thing, allowing clients some degree of freedom with regard to how much state they keep track of and compute themselves. The full state therefore must be made available *somewhere*, so that nodes looking to download part of all of the state may do so.

In some sense, existing paradigms like fast sync already provide for this functionality. But the introduction of zero-state and partial-state nodes complicates things for new nodes getting up to speed. Right now, a new node can expect to download the state from any healthy peers it connects to, because all nodes keep a copy of the current state. But that assumption goes out the window if some of peers are potentially zero-state or partial-state nodes.  

The pre-requisites for this milestone have to do with the ways nodes signal to each other what pieces of state they have, and the methods of delivering those pieces reliably over a constantly changing peer-to-peer network.

![State Availability](/img/2020/01/tree_State_Availability.jpg)

### Network Propagation Rules
This diagram below represents a hypothetical network topology that could exist in stateless Ethereum. In such a network, nodes will need to be able to position themselves according to what parts of state they want to keep, if any.

![semi-stateless-topology](https://blog.ethereum.org/img/2019/12/semi-stateless-topology.jpg)

Improvements such as [EIP #2465](https://github.com/ethereum/EIPs/issues/2465) fall into the general category of network propagation rules: New message types in the network protocol that provide more information about what information nodes have, and define how that information is passed to other nodes in potentially awkward or limited network topologies.

### Data Delivery Model / DHT routing

If improvements like the message types described above are accepted and implemented, nodes will be able to easily tell what parts of state are held by connected peers. What if none of the connected peers have a needed piece of state?

Data delivery is a bit of an open-ended problem with many potential solutions. We could imagine turning to more 'mainstream' solutions, making some or all of the state available over HTTP request from a cloud server. A more ambitious solution would be to adopt features from related peer-to-peer data delivery schemes, allowing requests for pieces of state to be proxied through connected peers, finding their correct destinations through a [Distributed Hash Table](https://en.wikipedia.org/wiki/Distributed_hash_table). The two extremes aren't inherently incompatible; Porque no los dos?

### State tiling

One approach to improving state distribution is to break the full state into more manageable pieces (tiles), stored in a networked cache that can provide state to nodes in the network, thus lightening the burden on the full nodes providing state. The idea is that even with relatively large tile sizes, it is likely that some of the tiles would remain un-changed from block to block.

The geth team has performed some experiments which suggest state tiling is feasible for improving the availability of state snapshots.

### Chain pruning

[Much has been written](https://gist.github.com/karalabe/60be7bef184c8ec286fc7ee2b35b0b5b) on chain pruning already, so a more detailed explanation is not necessary. It is worth explicitly stating, however, that full nodes can safely prune historical data such as transaction receipts, logs, and historical blocks *only if historical state snapeshots can be made readily available to new full nodes*, through something like state tiling and/or a DHT routing scheme.

## Network Protocol Spec

At last, the complete picture of Stateless Ethereum is coming into focus. The three milestones of Witness Format, EVM Semantics, and State Availability together enable a complete description of a Network Protocol Specification: The well-defined upgrades that should be coded into every client implementation, and deployed during the next hard fork to bring the network into a stateless paradigm.

We've covered a lot of ground in this article, but there are still a few odd and ends from the diagram that should be explained:

### Formal Stateless Specification
At the end of the day, it is not a *requirement* that the complete stateless protocol be formally defined. It is plausible that a reference implementation be coded out and used as the basis for all clients to re-implement. But there are undeniable benefits to creating a "formalized" specification for witnesses and stateless clients. This would be essentially an extension or appendix that would fit in the Ethereum Yellow Paper, detailing in precise language the expected behavior of an Ethereum stateless client implementation.

### Beam Sync, Red Queen's sync, and other state sync optimizations

Sync strategies are not primary to the network protocol, but instead are implementation details that affect how performant nodes are in enacting the protocol. Beam sync and Red Queen's sync are related strategies for building up a local copy of state from witnesses. Some effort should be invested in improving these strategies and adapting them for the final 'version' of the network protocol, when that is decided and implemented.

For now, they are being left as 'bonus' items in the tech tree, because they can be developed in isolation of other issues, and because details of their implementation depend on more fundamental choices like witness format. Its worth noting that these extra-protocol topics are, by virtue of their independence from 'core' changes, a good vehicle for implementing and testing the more fundamental improvements on the left side of the tree.


## Wrapping up

Well, that was quite a long journey! I hope that the topics and milestones, and general idea of the "tech tree" is helpful in organizing the scope of "Stateless Ethereum" research.

The structure of this tree is something I hope to keep updated as things progress. As I said before, it's not an 'official' or 'final' scope of work, it's just the most accurate sketch we have at the moment. Please do reach out if you have suggestions on how to improve or amend it.

As always, if you have questions, requests for new topics, or want to participate in stateless Ethereum research, come introduce yourself on ethresear.ch, and/or reach out to @gichiba or @JHancock on twitter.
