---
id: 3291
title: 'FAQ: Upcoming Ethereum Hard Fork'
date: 2016-10-18T01:05:51+00:00
author: Hudson Jameson
layout: post
guid: https://blog.ethereum.org/?p=3291
permalink: /2016/10/18/faq-upcoming-ethereum-hard-fork/
dsq_thread_id:
  - "5231279744"
---
The Ethereum network will be undergoing a hard fork at block number 2463000, which will likely occur between 12:00 and 13:00 UTC on Tuesday, October 18, 2016. A countdown timer can be seen at <a href="https://fork.codetract.io/" target="_blank">https://fork.codetract.io/</a>.

## As a user, what do I need to do?

<span style="font-weight: 400;">Download the latest version of your Ethereum client:</span>
<ul>
 	<li><a href="https://github.com/ethereum/mist/releases/tag/v0.8.6" target="_blank">Latest version of Ethereum Wallet/Mist (v0.8.6)</a></li>
 	<li><a href="https://github.com/ethereum/go-ethereum/releases/latest" target="_blank">Latest geth client (v1.4.18)</a></li>
 	<li><a href="https://github.com/ethcore/parity/releases/latest" target="_blank">Latest Parity client (v1.3.8)</a></li>
 	<li><a href="https://github.com/ethereum/ethereumj/releases/latest" target="_blank">Latest ethereumJ client (v1.3.6)</a></li>
</ul>

## What happens if I do not participate in the hard fork?

<strong>If you are using an Ethereum client that is not updated for the upcoming hard fork, your client will sync to the pre-fork blockchain once the fork occurs. You will be stuck on an incompatible chain following the old rules and you will be unable to send ether or operate on the post-fork Ethereum network.</strong>

## What is a hard fork in Ethereum-land?

A hard fork is a change to the underlying Ethereum protocol, creating new rules to improve the system. All Ethereum clients need to upgrade; otherwise they will be stuck on an incompatible chain following the old rules. The decentralized nature of blockchain systems makes a hard fork upgrade more difficult. Hard forks in a blockchain require cooperation and communication with the community, as well as with the developers of the various Ethereum clients in order for the transition to go smoothly.

## What happens during a hard fork?

After consensus is reached on what changes should be included in a hard fork, changes to the protocol are written into the various Ethereum clients, such as geth, Parity, and ethereumJ. The protocol changes are activated at a specific block number. Any nodes that have not been upgraded to the new ruleset will be abandoned on the old chain where the previous rules continue to exist.

## Why do we need a hard fork?

Since September 18th (UTC), the Ethereum network has been under attack by a person or group resulting in large delays before transactions were processed. The network is currently filled with pending transactions which is causing users delays in processing their transactions. You can think of this as a denial of service (DoS) attack on the Ethereum blockchain.

Every operation that an Ethereum contract performs on the network is given a price or gas fee. Using the ADD operation is less computationally expensive than performing a complex operation such as hashing a number using SHA256. The attacker performed a DoS attack by repeatedly calling certain operation codes (opcodes) in their smart contracts that are computationally difficult for clients to process, but very cheap to add to the network. In order to prevent the attacker(s) from continuing to flood the network with low-priced contracts with high computational cost, we are raising the price of certain operations.

## Why are we doing two hard forks and what will the effects be?

The two hard forks each address different problems that have arisen from the attacks. The first hard fork is meant to address urgent network health issues concerning underpriced operation codes. As described in Ethereum Improvement Proposal 150, “EIP 150 Hard Fork,” the first hard fork is set to occur at block number 2463000 and will adjust the price of underpriced opcodes involved in the attack. The second hard fork is expected to address a number of less urgent matters such as removing empty accounts which the attacker used to flood the Ethereum network, and which caused the size of the blockchain to inflate. The second hard fork is still being discussed.

After the second hard fork has been implemented, there will likely be a “clean-up period” during which time there may continue to be delays and difficulties processing transactions and syncing until the blockchain state has been fully debloated.

## How will the EIP 150 hard fork affect contracts?

See following commentary: <a href="https://www.reddit.com/r/ethereum/comments/57p0bv/a_quick_note_on_how_the_call_gas_cost_increase/" target="_blank">https://www.reddit.com/r/ethereum/comments/57p0bv/a_quick_note_on_how_the_call_gas_cost_increase/</a>

## Where can I find more technical details on what the two new hard forks will change in the protocol?

The respective changes to Ethereum protocol are documented in the GitHub repository for <a href="https://github.com/ethereum/EIPs" target="_blank">Ethereum Improvement Proposals</a>
The following two EIPs describe the current plans for the upcoming hard forks:
<ul>
	<li><a href="https://github.com/ethereum/EIPs/issues/150" target="_blank">EIP 150 version 1c: “Long-term gas cost changes for IO-heavy operations to mitigate transaction spam attacks”</a></li>
	<li><a href="https://github.com/ethereum/EIPs/issues/158" target="_blank">EIP 158: “State clearing”</a></li>
</ul>
<strong>A big thanks to the Ethereum community for their patience and understanding, and to all Ethereum developers across all clients and platforms who came together to provide input, thoughts, and contributions to address stopping the attacks and helping to improve the platform.</strong>

<div id="disqus_link_container"></div>
<script>jQuery(document).ready(function() { EthBlogUtils.display_disqus_link();});</script>