---
id: 3483
title: 'Hard Fork No. 4: Spurious Dragon'
date: 2016-11-18T18:19:46+00:00
author: Hudson Jameson
layout: post
guid: https://blog.ethereum.org/?p=3483
permalink: /2016/11/18/hard-fork-no-4-spurious-dragon/
---
The Ethereum network will be undergoing a hard fork at block number 2,675,000, which will likely occur between 15:00 and 16:00 UTC on Tuesday, November 22, 2016. A countdown timer can be seen at <a href="https://fork.codetract.io/" target="_blank">https://fork.codetract.io/</a>. The Morden test network will be undergoing a hard fork at block number 1,885,000.

## As a user, what do I need to do?

<span style="font-weight: 400;">Download the latest version of your Ethereum client:</span>
<ul>
 	<li><a href="https://github.com/ethereum/mist/releases/latest" target="_blank">Latest version of Ethereum Wallet/Mist (v0.8.7)</a></li>
 	<li><a href="https://github.com/ethereum/go-ethereum/releases/latest" target="_blank">Latest geth client (v1.5.2)</a></li>
 	<li><a href="https://github.com/ethcore/parity/releases/latest" target="_blank">Latest Parity client (v1.4.4)</a></li>
 	<li><a href="https://github.com/cryptape/ruby-ethereum/releases/latest" target="_blank">Latest ruby-ethereum client (v0.11.0)</a></li>
</ul>
## What happens if I do not update my client?
<strong>If you are using an Ethereum client that is not updated for the upcoming hard fork, your client will sync to the pre-fork blockchain once the fork occurs. You will be stuck on an incompatible chain following the old rules and you will be unable to send ether or operate on the post-fork Ethereum network.</strong>

<strong>Importantly, if your client is not updated, it also means that any transactions you make will still be susceptible to replay attacks.</strong>

## What if I am using a web or mobile Ethereum wallet like MyEtherWallet or Jaxx?
Ethereum websites and mobile applications that allow you to store ether and/or make transactions are running their own Ethereum client infrastructure to facilitate their services. Generally, you do not need to do anything if you use a third party web based or mobile Ethereum wallet. However, you should still check with your web or mobile Ethereum wallet provider to see what actions they are taking to update for the hard fork and if they are asking their users to take other steps.

<strong>In particular, you should ensure that transactions are generated with the new replay-protected EIP 155 scheme.</strong>

## What do I do if my Ethereum client is having trouble syncing to the blockchain?
Make sure you have downloaded the latest version of your Ethereum client.
<ul>
 	<li>If you are using Geth or Mist, <a href="http://ethereum.stackexchange.com/questions/603/help-with-very-slow-mist-sync" target="_blank">refer to this Ethereum StackExchange thread</a>.</li>
 	<li>If you are using Parity, <a href="https://github.com/ethcore/parity/wiki/FAQ#what-can-i-do-when-parity-has-trouble-getting-in-sync" target="_blank">refer to this section of the Parity wiki</a>.</li>
</ul>
## Why are we proposing to hard fork the network?
"Spurious Dragon" is the second hard fork of the two-round hard fork response to the DoS attacks on the Ethereum network in September and October. The previous hard fork (a.k.a "Tangerine Whistle") <a href="https://blog.ethereum.org/2016/10/18/faq-upcoming-ethereum-hard-fork/" target="_blank">addressed immediate network health issues due to the attacks</a>. The upcoming hard fork addresses important but less pressing matters such as further tuning opcode pricing to prevent future attacks on the network, enabling "debloat" of the blockchain state, and adding replay attack protection.

## What changes are a part of this hard fork?
The following <a href="https://github.com/ethereum/EIPs" target="_blank">Ethereum Improvement Proposals (EIPs)</a> describe the protocol changes implemented in this hard fork.
<ul>
 	<li><a href="https://github.com/ethereum/EIPs/issues/155" target="_blank">EIP 155: Replay attack protection</a> - prevents transactions from one Ethereum chain from being rebroadcasted on an alternative chain. For example: If you send 150 test ether to someone from the Morden testnet, that same transaction cannot be replayed on the main Ethereum chain. <strong>Important note:</strong> EIP 155 is <strong>backwards compatible</strong>, so transactions generated with the "pre-Spurious-Dragon" format will still be accepted. However, to ensure you are protected against replay attacks, you will still need to use a wallet solution that implements EIP 155.
Be aware that this backwards compatibility also means that transactions created from alternative Ethereum based blockchains that have not implemented EIP 155 (such as Ethereum Classic) can still be replayed on the main Ethereum chain.</li>
 	<li><a href="https://github.com/ethereum/EIPs/issues/160" target="_blank">EIP 160: EXP cost increase</a> - adjusts the price of `EXP` opcode so it balances the price of `EXP` with the computational complexity of the operation, essentially making it more difficult to slow down the network via computationally expensive contract operations.</li>
 	<li><a href="https://github.com/ethereum/EIPs/issues/161" target="_blank">EIP 161: State trie clearing</a> - makes it possible to remove a large number of empty accounts that were put in the state at very low cost as a result of earlier DoS attacks. With this EIP, 'empty' accounts are removed from the state whenever 'touched' by another transaction. Removal of the empty accounts greatly reduces blockchain state size, which will provide client optimizations such as faster sync times. The actual removal process will begin after the fork by systematically performing `CALL` to the empty accounts that were created by the attacks.</li>
 	<li><a href="https://github.com/ethereum/EIPs/issues/170" target="_blank">EIP 170: Contract code size limit</a> - changes the maximum code size that a contract on the blockchain can have. This update prevents an attack scenario where large pieces of account code can be accessed repeatedly at a fixed gas cost. The maximum size has been set to 24576 bytes, which is larger than any currently deployed contract.</li>
</ul>

---

<strong>DISCLAIMER</strong>
This is an emergent and evolving highly technical space. If you choose to implement the recommendations in this post and continue to participate, you should make sure you understand how it impacts you. You should understand that there are risks involved including but not limited to risks like unexpected bugs. By choosing to implement these recommendations, you alone assume the risks of the consequences.
<div id="disqus_link_container"></div>
<script>jQuery(document).ready(function() { EthBlogUtils.display_disqus_link();});</script>