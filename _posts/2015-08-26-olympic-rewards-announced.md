---
id: 1950
title: Olympic Rewards Announced
date: 2015-08-26T14:30:34+00:00
author: Vitalik Buterin
layout: post
guid: https://blog.ethereum.org/?p=1950
permalink: /2015/08/26/olympic-rewards-announced/
dsq_thread_id:
  - "4068490364"
---
ETH DEV would like to thank everyone for their participation in the Olympic test network, which helped us greatly in stress testing, optimization, and finding bugs in the Ethereum clients, as well as determining what the limits of the current Ethereum system are. To that end, we are pleased to announce the Olympic reward recipients. On top of the rewards announced below, every miner who mined a block in the Olympic chain with block number in the range [310000, 589999] is entitled to receive 0.5 ETH per block, and every miner who mined a block in the Olympic chain with block number in the range [590000, 999999] is entitled to receive 0.05 ETH per block.

<h3>Top miners for blocks 300000-1000000</h3>

To claim these rewards, send a transaction on Olympic from the address that earned the reward to your desired address.

<ol>
<li><code>753f49fb35f0f6f8a7bff0413e376af88f7ff684</code> (57744 blocks) - 2500 ETH</li>
<li><code>891f82ee3e031c5dd85e63b23521b4b9f10c67ff</code> (39526 blocks) - 1250 ETH</li>
<li><code>24365bc872e4166081611bb0b5483526742c6616</code> (39358 blocks) - 625 ETH</li>
<li><code>8ef297b17b65ded8142dbdb106818cd09899a8f5</code> (34142 blocks) - 375 ETH</li>
<li><code>027b1495165fec8cfc49bdb8450ded4cbce4b12e</code> (33305 blocks) - 250 ETH</li>
</ol>

<h3>Top senders for blocks 300000-1000000</h3>

<ol>
<li><code>b4e64290541cbf36159e727dfd8d873f77b42149</code> (5429042 txs) - 2500 ETH</li>
<li><code>00fc572550f3bdfe84f72e3eaa99d02a43f69733</code> (1912815 txs) - 1250 ETH</li>
<li><code>8464ba8c33260c70c9ab03487bee8a0dda35d383</code> (947176 txs) - 625 ETH</li>
<li><code>082d4cdf07f386ffa9258f52a5c49db4ac321ec6</code> (837307 txs) - 375 ETH</li>
<li><code>970f4f9eb29e8831514a3ff9b8eb2bb860d4731c</code> (831275 txs) - 250 ETH</li>
</ol>

<h3>Lowest address at block 848081</h3>

Only addresses with nonzero nonce (ie. that sent at least one transaction) were included.

<ol>
<li><code>0000000000912d1d5757aa9df23dd770b0195a45</code> - 750 ETH</li>
<li><code>00000000097cd8a7d004985ab03af70fe77aa8c2</code> - 500 ETH</li>
<li><code>0000000103026f36d9f2ba6468d2816cd5dce83a</code> - 250 ETH</li>
</ol>

### CONTRACT REWARDS

To claim these, please send a transaction in Olympic specifying your new address to send funds to with the account that created the contract. Please place the nonce which the account had when it created the contract in the transaction data (eg. if the nonce is 46, put one byte 0x2e as the data).

Most storage keys at block 848081:

<ol>
<li><code>332b656504f4eabb44c8617a42af37461a34e9dc</code> (1727960 slots) - 2500 ETH</li>
<li><code>f558470e1ed9e33f2ebf994938722d7f287221f2</code> (685670 slots) - 1250 ETH</li>
<li><code>728d902f5b805a3f49707f36957d876dd6d40e53</code> (657813 slots) - 625 ETH</li>
<li><code>2ea6a457c468ed38b222a2c504a674905b5fcb7e</code> (607504 slots) - 375 ETH</li>
<li><code>b56e8f043e529bca0344a96cbec8b00bc85c7ca6</code> (135250 slots) - 250 ETH</li>
</ol>

Contract with highest nonce at block 848081:

<ol>
<li><code>e35e196a53cabeb4eb4cdd66467427a798d8c24f</code> (4823) - 750 ETH</li>
<li><code>3673cb526c8505002e7ba558140b0c4d60009ac9</code> (2907) - 500 ETH</li>
<li><code>66314103c204504486b6ce587109853f4e99e29b</code> (385) - 250 ETH</li>
</ol>

### Manually issued rewards for all-around good behavior:

PhiStr90 - 3500 ETH
Samuel Lavery - 1750 ETH
Dino Mark - 1250 ETH
Kobi Gurk - 500 ETH

A file containing the amount of ETH to be received by each miner can be found here: <a href="http://vitalik.ca/files/olympic_miners.txt">http://vitalik.ca/files/olympic_miners.txt</a>. To claim these rewards, follow the same process as for top miner/sender rewards described above.