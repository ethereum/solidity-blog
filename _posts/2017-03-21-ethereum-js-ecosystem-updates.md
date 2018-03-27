---
id: 3752
title: Ethereum JS Ecosystem Updates
date: 2017-03-21T03:11:39+00:00
author: Martin Becze
layout: post
guid: https://blog.ethereum.org/?p=3752
permalink: /2017/03/21/ethereum-js-ecosystem-updates/
---
It's been a fairly busy for the last couple of months for the Ethereum javascripters. To start with, there was a really great hackathon with IPFS. You can read Dan Finlay's excellent write up <a href="https://media.consensys.net/ipfs-ethereum-hackathon-results-d6b90b34286f#.25vwli7yd"><span style="font-weight: 400;">here</span></a><span style="font-weight: 400;">.</span>

<span style="font-weight: 400;">Also, during this time Aaron Davis (<a href="https://github.com/kumavis">Kumavis</a>) made some excellent progress towards a JS light client by utilizing IPFS's <a href="https://github.com/libp2p">libp2p</a> to build a in-browser mesh network and <a href="https://github.com/ipld">IPLD</a> to provide the merklization layer. This will be important work in the future for building pure in-browser clients. Also <a href="http://github.com/cdetrio">Casey Detrio</a> worked on a standard json RPC test suite, which you can see the results of </span><a href="http://cdetr.io/eth-compat-table/"><span style="font-weight: 400;">here</span></a><span style="font-weight: 400;">.</span>

<span style="font-weight: 400;">After the Seattle Meetup, we (<a href="https://github.com/axic">Axic</a> and <a href="https://github.com/wanderer">Wanderer</a>) sat down for a week long hackathon in Budapest to hash out some details of <a href="https://github.com/ewasm">ewasm</a>. Elsewhere in JS world, Fabian is doing a huge refactor of Web3.js for the 1.0 release, while Nick Dodson has been busy with <a href="https://github.com/ethjs">ethjs</a>. The rest of this post will be charting the various projections that this technology could provide as well as going into some details about each individual project. All these projects are all open source and encourage community participation, so if you are interested please check them out, say hello and send in a PR if you have time! </span>
<h2><span style="font-weight: 400;">EWASM</span></h2>
<span style="font-weight: 400;"><a href="https://github.com/ewasm/design">Ewasm’s</a> goal is to research and replace the EVM with <a href="http://github.com/webassembly">Webassembly</a> and secondarily, implement a client for the current system which can be efficiently JITed (or <a href="http://github.com/ewasm/evm2wasm">transcompiled</a>) to WebAssembly.</span>

<span style="font-weight: 400;">A major piece of evaluating WebAssembly for blockchain usage will be create a test network and this year the focus of the Ewasm team will be bringing that test network to life. The testnet work will:</span>
<ul>
 	<li style="font-weight: 400;"><span style="font-weight: 400;">enable hands-on work with ewasm for a wider audience</span></li>
 	<li style="font-weight: 400;"><span style="font-weight: 400;">enable related work, such as experiments with casper to be done by providing a flexible platform for experimentation</span></li>
</ul>
<span style="font-weight: 400;">The ewasm track is dedicated to ewasm research and development, while the client integration track will be dedicated to developing the network and bring full and light clients into existence. But there are many shared components to these two tracks. The Ewasm project is being broken down into two main components: the <a href="http://github.com/ewasm/ewasm-kernel">Kernel Layer</a>, which handles IPC and manages the state, and the core VM. This should enable us to use the same framework for different VM implementations.</span>

<span style="font-weight: 400;">So to recap, the major tasks for ewasm are:</span>
<ul>
 	<li style="font-weight: 400;"><span style="font-weight: 400;">Build an ewasm test network</span></li>
 	<li style="font-weight: 400;"><span style="font-weight: 400;">Creating a reusable “kernel” module</span></li>
 	<li style="font-weight: 400;"><span style="font-weight: 400;">Revamp ethereumjs-vm</span>
<ul>
 	<li style="font-weight: 400;"><span style="font-weight: 400;">Use ewasm-kernel for the message passing</span></li>
 	<li style="font-weight: 400;"><span style="font-weight: 400;">Implement the latest EIPs</span></li>
</ul>
</li>
 	<li style="font-weight: 400;"><span style="font-weight: 400;">Ewasm integration tools</span></li>
 	<li style="font-weight: 400;"><span style="font-weight: 400;">Solidity-ewasm integration (upcoming effort for the solidity hackathon!)</span></li>
</ul>
Please come join the implementation effort! We have semi-weekly meetings on Tuesdays. Our communication channel is on Matrix at <a href="https://riot.im/app/#/room/#prima:matrix.org">prima:matrix.org</a> (or #ewasm on IRC or at <a href="https://gitter.im/ewasm/Lobby">gitter</a>)
<h2><span style="font-weight: 400;">Networking</span></h2>
<span style="font-weight: 400;">There are several reasons to have an Ethereum networking implementation in JS. For one, it would allow us to implement a full and light Ethereum JS node. These light clients would run both in a node.js environment and in a browser. A prerequisite for an in-browser light client is “bridge” nodes. These nodes might also act as </span><a href="https://www.webrtc-experiment.com/docs/WebRTC-Signaling-Concepts.html"><span style="font-weight: 400;">signaling servers</span></a><span style="font-weight: 400;"> for the </span><a href="https://webrtc.org/"><span style="font-weight: 400;">webrtc</span></a><span style="font-weight: 400;"> network that the browser light clients would use to relay  messages from the RLPx network to the webrtc network. This work is being spearheaded by Metamask using IPFS’s libp2p. Also the <a href="https://github.com/ethereumjs/ethereumjs-devp2p">RLPx</a> implementation was recently revamped by </span><a href="https://github.com/fanatid"><span style="font-weight: 400;">fanatid</span></a><span style="font-weight: 400;">.</span>
<h2><span style="font-weight: 400;">IPLD</span></h2>
<span style="font-weight: 400;">Ethereum’s blockchain and on-chain state can be understood as a graph of hash-linked data. <a href="https://github.com/ipld">IPFS/IPLD</a> is proposed as a generic system to describe and distribute hash-linked data. Therefore we can describe Ethereum as an application layer on top of the hash-linked data availability platform. As a proof of concept, <a href="https://github.com/kumavis">Kumavis</a> implemented <a href="https://github.com/ipld/js-ipld-eth-block">IPLD resolvers</a> for the Ethereum data formats that define where hash-links are encoded inside the canonical Ethereum formats (e.g. block and state trie node). This, combined with other generic features of libp2p (IPFS’s generic p2p networking stack), enables the creation of minimal Ethereum clients that focus on the consensus protocol and state transition mechanism. One advantage of this approach is that the networking layer is transport-agnostic and can be used in environments that don’t have access to tcp/udp (such as the browser) that the standard Ethereum clients require. This project is still in the research phase. MetaMask hopes to use this approach to implement a browser compatible Ethereum light client via a secondary network, bridged by hybrid nodes.</span>
<h2><span style="font-weight: 400;">Web3.js 1.0 incoming! </span></h2>
<span style="font-weight: 400;">A new version of web3.js is in the making. It is the biggest refactor of the codebase since the inception of the popular Ethereum library. It </span><span style="font-weight: 400;">will have a lot of convenience features like confirmation and receipt event on transactions, a nice subscription API, and checksum checks on address inputs.</span>

<span style="font-weight: 400;">The API is still not yet finalized, but if you are eager to have a look you can check out the docs <a href="http://web3js.readthedocs.io/en/1.0/getting-started.html">here</a>.</span>

The new version will also have quite a few breaking changes, but those updates are necessary to get the new API right and remove some some deprecated methods along the way, like synchronous calls. 1.0 will only have <em>promises</em> and in some events “PromiseEvents” to better reflect multiple events on a transaction's execution. For those who are thinking of transitioning their apps to the new web3, there will be a migration guide upon launch to help make the transition from 0.x.x as easy as possible.

<span style="font-weight: 400;">In Mist there will be no web3 exposed by default anymore, as this encourages the bad habit of relying on the Mist-provided web3, which makes breaking changes disastrous for dapps. Instead, there will be an “ethereumProvider”, which libraries like web3 can use to talk to the underlying node. Web3.js will automatically detect any given provider and expose it on its API for easy instantiation.</span>

<span style="font-weight: 400;">For those who can’t wait and want to try it right now, checkout the 1.0 branch in the <a href="https://github.com/ethereum/web3.js/tree/1.0">web3.js repo</a></span><span style="font-weight: 400;">. Be aware there might be dragons!</span>
<h2><span style="font-weight: 400;">Ethjs</span></h2>
<span style="font-weight: 400;"><a href="https://github.com/ethjs">Ethjs</a> is a new highly optimised, light-weight JS utility for </span><a href="https://www.ethereum.org/"><span style="font-weight: 400;">Ethereum</span></a><span style="font-weight: 400;"> geared to working with the json RPC, much like web3.js but lighter, async only and using <a href="https://github.com/indutny/bn.js/">bn.js</a>. The current ongoing activity includes:</span>
<ul>
 	<li style="font-weight: 400;"><span style="font-weight: 400;">Adding the ABI methods for decoding logs in ethjs-abi</span></li>
 	<li style="font-weight: 400;"><span style="font-weight: 400;">Having fixed a small decoding bug in ethjs-abi (handling 0x addresses)</span></li>
 	<li style="font-weight: 400;"><span style="font-weight: 400;">Merged new schema for personal recover and sign ethjs-schema</span></li>
 	<li style="font-weight: 400;"><span style="font-weight: 400;">Looking for help making ethjs-filter stateless (infura ready)</span></li>
 	<li style="font-weight: 400;"><span style="font-weight: 400;">Bug fixing in ethjs-contract</span></li>
 	<li style="font-weight: 400;"><span style="font-weight: 400;">Documentation updates all around</span></li>
 	<li style="font-weight: 400;"><span style="font-weight: 400;">Upcoming ethjs version 0.2.7 release!</span></li>
</ul>
<h2><span style="font-weight: 400;">TestRPC</span></h2>
<span style="font-weight: 400;">Working on the 4.0.0 release! This release will include:</span>
<ul>
 	<li style="font-weight: 400;"><span style="font-weight: 400;">Database persistence. Now you can create a test chain and save that data, just like any other private chain!</span></li>
 	<li style="font-weight: 400;"><span style="font-weight: 400;">Clean up of how data is stored in memory, which should reduce memory issues significantly. Although there will be a slight cost in some performance, which mostly be unnoticeable unless you’re making thousands of transactions, it will bring a huge increase in stability.</span></li>
 	<li style="font-weight: 400;"><span style="font-weight: 400;">Bundling for the browser (provider only).</span></li>
 	<li style="font-weight: 400;"><span style="font-weight: 400;">Easier installs on Windows, and possibly other platforms.</span></li>
</ul>
We’ll be moving the TestRPC to the <a href="http://github.com/trufflesuite">Truffle github organization</a> since it’s primarily maintained by Truffle developers. There are significant new TestRPC add-ons coming. And we're investing significant energy in documentation and branding that unifies it under the Truffle brand. Any feedback on this move is appreciated.  Lastly, the TestRPC needs a new name that exudes everything it can do. If you have an idea let us know!
<h2>Community</h2>
The Ethereum JS community is an exciting and wonderful thing to be a part of. There are many great projects happening. If you are interested in plug in we have weeklyFriday meetings at 3:00 EST / 10:00 PST / 18:00 UTC. Watch <a href="https://gitter.im/ethereum/ethereumjs-lib">our gitter channel</a> for the chat link. Also, we are organizing an <a href="https://github.com/ethereumjs/organization/issues/9">upcoming hackathon</a>. Let us know if you are interested.

<hr />

<strong>ADDENDUM</strong> [Mar. 22, 2017]: Note that some of the projects in this post are not directly supported by Ethereum Foundation, but have been included as they are relevant to the overall Ethereum JS ecosystem update by the author.