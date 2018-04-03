---
id: 1698
title: 'The end of the beginning&#8230;'
date: 2015-05-02T17:44:52+00:00
author: Gavin Wood
layout: post
guid: https://blog.ethereum.org/?p=1698
permalink: /2015/05/02/the-end-of-the-beginning/
dsq_thread_id:
  - "3730648283"
tags:
  - dev
  - ethereum
  - olympic
---
As part of our tiered release process, we will soon be beginning the final phase prior to our Frontier release. So far our ongoing Proof-of-Concept IX testnet has been running smoothly for around a month with dozens of <a href="https://eth-netstats.herokuapp.com/" title="Netstats" target="_blank">nodes</a> around the world, well over 200,000 blocks and including clients on several platforms and three different clean-room implementations. However, that's not enough of a test for us.

In a phase of the PoC-IX testnet called "Olympic", the usage of this network will be incentivised through a collection of prizes (in ether, naturally!). We will give advice on what usage should maximise your chances of winning ether, though the precise judgement metrics will be withheld until prize time. Suffice it to say, those in the employ of Ethereum or its subsidiaries will not be eligible.

The sorts of usage we will be awarding include:

<li>mining
<li>spending gas (in a variety of ways)
<li>interesting contracts (variety of opcodes, length of execution time)
<li>... and of course a grand prize for causing a fork!
<br>
Vinay will keep you updated on this but our rough time scale for this is <i>as soon as possible</i> :)
<br>
<strong>Ongoing work from my side...</strong>
<br>
As you might notice if you keep an eye on our repositories, development proceeds steadily. Alex (alongside Felix on the Go side) continue to perfect our peer-network infrastructure, with the last few changes having been prototyped recently and a period of review and testing to be carried out prior to our Olympic binaries being released. Marek's work (alongside Fabian on the Go side) on the JSON-RPC testing and web3.js continues to prepare the ground for Frontier and lots of lovely ÐApps.

Meanwhile, Lefteris has been refactoring our Ethash implementation, good for both C++ and Go clients and helping make it as efficient and clean as possible. Yann and Arkadiy have been busy polishing Mix and getting the deployment experience as smooth as possible, with the help of Avsa, Fabian and Jutta. Christian and Liana continue working on Solidity, now putting in various optimisations and providing information for gas usage accounting in Mix. For my time coding, I've been doing mainly core refactoring and optimisation, getting the internals as smooth and reliable as possible, especially in block importing and transaction processing.

Having largely completed the our test suite for consensus checking, Christoph now continues on his mammoth mission to get full test coverage through the C++ codebase, helped along by Dimitri. One of our new starters, Vlad, will be helping out with p2p networking and ultimately working his way towards Whisper.

For the past month or so, Edward, our other new starter, has been working on our new developer-resources website for which I have big plans. More on that closer the time of release.

That's it for now - watch this space for the announcement of Olympic binaries and start figuring out how you'll be trying to win some of that ether!

