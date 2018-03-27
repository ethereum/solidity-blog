---
id: 3246
title: The Ethereum network is currently undergoing a DoS attack
date: 2016-09-22T16:46:27+00:00
author: Jeffrey Wilcke
layout: post
guid: https://blog.ethereum.org/?p=3246
permalink: /2016/09/22/ethereum-network-currently-undergoing-dos-attack/
dsq_thread_id:
  - "5164533908"
---
URGENT ALL MINERS: The network is under attack. The attack is a computational DDoS, ie. miners and nodes need to spend a very long time processing some blocks. This is due to the EXTCODESIZE opcode, which has a fairly low gasprice but which requires nodes to read state information from disk; the attack transactions are calling this opcode roughly 50,000 times per block. The consequence of this is that the network is greatly slowing down, but there is NO consensus failure or memory overload. We have currently identified several routes for a more sustainable medium-term fix and have developers working on implementation.

It is highly reccomended to switch to Parity mining. Use these settings:

<code>--cache-size-db 1024 --gas-floor-target 1000000 --gasprice 50000000000</code>

Parity mining guides:
https://docs.google.com/document/d/1j3v8bKjXQcMNNdx5Juwo3PRRCcHy8XcA6R9-0W7NCso/edit
https://github.com/ethcore/parity/wiki/Mining
If you continue to use geth, use the following settings:

<code>--cache 1024 --targetgaslimit 1000000 --gasprice 50000000000</code>

<div id="disqus_link_container"></div>
<script>jQuery(document).ready(function() { EthBlogUtils.display_disqus_link();});</script>