---
id: 3951
title: Byzantium HF Announcement
date: 2017-10-12T07:51:59+00:00
author: Ethereum Team
layout: post
guid: https://blog.ethereum.org/?p=3951
permalink: /2017/10/12/byzantium-hf-announcement/
---
The Ethereum network will be undergoing a planned hard fork at <strong>block number 4.37mil (4,370,000)</strong>, which will likely occur between 12:00 UTC and 13:00 UTC on <strong>Monday, October 16, 2017</strong>. The Ropsten test network underwent a hard fork on September 19th (UTC) at block number 1.7mil (1,700,000). A countdown timer can be seen at <a href="https://fork.codetract.io/">https://fork.codetract.io/</a>.
<h2>As a user, what do I need to do?</h2>
Download the latest version of your Ethereum client:
<ul>
 	<li><a href="https://github.com/ethereum/mist/releases">Latest version of Ethereum Wallet/Mist</a>.</li>
 	<li><a href="https://geth.ethereum.org/downloads/">Latest geth client (v 1.7.2)</a></li>
 	<li><a href="https://github.com/paritytech/parity/releases/tag/v1.7.6">Latest Parity client (v 1.7.6)</a></li>
 	<li><a href="https://github.com/ether-camp/ethereum-harmony/releases/tag/v2.1b56">Latest Harmony client (v 2.1.0)</a></li>
</ul>
<h2>What if I am using a web or mobile Ethereum wallet like MyEtherWallet or Jaxx?</h2>
Ethereum websites and mobile applications that allow you to store ether and/or make transactions are running their own Ethereum client infrastructure to facilitate their services. If you use a third-party web-based or mobile Ethereum wallet, your wallet provider may need to update for the hard fork. It is recommended that you check with them to see what actions they are taking to update for the hard fork and if they are asking their users to take other steps.
<h2>What is a hard fork in Ethereum?</h2>
A hard fork is a change to the underlying Ethereum protocol, creating new rules to improve the system. The protocol changes are activated at a specific block number. All Ethereum clients need to upgrade, otherwise they will be stuck on an incompatible chain following the old rules.
<h2>What happens if I do not participate in the hard fork?</h2>
If you are using an Ethereum client that is not updated for the upcoming hard fork, your client will sync to the pre-fork blockchain once the fork occurs. You will be stuck on an incompatible chain following the old rules, without replay protection against the main network. Old clients will be able to construct transactions, but will not be able to see the effects of those transactions.
<h2>What is Metropolis, Byzantium, and Constantinople?</h2>
Metropolis is a planned Ethereum development phase that includes two hard forks: Byzantium and Constantinople. Byzantium is occurring at block number 4.37mil. Constantinople does not currently have a release date, but is expected in 2018.
<h2>What changes are included in the Byzantium hard fork?</h2>
The following upgrades are included:
<ul>
 	<li>Addition of ‘REVERT’ opcode, which permits error handling without consuming all gas (<a href="https://github.com/ethereum/EIPs/pull/206">EIP 140</a>)</li>
 	<li>Transaction receipts now include a status field to indicate success or failure <a href="https://github.com/ethereum/EIPs/pull/658">EIP 658</a>)</li>
 	<li>Elliptic curve addition and scalar multiplication on alt_bn128 (<a href="https://github.com/ethereum/EIPs/pull/213">EIP 196</a>) and pairing checks (<a href="https://github.com/ethereum/EIPs/pull/212">EIP 197</a>), permitting ZK-Snarks and other cryptographic mathemagic™</li>
 	<li>Support for big integer modular exponentiation (<a href="https://github.com/ethereum/EIPs/pull/198">EIP 198</a>), enabling RSA signature verification and other cryptographic applications</li>
 	<li>Support for variable length return values (<a href="https://github.com/ethereum/EIPs/pull/211">EIP 211</a>)</li>
 	<li>Addition of the ‘STATICCALL’ opcode, permitting non-state-changing calls to other contracts (<a href="https://github.com/ethereum/EIPs/pull/214">EIP 214</a>)</li>
 	<li>Changes to the difficulty adjustment formula to take uncles into account (<a href="https://github.com/ethereum/EIPs/issues/100">EIP 100</a>)</li>
 	<li>Delay of the ice age / difficulty bomb by 1 year, and reduction of block reward from 5 to 3 ether (<a href="https://github.com/ethereum/EIPs/pull/669">EIP 649</a>)</li>
</ul>
<h2>What if something goes wrong?</h2>
In the event that a critical bug is discovered, the following communication channels will be utilized:
<ul>
 	<li><a href="https://blog.ethereum.org/">Ethereum Foundation blog</a></li>
 	<li><a href="https://twitter.com/ethereumproject">Ethereum Foundation Twitter account</a></li>
</ul>
<h2>Important Note for Dapp Developers</h2>
<strong>The way to detect failed transactions will change with Byzantium, even for contracts created before the Byzantium hard fork is enacted.</strong> After the fork, eth.getTransactionReceipt(...) will return a status field. The status field has a value of 0 when a transaction has failed and 1 when the transaction has succeeded. For more information, please see <a href="https://ethereum.stackexchange.com/questions/28077/how-do-i-detect-a-failed-transaction-after-the-byzantium-fork-as-the-revert-opco/28078?stw=2#28078">this post on the Ethereum StackExchange</a>.

A big thanks to the Ethereum development community across all clients and platforms who came together to provide input, thoughts, and contributions for this upgrade.

<strong>DISCLAIMER</strong> This is an emergent and evolving highly technical space. If you choose to implement the recommendations in this post and continue to participate, you should make sure you understand how it impacts you. You should understand that there are risks involved including but not limited to risks like unexpected bugs. By choosing to implement these recommendations, you alone assume the risks of the consequences. This post and recommendations are not a sale of any kind and do not create any warranties of any kind including but not limited to any relating to the Ethereum network or the Ethereum clients referred to herein.