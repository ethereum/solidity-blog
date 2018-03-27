---
id: 1755
title: Another Ethereum ÐΞV Update
date: 2015-06-15T08:23:02+00:00
author: Gavin Wood
layout: post
guid: https://blog.ethereum.org/?p=1755
permalink: '/2015/06/15/another-ethereum-d%ce%bev-update/'
dsq_thread_id:
  - "3849292325"
image: /wp-content/uploads/2015/06/youth.jpg
tags:
  - dev
  - ethereum
  - update
---
Been a while, I know, but then these are rather busy days.

I'm writing this from a Starbucks in Shanghai sitting behind the Great Firewall, able to peep out only occasionally. I've been in Asia with Marek for the last couple of weeks, mainly for meetups, workshops and technical meetings. During this time, we've seen the release, struggle and survival of the Olympic testnet, a very clear signal that a multi-client system would be far superior to the present monoculture. We've seen the beginning of the second external Go audit, mainly for checking of regressions but also to offer comments and criticisms on aspects that have changed in the meantime such as Go's network layer and sync strategy. We've seen Mix and Solidity continue to progress and mature. And through ongoing development and the progression of the APIs we've seen all clients become cleaner, faster and more resilient.

<strong>Exchanges</strong>

We've been continuing to work with exchanges, now Marek and Konstantin both provide substantial technical support, helping the coders from the exchanges understand the wide differences between using standard cryptocurrency APIs and Ethereum's smart-contract-based approach. In order to provide better support for them and maximise adoption, we've designed and introduced a middleware JSON RPC proxy allowing a much more familiar interface to Ethereum for exchanges.

<strong>C++</strong>

On the C++ side, we have introduced PV61, a new Ethereum network protocol version (backwards compatible so as not to hamper the Frontier release schedule for Go) providing super-fast parallel hash-chain downloading and much more resilience to dirty hash-chain attacks. Arkadiy is helping code that up for C++ with the hope that it can be worked into Go before Homestead.

Solidity, meanwhile, has progressed substantially: Solidity can now structurally predict gas usage, optimise for storage access and code size very well, and support internal-function calls with dynamic types, together with a whole host of other features. Liana continues hacking on that codebase together with Christian.

Mix, under the guidance of Aeron and Jutta with Avsa providing visual design insights and Yann hacking, is seeing substantial UX polish and refactoring, with the general target being to have it be an ultra-usable, yet powerful, IDE for contract development. ÐApps may now be deployed to the network in a simple 3-stage process, and the state/blockchain management layer is becoming far improved, featuring a single-pane view from which you can easily see all transactions, calls and logs and propagate transactions between chain-refreshes easily.

Alex continues to work on the low-level network layer, now being helped by Vlad. We had a summit in Zug 3 weeks ago where I laid out the plans for our libp2p networking layer, and how the Whisper and Ethereum protocols fit into that. Our first full-time developer on Whisper, Vlad will continue hacking at libp2p and Whisper, making the protocol more resilient to various Ethereum networking attacks whilst continuing the R&D for the p2p asynchronous non-endpoint-specific signalling protocol.

Pawel, working from Warsaw, has been continuing on the JIT EVM and EVM libraries, optimising stack usage and helping fix the stack depth issues we were seeing on Mac OS X. Vladimir meanwhile continues his work on testing and Marian on the <a href="http://stats.ethdev.com">netstats page</a> which has recently proved its utility in more ways than one. Those who have been experimenting with mining setups on the Olympic testnet will probably recognise Lefteris's work in taking over the Ethash implementation, and updating, refactoring and generally supporting Tim Hughes' OpenCL implementation. Special thanks to each of the members of the community who have helped one way or another with the development of this code. You know who you are (-: Christoph, meanwhile, is now working on test coverage for the core classes, as well as helping fix various issues in the core. 

Aside from working with the Mix teams, Arkadiy and Marek, I've been coding up the new C++ wallet/secret store code (a revamped format and compatibility with Go and Python), optimising the core (import times for at least one gas-heavy block were recently measured as being ~10x faster than Python and ~3x faster than Go), bug fixing across the core, implementation of the new 'ethvm' binary for running the EVM in standalone mode and the 'ethkey' binary for wallet management and, most recently, coding up a much better interactive console interface, similar to Go's Javascript console. I'm also hoping to get a much better blockchain download diagnostic display going soon in AlethZero.

<strong>Auditing</strong>

The internal audit of the C++ codebase has begun, with Christoph working on tests for all core classes and Arkadiy helping with some of the core optimisations. The SecretStore and Wallet classes have already been audited by Christian, thereby giving credibility that the C++ client is reasonably secure for managing private keys. The C++ external audit will begin in three short weeks.

Since the substantial failure of the testnet caused by issues in only one of the three clients, including issues that survived two auditing procedures, it has become clear to us that auditing, external or internal, is no silver bullet. While we are committed to providing the very best software and will continue with our auditing programme, please let's be clear: there is nothing magical about the Go client having gone through an auditing process. We are aiming to have all clients through at least a basic auditing process by the Homestead release and I see no reason to use the Go client over other clients (C++ or Python) at this stage, nor, for the vast majority of users, during Frontier. No clients come with any guarantees. Indeed there's the argument that minority clients, being a smaller target, are less likely to be attacked.

<strong>Onwards</strong>

We continue our preparations for the Frontier release. While we're still uncertain of the precise release date, we are becoming increasingly happy at the resilience of the Olympic testnet. As the Olympic testnet's failure was ongoing, the adversity caused some reflection over how we might mitigate such problems in the future. The depth and duration of the consensus failure can, roughly speaking, be put down to two problems: firstly that there was a bug in the Go codebase causing it to accept invalid blocks (in this case, blocks with an invalid proof-of-work nonce); secondly that there was a huge problem with upgrading the network since miners continued to mine on the 'bad' chain and were slow to upgrade their nodes so that they mined on the correct chain. Essentially, the first was a forensic problem and the second an problem of organisation.

To make sure this is never so deep nor broad in the future, I designed two new protocols: the <a href="https://github.com/ethereum/wiki/wiki/Bad-Chain-Canary">Bad Chain Canary</a> and the <a href="http://github.com/ethereum/wiki/wiki/Bad-Block-Reporting">Bad Block Reporting API</a>. The bad chain canary is a simple contract. It is controlled by a single key (though there may be several of them) and sits quietly until poked by its owner. I will be one such owner, Vitalik and Jeff two others and there will likely be fourth, Christoph. When poked, it is given a blockhash and block number pair; this pair refers to a recent block on a bad chain. If they match the chain that the contract thinks it is sitting on, it puts itself in the "bad chain" state. This allows the Ethereum core devs to inform everyone who is running a client on a bad chain (i.e. one that does not conform to the Yellow Paper due to a bug) that it is such. The default in clients (easily overridden) is not to mine on such chains (since to do so would be a waste of energy), but rather to inform the user that they should upgrade at their earliest opportunity.

The Bad Block Reporting API is a simple JSON RPC that allows a number of our nodes to run in a 'sentinel' mode. In this mode, if they come across a bad block, they automatically compile a standardised report of what went wrong including expected transaction receipts, VM traces, proof-of-work and block error information. This report is then instantly sent to a ÐΞV server where it can trigger an early warning system and, should the release tzar (that'd be Taylor) deem it necessary, alert the core devs. Through standardisation and usage of a simple format, it's designed so that we can easily code up a JSON-comparison engine to quickly diagnose where the consensus issue lies.

One future job is to combine the two so that the JSON-RPC can also manage the reporting of canary information - in this case, a user may set up a sentinel server to give them an email whenever the contract reports that mining has ceased and/or they should upgrade their node to continue mining.

<strong>Foundation</strong>

Finally, the three directors of the Ethereum Foundation (Vitalik with three votes, then Taylor and Mihai with a vote each) will soon convene for, in Vitalik's words, a "kind of great passing of the baton event". Kelley, ÐΞV's indomitable COO, has been spearheading the search for proven candidates from a variety of fields who can add real value to the foundation both in terms of fundraising and of driving adoption. We're extremely grateful for all her hard work in this regard. 
