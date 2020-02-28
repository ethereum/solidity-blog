---
layout: post
published: true
title: 'The 1.x Files: February call digest'
date: '2020-02-28'
author: Griffin Ichiba Hotchkiss
category: 'Research & Development'
---

![The 1.x Files](https://blog.ethereum.org/img/2019/12/the1xfiles_black.png "The truth is out there.")

## February 26th tl;dc (too long, didn't call)

**Disclaimer:** This is a digest of the topics discussed in the recurring Eth1.x research call, and doesn’t represent finalized plans or commitments to network upgrades.

The main topics of this call were:

* The rough plan for the 1.x research summit in Paris following EthCC
* The Witness Format
* The 'data retrieval problem'

## Logistics

The summit to discuss and collaborate on Stateless Ethereum is planned for the weekend following EthCC, which will be an indispensable time for working on the most important and unsolved problems for this effort.

The schedule is not fixed yet, but a rough outline is coming together:

**Saturday** - After an hour of breakfast and free discussion, we'll come together to agree on goals and scope for the summit. Then there is about 4 hours reserved for organized presentations and 'deep dives' on particular topics of importance. In the later afternoon/evening there will be another hour+ of free time and informal discussion.

**Sunday** - The same as before, but with only 2 hours of structured presentations, to encourage attendees to break out into groups and work on the various research or implementation topics for the rest of the Summit. Finally, there will be a concluding discussion to map out next steps and revise the tech tree.  

It should be stated that this research summit is not focused on public or general engagement, in favor of making meaningful progress on the work ahead. This is not meant to be a spectator's event, and indeed there is some expectation that attendees will have 'done their homework' so that the short amount of time for discussion is efficiently spent.

## Technical discussion

### Witness Format

The first topic of technical discussion was centered around the recently submitted [draft witness specification](https://github.com/ethereum/stateless-ethereum-specs/pull/1), which will help to define implementation for all client teams.

The witness specification is really comprised of two parts: Semantics and Format. This organization has the desirable property of cleanly separating two aspects of the witness that might have different goals.

Semantics are a bit harder to get to grips with, and are concerned merely with the abstract methods of taking one group of objects and transforming them into other objects. The witness semantics are in simple formal language describing how to get from inputs to outputs, leaving all implementation details abstracted away. For example, questions about data serialization or parsing are not relevant to the witness semantics, as they are more of an implementation detail. The high-level goal of defining the semantics of witnesses in a formal way is to have a completely un-ambiguous reference for client teams to implement without a lot of back-and-forth. Admittedly, starting with formal semantics and working towards implementation (rather than say, coding out a reference implementation) is experimental, but it's hoped that it will save effort in the long run and lead to much more robust and diverse Stateless Ethereum implementations. Format is much more concrete, and specifies real details that affect interoperability between different implementations.

The witness format is where things like the size of code chunks will be defined, and a good witness format will help different implementations stay inter-operable, and in general terms describes encoding and decoding of data. The format is not specifically geared at reducing witness size, rather at keeping the client implementations memory-efficient, and maximizing the efficiency of generation and transmission. For example, the current format can be computed in real time while walking through the state trie without having to buffer or process whole chunks, allowing the witness to be split into small chunks and streamed.

As a first draft, there is expected to be some refactoring before and after Paris as other researchers give feedback, and already there is a request for a bit more content on design motivations and high-level explanation concerning the above content. It was also suggested in the call that the witness format be written in about in an upcoming "The 1x Files" post, which seems like a great idea (stay tuned for that in the coming weeks).

### Transaction validation, an interlude

Moving towards less concrete topics of discussion, one fundamental issue was brought up in the chat that warrants discussion: A potential problem with validating transactions in a stateless paradigm.

Currently, a node performs two checks on all transactions it sees on the network. First, the transaction nonce is checked to be consistent with all transactions from that account, and discarded if it is not valid. Second the account balance is checked to ensure that the account has enough gas money. In a stateless paradigm, these checks cannot be performed by anyone who does not have the state, which opens up a potential vector for attack. It's eminently possible that the format of witnesses could be made to include the minimum amount of state data required to validate transactions from witnesses only, but this needs to be looked into further.

The transaction validation problem is actually related to a more general problem that Stateless Ethereum must solve, which is tentatively being called "The data retrieval problem". The solution for data retrieval will also solve the transaction validation problem, so we'll turn to that now.

### Data retrieval in Stateless Ethereum

The full scope of this challenge is outlined [in an ethresearch forum post](https://ethresear.ch/t/the-data-availability-problem-under-stateless-ethereum/6973), but the idea relatively straightforward and built from a few assumptions:

It's possible to, within the current `eth` protocol, build a stateless client using existing network primitives. This is *sort of* what beam sync is, with the important distinction that beam sync is meant to keep state data and 'backfill' it to eventually become a full node. A stateless client, by contrast, throws away state data and relies entirely on witnesses to participate in the network.

The current protocol and network primitives assume that there is a high probability that connected peers keep valid state, i.e. that connected peers are full nodes. This assumption holds now because most nodes are indeed full nodes with valid state. But this assumption cannot be relied upon if a high proportion of the network is stateless. The current protocol also does **not** specify a way for a new connected node to see if a connected peer has or does not have a needed piece of state data.

Stateless clients have better UX than full nodes. They will sync faster, and allow for near instantaneous connection to the network. It's therefore reasonable to assume that over time more and more nodes will move towards the stateless end of the spectrum. If this is the case, then the assumption of data availability will become less and less sound with a higher proportion of stateless nodes on the network. There is a theoretical 'tipping point' where stateless nodes outnumber stateful nodes by far, and a random assortment of peers has a sufficiently low probability of at least one holding the desired piece of state. At that (theoretical) point, the network breaks.

The kicker here is that **if the network allows state to be gotten on demand (as it does now), a stateless client can (and will) be made on the same protocol**. Extending this reasoning to be more dramatic: Stateless clients are inevitable, and the data retrieval problem will come along with them. It follows then, that significant changes to the `eth` network protocol will need to be made in order to categorically prevent the network from reaching that tipping point, or at least push it further away through client optimizations.

There are a lot of open-ended topics to discuss here, and importantly there is disagreement amongst the 1x researchers about exactly how far the network is from that theoretical breaking point, or if the breaking point exists at all. This highlights the need for more sophisticated approaches to network simulation, as well as the need for defining the problem clearly at the research summit before working towards a solution.



## **À tout à l’heure !**

Exciting things will undoubtedly be unfolding as a result of the in-person research to be conducted in Paris in the coming fortnight, and the next few installments of "The 1.x Files" will be devoted to documenting and clearly laying out that work.

The summit in Paris is very nearly at full capacity, so if you have not filled out the RSVP form to attend please get in touch with Piper to see if there is space.

As always, if you're interested in participating in the Stateless Ethereum research effort, come join us on ethresear.ch, get invited to the telegram group, and reach out to @gichiba and/or @JHancock on twitter.
