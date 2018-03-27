---
id: 3255
title: 'Transaction spam attack: Next Steps'
date: 2016-09-22T21:31:35+00:00
author: Vitalik Buterin
layout: post
guid: https://blog.ethereum.org/?p=3255
permalink: /2016/09/22/transaction-spam-attack-next-steps/
dsq_thread_id:
  - "5165227399"
---
Today the network was attacked by a transaction spam attack that repeatedly called the EXTCODESIZE opcode (see trace sample <a href="http://vitalik.ca/files/1ksteps.txt">here</a>), thereby creating blocks that take up to ~20-60 seconds to validate due to the ~50,000 disk fetches needed to process the transaction. The result of this was a ~2-3x reduction in the rate of block creation while the attack was taking place; there was NO consensus failure (ie. network fork) and neither the network nor any client at any point fully halted. The attack has since, as of the time of this writing, mostly halted, and the network has for the time being recovered.

The short-term fix is for users, including miners, enterprise users (including exchanges) and individuals to run geth with the flags:

<code>--cache 1024 --targetgaslimit 1500000 --gasprice 20000000000</code>

Or Parity with the flags:

<code>--cache-size-db 1024 --gas-floor-target 1500000 --gasprice 20000000000 --gas-cap 1500000</code>

This (i) increases the cache size, reducing the number of disk reads that nodes need to make, and (ii) votes the gas limit down by ~3x, reducing the maximum processing time of a block by a similar factor.

In the medium term (ie. several days to a week), we are actively working on several fixes for the Go client that should both provide a more stable resolution for the present issue and mitigate the risk of similar attacks, including:
<ul>
 	<li>A change to miner software that automatically temporarily cuts the gas limit target by 2x when the miner sees a block that takes longer than 5 seconds to process, allowing for adjustments similar to what was coordinated today to happen automatically (see <a href="https://github.com/ethereum/go-ethereum/pull/3025">here</a> for a pull request; note that this is a miner strategy change and NOT a soft fork or hard fork)</li>
 	<li>Numerical tweaks to cache settings</li>
 	<li>Adding additional caches</li>
 	<li>Adding an additional cache for EXTCODESIZE specifically (as it is likely that EXTCODESIZE reads are several times slower than other IO-heavy operations since the contracts that are being read are ~18 KB long)</li>
 	<li>An on-disk cache of state values that allows them to be more quickly (ie. <code>O(log(n))</code> speedup) accessed</li>
</ul>
We are also exploring the option of replacing the leveldb database with something more performant and optimized for our use case, though such a change would not come soon. The Parity team is working on their own performance improvements.

In the longer term, there are low-level protocol changes that can also be explored. For example it may be wise to add a feature to Metropolis to increase the gas costs of opcodes that require reads of account state (SLOAD, EXTCODESIZE, CALL, etc), and especially read operations that read external accounts; increasing the gas cost of all of these operations to at least 500 would likely be sufficient, though care would need to be taken to avoid breaking existing contracts (eg. simultaneously implementing <a href="https://github.com/ethereum/EIPs/issues/90">EIP 90</a> would suffice).

This would put a much lower upper bound on the maximum number of bytes that a transaction may read, increasing safety against all potential attacks of this kind, and reducing the size of Merkle proofs and hence improving security for both light clients and sharding as a side effect. At present, we are focusing on the more immediate software-level changes; however, in the long term such proposals should be discussed and contract developers should be aware that changes of this sort may take place.

<div id="disqus_link_container"></div>
<script>jQuery(document).ready(function() { EthBlogUtils.display_disqus_link();});</script>