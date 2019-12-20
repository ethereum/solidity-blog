---
layout: post
published: true
title: 'The 1.x Files: December call digest'
date: '2019-12-20'
author: Griffin Ichiba Hotchkiss
---

![The 1.x Files](https://blog.ethereum.org/img/2019/12/the1xfiles_black.png "The truth is out there.")

## December 17 tl;dc (too long, didn't call)

***Disclaimer***: *This is a digest of the topics discussed in the recurring Eth1.x research call, and doesn't represent finalized plans or commitments to network upgrades.*

The main topic of discussion for this call and broader Eth1.x research is the feasibility of *stateless clients*. This week was largely about organizing and sizing up the various challenges the stateless client concept will need to overcome to be viable, and identifying some tangible objectives on the short-term (3 to 6 months) to focus on.

### On the logistical side:

* At this point there is not enough compiled content to warrant the creation of an Eth1x spec repo, but establishing a public repository for continued research will be revisited at the next call/meeting.

* The Eth 1.x working group will be aiming to hold ~3 in-person meet-ups next year, with the first one tentatively-planned adjacent to EthCC, but as France is still on strike this is still uncertain.

* The next call is tentatively scheduled for mid-January 2020. As always, if you are interested in joining the call or the [telegram group](https://ethresear.ch/t/telegram-group-for-eth1x-stateless-client/6470/6), please [introduce yourself](https://ethresear.ch/t/introductions-for-the-eth1-x-research-group/6430/22) on the ethresearch forums.  

* One area that the working group is focusing on is maintaining a high level of communication with the broader Ethereum community. This includes speaking and answering questions at the many Ethereum conferences in 2020, but also includes explainers and these call updates. If you have feedback on Eth 1.x comms, want to find a speaker for an upcoming event, or have any questions, please reach out to @gichiba or @JHancock on twitter or the ethresearch forums.

### Technical discussion:

* Statelessness is a spectrum. It would be nearly impossible to design and implement a network change that would make *all* nodes on Ethereum change to a stateless paradigm. Rather, something less dramatic should happen: New network primitives can be developed that allow for a range of stateful/stateless behavior; with miners and archive nodes remaining "full-state", while clients and dapp-specific nodes can choose to maintain partial state, or be fully stateless.

* There are a range of issues that need to be considered for this to work:

  * Questions about the incentives to keep full or partial state, and what effect those incentives have on the overall topology of the network. For example, there needs to be an incentive for a full-state miner to include witness data with each block, and an incentive to verify witnesses on received blocks. More generally, it seems like a semi-stateful network would naturally arrange itself like the diagram below, with full-state miners passing and verifying new blocks outwards to partial-state clients, who can still provide witnesses for the stateless clients out on the edges (relying entirely on witnesses). This isn't a good or bad thing, but the implications of a topology like below need to be fully considered.
    ![hypothetical semi-stateless topology](https://blog.ethereum.org/img/2019/12/semi-stateless-topology.jpg "hypothetical semi-stateless topology")

  * Witness format has not yet been formally defined. Optimistically, the format for witnesses would be cast in the same mold as [the red queen sync](https://ethresear.ch/t/red-queens-new-sync-proposal/5351) protocol for state, and the semantics of witnesses formalized as such. There are huge benefits to a more formal definition of witnesses in the context of client development, and indeed the turbo-geth team is hoping to have witnesses formalized early next year. Block witnesses and their propogation are essential for moving towards the stateless client concept.

  * In the more near-term, trinity's beam sync implements a limited version of state witnesses, and is a good first step toward witnesses on mainnet. Beam sync is not *specifically* a pre-cursor to stateless client research (rather, it's a UX improvement for Eth 1.0), but can be seen as an opportunity for network experimentation with witness propagation, especially cross-client implementations. Currently Besu is experimenting with their own prototype of beam sync. One tangible goal for the near term is getting a common test specification for cross-client sync, which will help with data collection later down the road. This sounds like a job for [Hive](https://github.com/ethereum/hive). As it stands, beam sync is planned to be ready for prime-time (reliably passing around witnesses) in Q2 next year.

  * There are complexities around gas and witness sizes that must be addressed for stateless clients to be workable. It's thought that Wei Tang's 'unGas' proposal is a potential solution, but even though it is not particularly complicated to implement within Ethereum, there are spillover effects (such as necessary changes to compilers, potential smart contract effects, etc) that complicate implementation. All these have yet to be thoroughly investigated.

* The broader goal with this discussion is to focus and organize work in the short-term. This means prioritizing work on witness specification/implementation, beginning to outline some standard testing for cross-client sync, and collecting more data on modeling network requirements for witness propagation.
