---
id: 3646
title: December Roundup
date: 2016-12-31T14:50:37+00:00
author: Vitalik Buterin
layout: post
guid: https://blog.ethereum.org/?p=3646
permalink: /2016/12/31/december-roundup/
---
December marks a month of continued progress in the Ethereum ecosystem. Research on proof of stake and sharding continues after the research team's <a href="https://blog.ethereum.org/2016/12/04/ethereum-research-update/">workshop in Singapore in November</a>, the light client slowly <a href="https://www.reddit.com/r/ethereum/comments/5kyxvd/i_just_tried_geth_light_today_and_am_jumping_for/">keeps getting better</a>, Whisper and Swarm keep moving forward, and discussions on protocol economics and community governance continue to mature.

First, privacy technologies on Ethereum, and particularly zk-SNARKs (or "zero knowledge proofs"), have been rapidly moving forward.
<ul>
 	<li>A blog post, "<a href="https://blog.ethereum.org/2016/12/05/zksnarks-in-a-nutshell/">zk-SNARKs in a Nutshell</a>", by Christian Reitwiessner</li>
 	<li>A blog post explaining <a href="https://medium.com/@VitalikButerin/quadratic-arithmetic-programs-from-zero-to-hero-f6d558cea649">quadratic arithmetic programs</a>, from myself</li>
 	<li>An <a href="http://github.com/ethereum/research/blob/master/zksnark/">implementation of elliptic curve pairings</a>, perhaps the most complex on-chain technical ingredient in zk-SNARK verification, from myself (with special thanks to Ariel and Sean from the Zcash team for their assistance)</li>
 	<li>Experimental work in <a href="https://github.com/ethereum/cpp-ethereum/blob/snark/libdevcrypto/LibSnark.cpp">integrating a zk-SNARK precompile</a> in C++ from Christian (also with special thanks to the Zcash team)</li>
</ul>
Vlad Zamfir has taken it upon himself to explain the history behind Casper, from his point of view:
<ul>
 	<li><a href="https://medium.com/@Vlad_Zamfir/the-history-of-casper-part-1-59233819c9a9">Chapter 1</a></li>
 	<li><a href="https://medium.com/@Vlad_Zamfir/the-history-of-casper-chapter-2-8e09b9d3b780">Chapter 2</a></li>
 	<li><a href="https://medium.com/@Vlad_Zamfir/the-history-of-casper-chapter-3-70fefb1182fc">Chapter 3</a></li>
 	<li><a href="https://medium.com/@Vlad_Zamfir/the-history-of-casper-chapter-4-3855638b5f0e">Chapter 4</a></li>
 	<li><a href="https://medium.com/@Vlad_Zamfir/the-history-of-casper-chapter-5-8652959cef58">Chapter 5</a></li>
</ul>
On proof of stake from myself:
<ul>
 	<li><a href="https://medium.com/@VitalikButerin/a-proof-of-stake-design-philosophy-506585978d51">A Proof of Stake Design Philosophy</a></li>
 	<li>And while we're at it, the <a href="https://github.com/ethereum/wiki/wiki/Proof-of-Stake-FAQ">Proof of Stake FAQ</a> and <a href="https://github.com/ethereum/wiki/wiki/Sharding-FAQ">Sharding FAQ</a> continue to exist and and continue to be worked on.</li>
</ul>
Vlad has also taken it upon himself to rail against the evils of "economic abstraction" (ie. the goal of trying to create token-agnostic public economic consensus protocols):
<ul>
 	<li><a href="https://medium.com/@Vlad_Zamfir/against-economic-abstraction-e27f4cbba5a7">Round 1</a></li>
 	<li><a href="https://medium.com/@Vlad_Zamfir/against-economic-abstraction-round-2-21f5c4e77d54">Round 2</a></li>
 	<li><a href="https://medium.com/@Vlad_Zamfir/against-economic-abstraction-mitigating-possible-collateral-damage-ce8b939e808f">But not technical abstraction!</a></li>
</ul>
Various discussions were had on monetary policy:
<ul>
 	<li>A <a href="https://github.com/ethereum/EIPs/issues/186">community-created EIP</a> (186) proposed to decrease ETH issuance by ~3x before PoS</li>
 	<li><a href="https://www.reddit.com/r/ethereum/comments/5kssyp/against_economic_abstraction_round_2/">Discussions on issuance in Casper</a> in the Reddit thread for one of Vlad's posts</li>
</ul>
Speaking of EIPs...
<ul>
 	<li>Greg Colvin's suggested modifications for <a href="https://github.com/ethereum/EIPs/issues/184">adding further static analysis capability</a> (184) as part of the move toward "EVM 1.5"</li>
 	<li>The Ethereum Name System (launched on the Ropsten testnet in late November), saw an <a href="https://github.com/ethereum/EIPs/issues/181">EIP opened</a> (181) to support reverse resolution of Ethereum addresses</li>
</ul>
The data storage-focused "sister protocol" Swarm continues to move forward:
<ul>
 	<li><a href="https://blog.ethereum.org/2016/12/15/swarm-alpha-public-pilot-basics-swarm/">Public pilot released</a>! (by Viktor Tron)</li>
 	<li>A <a href="https://www.reddit.com/r/ethereum/comments/5kl5lm/swarm_is_awesome/">reddit thread</a> with community feedback</li>
</ul>
And from the core client development standpoint:
<ul>
 	<li>Geth 1.5.5 was released, combining <a href="https://github.com/ethereum/go-ethereum/releases/tag/v1.5.5">small but important fixes</a> to various "bugs and annoyances"</li>
 	<li>Jan Xie is continuing work on pyethereum to see how well it can pass through all of the denial-of-service blocks in September and October. <a href="https://gist.github.com/janx/49b41aaf51f95f626b2b237c22ecf5dd">Not always</a>, though the good news is that there seem to be no quadratic memory issues that stop the client outright.</li>
 	<li>Another grab bag of <a href="https://github.com/ethereum/mist/releases/tag/v0.8.8">small but important security fixes and stability improvements</a> from Mist 0.8.8, after an audit from <a href="https://cure53.de/">Cure53</a></li>
</ul>
We wish the community a happy new year and look forward to more progress in January!