---
id: 3276
title: Announcement of imminent hard fork for EIP150 gas cost changes
date: 2016-10-13T23:25:27+00:00
author: Martin Swende
layout: post
guid: https://blog.ethereum.org/?p=3276
permalink: /2016/10/13/announcement-imminent-hard-fork-eip150-gas-cost-changes/
dsq_thread_id:
  - "5221149730"
---
During the last couple of weeks, the Ethereum network has been the target of a sustained attack. The attacker(s) have been very crafty in locating vulnerabilities in the client implementations as well as the protocol specification.

While the recent patches have led to an overall increased resiliency in the client implementations, the attacks have also demonstrated that a lower-level change to the EVM pricing model is needed.

For many users, the most visible consequence is probably that they are having difficulties getting transactions included in blocks, and full nodes are facing memory limitations in managing the bloated state.

This is our strategy to address these issues:
<ul>
 	<li>As a temporary measure to minimize the effects of the most recent attack, we recommend all miners to lower the gaslimit to 500K gas.</li>
 	<li>A hard-fork based on <a href="https://github.com/ethereum/EIPs/issues/150">EIP 150 version 1c</a> will be put into effect at block <del>2457000</del> [see below]. This will reprice certain operations to correspond better to the underlying computational complexity.</li>
 	<li>A second hard-fork will follow shortly after, aimed at reverting the current "state-bloat" introduced by the attacks. This second fork will serve to remove accounts which are empty; lacking code, balance, storage and nonce == 0.</li>
</ul>
We have implemented the changes required in the clients and are currently extending and adding tests in an effort to prevent the introduction of consensus-breaking vulnerabilities.

And as a reminder, the <a href="https://bounty.ethereum.org/">Ethereum Bug Bounty</a> is open and includes the new hardfork-implementations.

EDIT: Fork block has been moved to <strong>2463000</strong> in order to accommodate even more testing.

<div id="disqus_link_container"></div>
<script>jQuery(document).ready(function() { EthBlogUtils.display_disqus_link();});</script>