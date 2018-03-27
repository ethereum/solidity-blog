---
id: 3740
title: 'Ethereum R&#038;D Roundup: Valentine&#8217;s Day Edition'
date: 2017-02-14T19:27:28+00:00
author: Vitalik Buterin
layout: post
guid: https://blog.ethereum.org/?p=3740
permalink: /2017/02/14/ethereum-rnd-roundup-valentines-day-edition/
---
During the last month and a half, the Ethereum core development and research teams have been building upon the progress made in the last year, and with the specter of last year's security issues now well behind us, work has begun in full force on implementing the Metropolis hard fork.

First, ongoing progress on the collaboration with the Zcash team and the implementation of zk-SNARKs:
<ul>
 	<li>The full series of "zk-SNARK explainer" posts from myself is now finished, see <a href="https://medium.com/@VitalikButerin/quadratic-arithmetic-programs-from-zero-to-hero-f6d558cea649">https://medium.com/@VitalikButerin/quadratic-arithmetic-programs-from-zero-to-hero-f6d558cea649</a>, <a href="https://medium.com/@VitalikButerin/exploring-elliptic-curve-pairings-c73c1864e627">https://medium.com/@VitalikButerin/exploring-elliptic-curve-pairings-c73c1864e627</a> and <a href="https://medium.com/@VitalikButerin/zk-snarks-under-the-hood-b33151a013f6">https://medium.com/@VitalikButerin/zk-snarks-under-the-hood-b33151a013f6</a></li>
 	<li>An update on implementation from Christian Reitwiessner: <a href="https://blog.ethereum.org/2017/01/19/update-integrating-zcash-ethereum/">https://blog.ethereum.org/2017/01/19/update-integrating-zcash-ethereum/</a></li>
</ul>
On the proof of stake front, myself and Vlad and others have continued to solidify the Casper specification and converge on a roadmap. A key focus of our work has been on a notion of "protocol armor", which can turn many classes of traditional Byzantine-fault-tolerant consensus algorithms into "attributable-fault consensus algorithms", where if there is a protocol failure then not only do you know that a large number of validators were faulty, but you also know whom to blame. The work has not yet been fully written out, though it will be further formalized and presented soon, and anyone interested is free to follow along at <a href="https://gitter.im/ethereum/casper-scaling-and-protocol-economics">https://gitter.im/ethereum/casper-scaling-and-protocol-economics</a>.

A post on parametrizing Casper was written here: <a href="https://medium.com/@VitalikButerin/parametrizing-casper-the-decentralization-finality-time-overhead-tradeoff-3f2011672735">https://medium.com/@VitalikButerin/parametrizing-casper-the-decentralization-finality-time-overhead-tradeoff-3f2011672735</a>

We had <a href="https://github.com/ethereum/pm/issues/3">two</a> core dev <a href="https://github.com/ethereum/pm/issues/5">meetings</a>, and have approved the following EIPs for likely inclusion into Metropolis:
<ul>
 	<li><a href="http://github.com/ethereum/EIPs/issues/86">http://github.com/ethereum/EIPs/issues/86</a> (abstraction)</li>
 	<li><a href="http://github.com/ethereum/EIPs/issues/100">http://github.com/ethereum/EIPs/issues/100</a> (uncle mining incentive fix)</li>
 	<li><a href="https://github.com/ethereum/EIPs/issues/196">https://github.com/ethereum/EIPs/issues/196</a> and <a href="https://github.com/ethereum/EIPs/issues/197">https://github.com/ethereum/EIPs/issues/197</a> (elliptic curve pairings, the key ingredient for on-chain zk-SNARK verification)</li>
 	<li><a href="https://github.com/ethereum/EIPs/issues/198">https://github.com/ethereum/EIPs/issues/198</a> (bigint modular exponentation, for use in RSA and various other big-number-based cryptographic algorithms)</li>
 	<li><a href="https://github.com/ethereum/EIPs/pull/206">https://github.com/ethereum/EIPs/pull/206</a> (REVERT opcode, equivalent to the "throw" keyword in Solidity but without burning extra gas)</li>
 	<li><a href="https://github.com/ethereum/EIPs/pull/210">https://github.com/ethereum/EIPs/pull/210</a> (blockhash refactoring, serving the triple duty of (i) simplifying consensus logic, (ii) allowing the BLOCKHASH opcode to refer much further back, and (iii) enabling much more efficient and secure light client initial-syncing protocols)</li>
</ul>
Additionally, there were a few changes to the EIP process itself: <a href="https://www.reddit.com/r/ethereum/comments/5rp8mr/update_to_eip_ethereum_improvement_proposal_system/">https://www.reddit.com/r/ethereum/comments/5rp8mr/update_to_eip_ethereum_improvement_proposal_system/</a>

Work on Mist, Swarm, ENS and associated infrastructure is continuing at a rapid pace; Swarm is now at the stage where it can serve the wallet app, though the incentivization logic is not yet in place.
<ul>
 	<li>Our new developer Victor Maia is working on Swarm integration in Mist; you can follow progress here: <a href="https://github.com/ethereum/mist/pull/1661">https://github.com/ethereum/mist/pull/1661</a></li>
 	<li>The work on ENS includes a <a href="https://github.com/ethereum/ens">name resolver</a>, a registrar, a <a href="https://github.com/maurelian/eth-registrar-ens">Javascript implementation library</a> and <a href="https://github.com/ethereum/ens-registrar-dapp">a user-facing dapp</a>. Support from major Ethereum wallets is coming soon.</li>
</ul>
Work on programming languages is also moving forward:
<ul>
 	<li>Solidity is adding a fully specified way to access the compiler input, settings and output: <a href="https://solidity.readthedocs.io/en/latest/using-the-compiler.html#compiler-input-and-output-json-description">https://solidity.readthedocs.io/en/latest/using-the-compiler.html#compiler-input-and-output-json-description</a></li>
 	<li>There are plans (not yet completed) to add an intermediate language to Solidity to help with understanding what the compiler does and auditing compiler output</li>
 	<li>In the community, the <a href="https://gitter.im/fp-ethereum/Lobby">fp-ethereum functional programming initiative</a> got some attention</li>
 	<li>Viper saw another round of improvements, including support for unit types (timestamp, timedelta, wei, wei per second, etc), bytearrays and more builtin functions: <a href="https://github.com/ethereum/viper/commits/master">https://github.com/ethereum/viper/commits/master</a></li>
 	<li>The Javascript-based <a href="http://remix.ethereum.org/">development environment Remix</a> added much more powerful debugging support</li>
</ul>
Work on implementations is progressing:
<ul>
 	<li><span style="font-weight: 400;">The cpp-ethereum project is experimenting with the performance of the EVM. Some results suggest that the EVM might be able to get up to a small linear factor close to “raw metal” with the more structured </span><a href="https://github.com/ethereum/EIPs/pull/187/files"><span style="font-weight: 400;">EVM 1.5 model</span></a><span style="font-weight: 400;">.</span></li>
 	<li>With its newest 1.5.9 release, Geth supports hardware wallets: <a href="https://github.com/ethereum/go-ethereum/releases/tag/v1.5.9">https://github.com/ethereum/go-ethereum/releases/tag/v1.5.9</a></li>
 	<li>Work on integrating the recent pyethereum changes into pyethapp is continuing: <a href="https://github.com/ethereum/pyethereum/commits/state_revamp_for_stable">https://github.com/ethereum/pyethereum/commits/state_revamp_for_stable</a></li>
</ul>
We wish everyone a happy Valentine's day!