---
id: 2908
title: To fork or not to fork
date: 2016-07-15T10:56:30+00:00
author: Jeffrey Wilcke
layout: post
guid: https://blog.ethereum.org/?p=2908
permalink: /2016/07/15/to-fork-or-not-to-fork/
dsq_thread_id:
  - "4987346931"
---
<span style="font-weight: 400;">The DAO, though not a product developed by the Ethereum Foundation, has been a hot topic as of late, both internally in the organisation as well as within our community. The Hard Fork is a delicate topic and the way we see it, no decision is the right one. As this is not a decision that can be made by the foundation or any other single entity, we again turn towards the community to assess its wishes in order to provide the most appropriate protocol change.
</span>

<span style="font-weight: 400;">The specification proposed for the hard fork that is being implemented in the Geth client is as follows:</span>

<span style="font-weight: 400;">The DAO (</span><code><span style="font-weight: 400;">0xbb9bc244d798123fde783fcc1c72d3bb8c189413</span></code><span style="font-weight: 400;">), its extraBalance (</span><code><span style="font-weight: 400;">0x807640a13483f8ac783c557fcdf27be11ea4ac7a</span></code><span style="font-weight: 400;">), all children of the DAO creator (</span><code><span style="font-weight: 400;">0x4a574510c7014e4ae985403536074abe582adfc8</span></code><span style="font-weight: 400;">) and the extrabalance of each child are encoded into a list L at block 1880000. The contents of L can be viewed </span><a href="https://gist.github.com/gavofyork/af747a034fbee2920f862ed352d32347"><span style="font-weight: 400;">here</span></a><span style="font-weight: 400;">. At the beginning of block X (X = 1920000, on July 20 or 21 depending on your time zone), all ether throughout all accounts in L will be transferred to contract account C, which is at (</span><code><span style="font-weight: 400;">0xbf4ed7b27f1d666546e30d74d50d173d20bca754</span></code><span style="font-weight: 400;">). You can verify the solidity source code of C on <a href="https://etherscan.io/address/0xbf4ed7b27f1d666546e30d74d50d173d20bca754#code">etherscan</a></span><span style="font-weight: 400;">. From this contract, DAO token holders can submit their DAO in order to withdraw ETH at a rate of 1 ETH = 100 DAO. The extrabalance, as well as some additional ether that remains due to complications in the interactions between the re-entrancy exploit and the splitting mechanism, will be withdrawable by the DAO curator to be distributed as appropriate to cover all edge cases.</span>

<span style="font-weight: 400;">Additional information to facilitate verification of the fork spec and implementation is expected to be released separately by the community; consensus code in Geth that implements the fork logic is covered by the bug <a href="https://bounty.ethereum.org/">bounty program</a>.</span>

<span style="font-weight: 400;">Unfortunately time limits require swift adoption before a protocol change becomes impractical. The community tool </span><a href="http://carbonvote.com"><span style="font-weight: 400;">carbonvote</span></a><span style="font-weight: 400;"> will be used to set the default fork option for Geth. At block number 1894000 the votes will be tallied, and the outcome will determine whether the default is set <em>to fork</em> or <em>not to fork</em>. Then merging the </span><span style="font-weight: 400;"><a href="https://github.com/ethereum/go-ethereum/pulls?q=is%3Apr+is%3Aopen+label%3Adao-fork-part">DAO fork PRs</a> will proceed</span><span style="font-weight: 400;">, followed shortly by a release for both Geth and Mist. Users with business-critical applications who need to update quickly should frequently check the blog and social media for ongoing updates.</span>

<div id="disqus_link_container"></div>
<script>jQuery(document).ready(function() { EthBlogUtils.display_disqus_link();});</script>