---
id: 3785
title: 'Ethereum Dev Roundup: Q1 (Boring Edition)'
date: 2017-04-07T11:30:17+00:00
author: Vitalik Buterin
layout: post
guid: https://blog.ethereum.org/?p=3785
permalink: /2017/04/07/ethereum-dev-roundup-q1-boring-edition/
---
The last one and a half months have seen great progress for Ethereum research, and we are excited that the protocol is moving closer and closer to the point where it is ready for mainstream adoption. Progress on consensus algorithms, privacy, zero knowledge proofs and formal verification is happening at increasing speed, and our research and development teams are expanding substantially.

A large amount of progress has been made on Casper. You can find a few Medium posts describing the algorithm here:
<ul>
 	<li><a href="https://medium.com/@VitalikButerin/minimal-slashing-conditions-20f0b500fc6c">https://medium.com/@VitalikButerin/minimal-slashing-conditions-20f0b500fc6c</a></li>
 	<li><a href="https://medium.com/@VitalikButerin/safety-under-dynamic-validator-sets-ef0c3bbdf9f6">https://medium.com/@VitalikButerin/safety-under-dynamic-validator-sets-ef0c3bbdf9f6</a></li>
</ul>
And a few blog posts from Yoichi formally proving correctness:
<ul>
 	<li><a href="https://medium.com/@pirapira/formal-methods-on-some-pos-stuff-e309775c2ab8">https://medium.com/@pirapira/formal-methods-on-some-pos-stuff-e309775c2ab8</a></li>
 	<li><a href="https://medium.com/@pirapira/a-mechanized-safety-proof-for-pos-with-dynamic-validators-17e9b45faff4">https://medium.com/@pirapira/a-mechanized-safety-proof-for-pos-with-dynamic-validators-17e9b45faff4</a></li>
 	<li><a href="https://medium.com/@pirapira/fixing-safety-proof-on-dynamic-validator-pos-5d8d463d8ae8">https://medium.com/@pirapira/fixing-safety-proof-on-dynamic-validator-pos-5d8d463d8ae8</a></li>
</ul>
And on "Vlad Casper":
<ul>
 	<li><a href="https://medium.com/@pirapira/formal-methods-on-another-casper-8a75f6e02073">https://medium.com/@pirapira/formal-methods-on-another-casper-8a75f6e02073</a></li>
</ul>
And it's not just theory:
<ul>
 	<li>Casper contract, with test script: <a href="https://github.com/ethereum/research/blob/master/casper4">https://github.com/ethereum/research/blob/master/casper4</a></li>
 	<li>Some work on the daemon by Karl Floersch: <a href="https://github.com/karlfloersch/simplecasper">https://github.com/karlfloersch/simplecasper</a></li>
</ul>
More progress on Casper and sharding will continue to be published once it stabilizes to the point where it makes sense to write the ongoing work up in a document; the high-level details are now close to finalized, current discussions have more to do with the finer details of the design of the Casper contract.

And some other blog posts from Vlad:
<ul>
 	<li>Dealing with Failure in Cryptocurrency: <a href="https://medium.com/@Vlad_Zamfir/dealing-with-failure-in-cryptocurrency-463475da83e5">https://medium.com/@Vlad_Zamfir/dealing-with-failure-in-cryptocurrency-463475da83e5</a></li>
 	<li>Simple model of an "internal" PoW attacker: <a href="https://medium.com/@Vlad_Zamfir/simple-model-of-an-internal-pow-attacker-1a713cf00672">https://medium.com/@Vlad_Zamfir/simple-model-of-an-internal-pow-attacker-1a713cf00672</a></li>
 	<li>A post agitating for smaller block rewards: <a href="https://medium.com/@Vlad_Zamfir/the-case-for-smaller-block-rewards-fb0eab38e15c">https://medium.com/@Vlad_Zamfir/the-case-for-smaller-block-rewards-fb0eab38e15c</a> ; see also <a href="https://www.reddit.com/r/ethereum/comments/609yjl/vladzamfir_if_the_price_of_ether_stays_high_i/">Reddit thread</a> and <a href="http://www.carbonvote.com/">Carbonvote</a></li>
</ul>
Some bugs were discovered in the ENS code leading to a delay of its launch. A security audit was commissioned, and the audit is now complete, bringing us a step closer to the final launch day:
<ul>
 	<li><a href="https://github.com/ethereum/ens/issues/96">https://github.com/ethereum/ens/issues/96</a></li>
 	<li><a href="https://github.com/ethereum/ens/issues/97">https://github.com/ethereum/ens/issues/97</a></li>
</ul>
Updates on formal proving:
<ul>
 	<li><a href="https://medium.com/@pirapira/ethereum-virtual-machine-for-coq-v0-0-2-d2568e068b18">https://medium.com/@pirapira/ethereum-virtual-machine-for-coq-v0-0-2-d2568e068b18</a></li>
 	<li>EVM formal provers can now run state tests: <a href="https://github.com/pirapira/eth-isabelle">https://github.com/pirapira/eth-isabelle</a></li>
</ul>
Metropolis is continuing to progress, and the core development teams are continuing to make progress toward fully agreeing on the specification. Tests for some of the EIPs are already available, and more tests are continuously being written.
<ul>
 	<li>Core dev meeting notes: <a href="https://github.com/ethereum/pm/tree/master/All%20Core%20Devs%20Meetings">https://github.com/ethereum/pm/tree/master/All%20Core%20Devs%20Meetings</a></li>
 	<li>Implementation in Geth: <a href="https://github.com/ethereum/go-ethereum/pull/3757">https://github.com/ethereum/go-ethereum/pull/3757</a></li>
 	<li>Implementation in Parity: <a href="https://github.com/paritytech/parity/issues/4833">https://github.com/paritytech/parity/issues/4833</a></li>
 	<li>Tests: <a href="https://github.com/ethereum/tests/tree/zeroTransaction">here</a>, <a href="https://github.com/ethereum/tests/commit/0d7e2ba8a2c72831696e8bcd09ce0cedeb01e027">here</a> and <a href="https://github.com/ethereum/tests/commit/05bffb4d7d6c2a5d6ee763693cf1c6a8d684b5ae">here</a>, among other places.</li>
</ul>