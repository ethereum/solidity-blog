---
layout: post
published: true
title: 'The 1.x Files: January call digest'
date: '2020-01-16'
author: Griffin Ichiba Hotchkiss
---

![The 1.x Files](https://blog.ethereum.org/img/2019/12/the1xfiles_black.png "The truth is out there.")

## January 14th tl;dc (too long, didn't call)

***Disclaimer***: *This is a digest of the topics discussed in the recurring Eth1.x research call, and doesn't represent finalized plans or commitments to network upgrades.*

The main topics of this call were
* Rough data quantifying advantages of switching to a binary trie structure
* Transition strategies and potential challenges for a switch to binary tries
* "Merklizing" contract code for witnesses, and implications for gas scheduling/metering
* Chain pruning and historical chain/state data -- network implications and approaches to distribution.


## Logistics

The weekend following EthCC (March 7-8), there will be a small 1.x research summit, with the intent of having a few days of solid discussion and work on the topics at hand. The session will be capped (by venue constraints) at 40 attendees, which should be more than enough for the participants expected.

There will also likely be some informal, ad-hoc gathering around Stanford Blockchain week and ETHDenver, but nothing explicitly planned.

The next call is tentatively scheduled for the first or second week in February -- half-way between now and the summit in Paris.

## Technical discussion

### [EIP #2465](https://github.com/ethereum/EIPs/issues/2465)
Although not directly related to stateless ethereum, this EIP improves the network protocol for transaction propagation, and is thus a pretty straightforward improvement that moves things in the right direction for what research is working on. Support!

### Binary Trie size savings

Transitioning to a binary trie structure (instead of the current hexary trie structure) should in theory reduce the size of witnesses by something like 3.75x, [but in practice that reduction might only be about half, depending on how you look at it.](https://medium.com/@mandrigin/stateless-ethereum-binary-tries-experiment-b2c035497768).

Witnesses are about 30% code and 70% hashes. Hashes within the trie are reduced by 3x, but code is not improved with a binary trie, since it always needs to be included in the witness. So switching to a binary trie format will bring witness sizes to ~300-1400kB, down from ~800-3,400kB in the hexary trie.

### Making the switch
Enacting the actual transition to a binary trie is another matter, with a few questions that need to be fleshed out. There are essentially two different possible strategies that could be followed:

*progressive transition* -- This is a 'ship of Theseus' model of transition whereby the entire state trie is migrated to a binary format account-by-account and storageSlot-by-storageSlot, as each part of state is touched by EVM execution. This implies that, forevermore, Ethereum's state would be a hexary/binary hybrid, and accounts would need to be "poked" in order to be updated to the new trie format (maybe with a POKE opcode ;). The advantages are that this does not interrupt the normal functioning of the chain, and does not require large-scale coordination for upgrading. The disadvantage is complexity: both hexary and binary trie formats need to be accounted for in clients, and the process would never actually "finish", because some parts of the state cannot be accessed externally, and would need to be explicitly poked by their owners which probably wont happen for the entire state. The progressive strategy would also require clients to modify their database to be a kind of 'virtualized' binary trie inside of a hexary database layout, to avoid a sudden dramatic increase in storage requirements for all clients (note: this database improvement can happen independent of the full 'progressive' transition, and would still be beneficial alone).

*compute and clean-cut* -- This would be an 'at once' transition accomplished over one or more hard-forks, whereby a date in the future would be chosen for the switch, and then all participants in the network would need to recompute the state as a binary trie, and then switch to the new format together. This strategy would be in some sense 'simpler' to implement because it's straightforward on the engineering side. But it's more complex from a coordination perspective: The new binary trie state needs to be pre-computed before the fork which could take an hour (or thereabouts) -- during that window, its not clear how transactions and new blocks would be handled (because they would need to be included in the yet-un-computed binary state trie, and/or the legacy trie). This process would be made harder by the fact that many miners and exchanges prefer to upgrade clients at the last moment. Alternatively we could imagine halting the entire chain for a short time to re-compute the new state -- a process which might be even trickier, and potentially controversial, to coordinate.

Both options are still 'on the table', and require further consideration and discussion before any decisions are made with regards to next steps. In particular weighing the trade-offs between implementation complexity on one hand and coordination challenges on the other.

### Code "chunking"
Addressing the code portion of witnesses, there has been some prototyping work done on code 'merklization', which essentially allows contract code to be split up into chunks before being put into a witness. The basic idea being that, if a method in a smart contract is called, the witness should only need to include the parts of the contract code that were actually called, rather than the entire contract. This is still very early research, but it suggests an additional ~50% reduction in the code portion of a witness. More ambitiously, the practice of code chunking could be extended to create a single global 'code trie', but this is not a well developed idea and likely has challenges of its own that warrant further investigation.

There are different methods by which code can be broken up into chunks, and then be used to generate witnesses. The first is 'dynamic', in that it relies on finding JUMPDEST instructions, and cleaving near those points, which results in variable chunk sizes depending on the code being broken up. The second is 'static', which would break up code into fixed sizes, and add some necessary metadata specifying where correct jump destinations are within the chunk. It seems like either of these two approaches would be valid, and both might be compatible and could be left up to users to decide which to employ. Either way, chunking enables a further shrinking of witness sizes.

### (un)gas
One open question is what changes would be necessary or desirable in gas scheduling with the introduction of block witnesses. Witness generation needs to be paid for in gas. If the code is chunked, within a block there would be some overlap where multiple transactions cover the same code, and thus parts of a block witness would be paid for more than once by all the included transactions in the block. It seems like a safe idea (and one that would be good for miners) would be to leave it to the poster of a transaction to pay the full cost of their own transaction's witness, and then let the miner keep the overpayment. This minimizes the need for changes in gas costs and incentivizes miners to produce witnesses, but unfortunately breaks the current security model of only trusting sub-calls (in a transaction) with a portion of the total committed gas. How that change to the security model is handled is something that needs to be considered fully and thoroughly. At the end of the day, the goal is to charge each transaction the cost of producing its own witness, proportional to the code it touches.

[Wei Tang's UNGAS proposal](https://ethereum.corepaper.org/compatibility/forward/#remove-gas-observables-and-better-error-handling) might make any changes to the EVM easier to accomplish. It's not strictly necessary for stateless Ethereum, but it is an idea for how to make future breaking changes to gas schedules easier. The question to ask is "What do the changes look like both without and with UNGAS -- and those things considered does UNGAS actually make this stuff significantly easier to implement?". To answer this, we need experiments that run things with merklized code and new gas rules appled, and then see what should change with regard to cost and execution in the EVM.

### Pruning and data delivery
In a stateless model, nodes that do not have some or all of the state need a way to signal to the rest of the network what data they have and what data they lack. This has implications for network topology -- stateless clients that lack data need to be able to reliably and quickly find the data they need somewhere on the network, as well as broadcast up-front what data they don't have (and might need). Adding such a feature to one of the chain-pruning EIPs is a networking (but not consensus) protocol change, and its something that also can be done now.

The second side of this problem is where to store the historical data, and the best solution so far proposed is an Eth-specific distributed storage network, that can serve requested data. This could come in many flavors; the complete state might be amenable to 'chunking', similar to contract code; partial-state nodes could watch over (randomly assigned) chunks of state, and serve them by request on the edges of the network; clients might employ additional data routing mechanism so that a stateless node can still get missing data through an intermediary (which doesn't have the data it needs, but is connected to another node that does). However it's implemented, the general goal is that clients should be able to join the network and be able to get all the data they need, reliably, and without jockying for position connecting to a full-state node, which is effectively what happens with LES nodes now. Work surrounding these ideas is still in early stages, but the geth team has some promising results experimenting with 'state tiling' (chunking), and turbo-geth is working on data routing for gossiping parts of state.

---

As always, if you have questions about Eth1x efforts, requests for topics, or want to contribute, come introduce yourself on ethresear.ch or reach out to @gichiba and/or @JHancock on twitter.
