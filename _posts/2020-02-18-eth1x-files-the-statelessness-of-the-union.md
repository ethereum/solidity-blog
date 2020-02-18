---
layout: post
published: true
title: 'The 1.x Files: The State(lessness) of the Union'
date: '2020-02-18'
author: Griffin Ichiba Hotchkiss
category: 'Research & Development'
---

![The 1.x Files](https://blog.ethereum.org/img/2019/12/the1xfiles_black.png "The truth is out there.")

The next Stateless Ethereum research call is coming up in less than a week! The telegram chat now has hundreds of messages to catch up on, and only a small portion of the topics discussed have made it into the ethresearch forums.

This post is just a quick primer and general update; we'll re-cap most issues under discussion currently, and set the stage for call #4 as well as the 1.x researcher's summit following EthCC.

This post assumes a higher-than-average technical understanding of the concepts at play, because there is a lot to catch up on. If you are coming in fresh, I recommend reading [The Stateless Ethereum Tech Tree](https://blog.ethereum.org/2020/01/28/eth1x-files-the-stateless-ethereum-tech-tree/) for a more thorough context.

It may be helpful to refer to this diagram from the linked post above:

![Tech tree](https://blog.ethereum.org/img/2020/01/Stateless_tech_tree_v2.jpg)

# The General Scope

Stateless Ethereum is an initiative to upgrade the current Ethereum protocol to allow for "stateless" client paradigm, in which not all nodes are required to keep a current copy of the state trie. Instead, stateless nodes rely on a "witness" produced by a full state node in order to execute new blocks.

Witnesses are the central conceptual object in stateless discussions, and most stateless research is along two main lines of inquiry:

* Witness format and implementation
* State and Witness data availability

In the former, we have questions about the structure of witnesses themselves, changes required to the state trie format, questions about client database implementations, and optimizations to bound and shrink hypothetical witness size.

In the latter are less understood questions about network topology, gossip protocols for efficiently passing around witnesses, and quantitative questions about network performance and incentives.

## Known Knowns

To get where we want to go, some things will all-but-certainly need to happen. These are in detail outlined in Vitalik's post on [Protocol Changes to bound Witness Size](https://ethereum-magicians.org/t/protocol-changes-to-bound-witness-size/3885):

* A transition to a binary trie via either a 'progressive' or 'clean cut' strategy. A progressive switchover requires [Sparse Merkle Trees](https://ethresear.ch/t/optimizing-sparse-merkle-trees/3751/20), while a clean-cut approach will likely cause a temporary disruption to the chain.

* Witnesses will be produced by miners keeping a copy of full state, and should be paid for by gas included within the transaction. This will likely involve gas price increases for EXTCODESIZE, BALANCE, CALL, and SLOAD opcodes.

In addition to these general points, we have some solid numbers from experiments that have been run by Igor Mandrigen:

Witness sizes in the binary trie representation are always more efficient than hexary, and weigh in somewhere in the ballpark of 0.3-1.4 MB depending on the witness. [[source](https://medium.com/@mandrigin/stateless-ethereum-binary-tries-experiment-b2c035497768)]

In the case of partial-state nodes, it's possible to reduce the storage requirements substantially by keeping partial data in the cache and building state incrementally from witnesses. [[source](https://medium.com/@mandrigin/stateless-ethereum-binary-tries-experiment-b2c035497768)]

Finally, [a first draft witness specification](https://github.com/ethereum/stateless-ethereum-specs/pull/1) has been submitted to the Stateless Ethereum specs repository, and is now under review. A formal witness specification is an important step toward the implementation of a stateless prototype.

## We don't *quite* know, but we know we can work it out

There are a few points of research that fit into the category of *"We know we need to do this, we don't know how exactly we're going to do it, but we have good intuitions and are reasonably confident it'll work".*

Code merkleization is one such area. To avoid unnecessarily large witnesses for contract calls, code merkleization splits up contract code so that only the portion of the code called needs to be included in the witness for a transaction. A current prototype exists that makes use of JUMPDEST instructions in the code to split up code chunks into more manageable sizes, but this is considered a 'leaky' abstraction because it exposes witness sizes to code semantics. A better approach would be fixed code chunk sizes, which is workable if a little bit of meta-data is put into each witness (specifying valid jump destinations), or if code is validated with all static jumps enforced at validation.

Another clear need is for witness indexing, i.e. to have some sort of 'canonical' identifier for all nodes to easily see that they have retrieved the correct witness for a given block *before* validating it locally. The most straightforward way to handle this is a protocol change to include `witnessHash` in the block header. This prevents potential attack vectors and makes generating a witness an explicit requirement for miners producing blocks to build an associated witness.

Also in this category are the large collection of 'meta' items that will help research continue and accelerate. This includes developer tools for experimenting with and collecting more useful data from the historical chain, formal EVM semantics, and a platform for running network simulations.

## Known unknowns
Many of the more fundamental questions concerning the *network* fall into the "unknown" category. These are questions that need a lot more investigation before we're confident about a direction to move forward in.

Data discovery and delivery for syncing nodes is a major concern for Stateless Ethereum, because unlike the current paradigm of sync, a new node cannot expect a random assortment of peers to have the pieces of state it needs to execute the current block.

Because the current Eth protocol does not provide an interface for clients to query about what state connected peers have available, this is a non-trivial problem. At a minimum, nodes will need the ability to query their peers for several different types of data, such as headers, receipts, transactions, and specific parts of state. There are plenty of open questions here about how that data is delivered to the requesting node, how full nodes will orient themselves in a network with many stateless peers, and how the network will prevent state data from getting lost forever. [A more detailed exposition](https://ethresear.ch/t/the-data-availability-problem-under-stateless-ethereum/6973) of this topic by Piper Mirriam was posted on the ethresear.ch forums.

The inherent challenge in these more distant questions is the fact that the network topology of stateless ethereum can only be guessed at based on assumptions of rational behavior and current activity. Unlike Eth2 research, however, Stateless Ethereum is similar enough to the current chain that historical statistics from Eth1 are relevant and useful for testing against. Still, more and higher quality data will be needed to inform further work on network topology.

A parallel line of quantitative inquiry will emerge once we have a reliable cluster of beam-syncing nodes to study and attack. A beam syncing node is essentially a janky version of a stateless node, which uses a "naive" primitive (`getNodeData`) to collect (and keep) missing pieces of state. We can learn a lot about desirable network primitives by trying to break beam sync, and then iterate on those lessons to bring beam sync closer to the "real" stateless sync paradigm.

Specific questions about the eventual transition to Eth2 are largely unanswered, although Vitalik's [alternative Eth1 <> Eth2 transition](https://ethresear.ch/t/alternative-proposal-for-early-eth1-eth2-merge/6666) proposal is most in line with current thinking in the 1x group.

## To Paris!

The 1x researchers will meet in Paris to discuss issues and work out solutions in person, the weekend following EthCC. This small in-person research summit will be a great opportunity to push forward with the work, and to collaborate on the more obscure and less understood challenges in Stateless Ethereum. *Allons-y!*

## Stateless Ethereum call #4

The next call is scheduled for Tuesday, February 25th. Reach out to @JHancock if you would like a calendar invite, and as always, @gichiba for feedback on these updates or requests for new topics.
