---
id: 1870
title: An Analysis of the First 100000 Blocks
date: 2015-08-18T14:30:56+00:00
author: Gustav Simonsson
layout: post
guid: https://blog.ethereum.org/?p=1870
permalink: /2015/08/18/frontier-first-100k-blocks/
dsq_thread_id:
  - "3990136325"
---
After a successful launch, Frontier passed it's 100000th block earlier this week. Here are some interesting stats from these first blocks.

<strong>The first 50 block times (in seconds):</strong>
<img id="__wp-temp-img-id" class=" aligncenter" title="" src="http://i.imgur.com/Lz9aS0p.png" alt="" width="670" height="422" />

Right after genesis, it took 29 seconds for the second block to appear, then 31 and 29 seconds for the next couple of blocks. Not too surprising as the miners scrambled with their setups as the final genesis block became known.

After these first blocks, we see the block time drop significantly at the onslaught of larger miners, flooring at 1 second that is the limit enforced by the consensus protocol.

Let's now look at the average block time (every 200th block from genesis to block 10000):

<img id="__wp-temp-img-id" class=" aligncenter" title="" src="http://i.imgur.com/O6G57xz.png" alt="" width="785" height="482" />

We can see the hashing power really attacking the network after the first few hundred blocks, with block times at 1-2 seconds as the difficulty adjustment algorithm is catching up. This algorithm either increases or decreases the difficulty by diff/2048 each block, depending on the block time.

After a few thousand blocks, we see the difficulty going up enough for block times to get into the 4-5 second range. After 9k blocks, we're starting to approach the target time of 15 seconds.

<strong>Biggest miners in first 100k blocks (address, blocks, %) :</strong>
<pre style="text-align: left">0xef247e639d49461d25f57e9362cade3120910ce0 851   0.81%
0x790b8a3ce86e707ed0ed32bf89b3269692a23cc1 914   0.87%
0x0037ce3d4b7f8729c8607d8d0248252be68202c0 949   0.90%
0xbb12b5a9b85d4ab8cde6056e9c1b2a4a337d2261 1102  1.04%
0x580992b51e3925e23280efb93d3047c82f17e038 1129  1.07%
0xf2d2aff1320476cb8c6b607199d23175cc595693 1141  1.08%
0x47ff6576639c2e94762ea5443978d7681c0e78dc 1159  1.10%
0x1b7047b4338acf65be94c1a3e8c5c9338ad7d67c 1335  1.26%
0xeb1325c8d9d3ea8d74ac11f4b00f1b2367686319 1446  1.37%
0xbcb2e3693d246e1fc00348754334badeb88b2a11 1537  1.45%
0xa50ec0d39fa913e62f1bae7074e6f36caa71855b 1692  1.60%
0xf8e0ca3ed80bd541b94bedcf259e8cf2141a9523 2437  2.31%
0x9746c7e1ef2bd21ff3997fa467593a89cb852bd0 3586  3.39%
0x88d74a59454f6cf3b51ef6b9136afb6b9d405a88 4292  4.06%
0xbb7b8287f3f0a933474a79eae42cbca977791171 8889  8.41%
0xf927a40c8b7f6e07c5af7fa2155b4864a4112b13 9151  8.66%
0xe6a7a1d47ff21b6321162aea7c6cb457d5476bca 11912 11.28%

</pre>
While one can never know for sure whether a single miner uses multiple coinbase addresses, assuming they use a single one, we have a pretty even distribution of hash power over the first 100k blocks. 0xe6a7a1d47ff21b6321162aea7c6cb457d5476bca with 11% of hashing power is <a href="http://ethpool.org/faq">ethpool</a>, the first mining pool for Ethereum. Next we have two large miners with around 8%. After these three the distribution is quite even, with most miners having less than 1% of total hashing power.

However, things develop rapidly in the Ethereum world and if we look at the last 15k blocks we see:
<pre>0x580992b51e3925e23280efb93d3047c82f17e038 327  2.2%
0xbb7b8287f3f0a933474a79eae42cbca977791171 496  3.3%
0xf927a40c8b7f6e07c5af7fa2155b4864a4112b13 612  4.1%
0x790b8a3ce86e707ed0ed32bf89b3269692a23cc1 674  4.5%
0xe6a7a1d47ff21b6321162aea7c6cb457d5476bca 5775 38.5%</pre>
Here it's clear ethpool is currently having almost 40% of the hashing power, and the 2nd and 3rd biggest miners are down to 4%

<strong>Blocks in a row</strong>

Another interesting stat is to look at continuous sets of blocks from the same miner. This can give some insight into how hashing power and latency is coming into play for larger miners.

Early after launch, most blocks in a row we saw was 6, for example blocks 1578, 1579, 1580, 1581, 1582 and 1583 mined by 0x9dfc0377058b7b9eb277421769b56df1395705f0.

0xbb7b8287f3f0a933474a79eae42cbca977791171 also mined 6 blocks a few times, for example blocks 656, 657, 658, 659, 660 and 661.

This miner also mined 5, 4, 3 and 2 blocks in a row on a number of occasions, not too surprising with 21% of the total hashing power at the time.

This happened very early after launch when difficulty was rapidly increasing and many miners had not yet joined the network. As hashing power evened out after block 5000 we did not see more than 4 blocks in a row for some time.

However, recently the most blocks in a row is 10, for example blocks 103049, 103050, 103051, 103052, 103053, 103054, 103055, 103056, 103057, 103058 by ethpool. Excluding ethpool, no miner has had more than 6 blocks in a row.

Stay tuned for more statistics from the Frontier network as we observe it over the next months!

<a href="https://github.com/Gustav-Simonsson">Gustav Simonsson</a> is a developer in the Ethereum Security and Go teams.

&nbsp;