---
id: 3265
title: Uncle Rate and Transaction Fee Analysis
date: 2016-10-31T03:52:04+00:00
author: Vitalik Buterin
layout: post
guid: https://blog.ethereum.org/?p=3265
permalink: /2016/10/31/uncle-rate-transaction-fee-analysis/
---
One of the important indicators of how much load the Ethereum blockchain can safely handle is how the uncle rate responds to the gas usage of a transaction. In all blockchains of the Satoshian proof-of-work variety, any block that is published has the risk of howbecoming a "stale", ie. not being part of the main chain, because another miner published a competing block before the recently published block reached them, leading to a situation where there is a "race" between two blocks and so one of the two will necessarily be left behind.

<img src="https://blog.ethereum.org/wp-content/uploads/2014/07/minernet3.png" alt="Stale block" />

One important fact is that the more transactions a block contains (or the more gas a block uses), the longer it will take to propagate through the network. In the Bitcoin network, one seminal study on this was <a href="http://www.gsd.inesc-id.pt/~ler/docencia/rcs1314/papers/P2P2013_041.pdf">Decker and Wattenhofer (2013)</a>, which found that the average propagation time of a block was about 2 seconds plus another 0.08 seconds per kilobyte in the block (ie. a 1 MB block would take ~82 seconds). A <a href="https://www.bitcoinunlimited.info/resources/feemarket.pdf">more recent Bitcoin Unlimited study</a> showed that this has since reduced to ~0.008 seconds per kilobyte due to transaction propagation technology improvements. We can also see that if a block takes longer to propagate, the chance that it will become a stale is higher; at a block time of 600 seconds, a propagation time increase of 1 second should correspond to an increased 1/600 chance of being left behind.

In Ethereum, we can make a similar analysis, except that thanks to Ethereum's "uncle" mechanic we have very solid data to analyze from. Stale blocks in Ethereum can be re-included into the chain as "uncles", where they receive up to 75% of their original block reward. This mechanic was originally introduced to reduce centralization pressures, by reducing the advantage that well-connected miners have over poorly connected miners, but it also has several side benefits, one of which is that stale blocks are tracked for all time in a very easily searchable database - the blockchain itself. We can take a data dump of blocks 1 to 2283415 (before the Sep 2016 attacks) as a source of data for analysis.

Here is a script to generate some source data: <a href="http://github.com/ethereum/research/tree/master/uncle_regressions/block_datadump_generator.py">http://github.com/ethereum/research/tree/master/uncle_regressions/block_datadump_generator.py</a>

Here is the source data: <a href="http://github.com/ethereum/research/tree/master/uncle_regressions/block_datadump.csv">http://github.com/ethereum/research/tree/master/uncle_regressions/block_datadump.csv</a>

The columns, in order, represent block number, number of uncles in the block, the total uncle reward, the total gas consumed by uncles, the number of transactions in the block, the gas consumed by the block, the length of the block in bytes, and the length of the block in bytes excluding zero bytes.

We can then use this script to analyze it: <a href="http://github.com/ethereum/research/tree/master/uncle_regressions/base_regression.py">http://github.com/ethereum/research/tree/master/uncle_regressions/base_regression.py</a>

The results are as follows. In general, the uncle rate is consistently around 0.06 to 0.08, and the average gas consumed per block is around 100000 to 300000. Because we have the gas consumed of both blocks and uncles, we run a linear regression to estimate of how much 1 unit of gas adds to the probability that a given block will be an uncle. The coefficients turn out to be as follows:

Block 0 to 200k:  3.81984698029e-08
Block 200k to 400k:  5.35265798406e-08
Block 400k to 600k:  2.33638832951e-08
Block 600k to 800k:  2.12445242166e-08
Block 800k to 1000k:  2.7023102773e-08
Block 1000k to 1200k:  2.86409050022e-08
Block 1200k to 1400k:  3.2448993833e-08
Block 1400k to 1600k:  3.12258208662e-08
Block 1600k to 1800k:  3.18276549008e-08
Block 1800k to 2000k:  2.41107348445e-08
Block 2000k to 2200k:  1.99205804032e-08
Block 2200k to 2285k:  1.86635688756e-08

Hence, each 1 million gas worth of transactions that gets included in a block now adds ~1.86% to the probability that that block will become an uncle, though during Frontier this was closer to 3-5%. The "base" (ie. uncle rate of a 0-gas block) is consistently ~6.7%. For now, we will leave this result as it is and not make further conclusions; there is one further complication that I will discuss later at least with regard to the effect that this finding has on gas limit policy.

### Gas pricing

Another issue that touches uncle rates and transaction propagation is gas pricing. In Bitcoin development discussions, a common argument is that block size limits are unnecessary because miners already have a natural incentive to limit their block sizes, which is that every kilobyte they add increases the stale rate and hence threatens their block reward. Given the 8 sec per megabyte impedance found by the Bitcoin Unlimited study, and the fact that each second of impedance corresponds to a 1/600 chance of losing a 12.5 BTC block reward, this suggests an equilibrium transaction fee of 0.000167 BTC per kilobyte assuming no block size limits.

In Bitcoin's environment, there are reasons to be long-term skeptical about the economics of such a no-limit incentive model, as there will eventually be no block reward, and when the only thing that miners have to lose from including too many transactions is fees from their <i>other</i> transactions, then there is an economic argument that the equilibrium stale rate will be <a href="https://www.reddit.com/r/btc/comments/3zz7qt/some_statistics_on_average_vs_median_for_adaptive/cyq938d">as high as 50%</a>. However, there are modifications that can be made to the protocol to limit this coefficient.

In Ethereum's current environment, block rewards are 5 ETH and will stay that way until the algorithm is changed. Accepting 1 million gas means a 1.86% chance of the block becoming an uncle. Fortunately, Ethereum's uncle mechanism has a happy side effect here: the average uncle reward is recently around 3.2 ETH, so 1 million gas only means a 1.86% chance of putting 1.8 ETH at risk, ie. an expected loss of 0.033 ETH and not 0.093 as would be the case without an uncle mechanism. Hence, the current gas prices of ~21 shannon are actually quite close to the "economically rational" gas price of 33 shannon (this is before the DoS attacks and the optimizations arising therefrom; now it is likely even lower).

The simplest way to push the equilibrium gasprice down further is to improve uncle inclusion mechanics and try to get uncles included in blocks as quickly as possible (perhaps by separately propagating every block as a "potential uncle header"); at the limit, if every uncle is included as quickly as possible, the equilibrium gas price would go down to about 11 shannon.

### Is Data Underpriced?

A second linear regression analysis can be done with source code here: <a href="http://github.com/ethereum/research/tree/master/uncle_regressions/tx_and_bytes_regression.py">http://github.com/ethereum/research/tree/master/uncle_regressions/tx_and_bytes_regression.py</a>

The purpose here is to see if, <i>after accounting for the above computed coefficients for gas</i>, there is a correlation with the number of transactions or with the size of a block in bytes left over. Unfortunately, we do not have block size or transaction count figures for uncles, so we have to resort to a more indirect trick that looks at blocks and uncles in groups of 50. The gas coefficients that this analysis finds are higher than the previous analysis: around 0.04 uncle rate per million gas. One possible explanation is that if a single block has a high propagation time, and it leads to an uncle, there is a 50% chance that that uncle is the high-propagation-time block, but there is also a 50% chance that the uncle will be the other block that it competes against. This theory matches well with the 0.04 per million "social uncle rate" and the ~0.02 per million "private uncle rate" finding; hence we will take it as the most likely explanation.

The regression finds that, after accounting for this social uncle rate, one byte accounts for an additional ~0.000002 uncle rate. Bytes in a transaction take up 68 gas, of which <a href="https://docs.google.com/spreadsheets/d/15wghZr-Z6sRSMdmRmhls9dVXTOpxKy8Y64oy9MvDZEQ/edit#gid=0">61 gas accounts for</a> its contribution to bandwidth (the remaining 7 is for bloating the history database). If we want the bandwidth coefficient and the computation coefficient in the gas table to both reflect propagation time, then this implies that if we wanted to <i>really</i> optimize gas costs, we would need to increase the gas cost per byte by 50 (ie. to 138). This would also entail raising the base gas cost of a transaction by 5500 (note: such a rebalance would not mean that everything gets more expensive; the gas limit would be raised by ~10% so that the <i>average-case</i> transaction throughput would remain unchanged). On the other hand, the risk of worst-case denial-of-service attacks is worse for execution than for data, and so execution requires larger safety factors. Hence, there is arguably not sufficiently strong evidence to do any re-pricings here at least for the time being.

One possible long-term protocol change would be to introduce separate gas pricing mechanisms for in-EVM execution and transaction data; the argument here is that the two are much easier to separate as transaction data can be computed separately from everything else, and so the optimal strategy may be to somehow allow the market to balance them; however, precise mechanisms for doing such a thing still need to be developed.

### Gas Limit Policy

For an individual miner determining their gas price, the "private uncle rate" of 0.02 per million gas is the relevant statistic. From the point of view of the whole system, the "social uncle rate" of 0.04 per million gas is what matters. If we did not care about safety factors and were ok with an uncle rate of 0.5 uncles per block (meaning, a "51% attack" would only need 40% hashpower to succeed, actually not as bad as it sounds) then at least this analysis suggests that the gas limit could theoretically be raised to ~11 million (20 tx/sec given an average 39k gas per tx as is the case under current usage, or 37 tx/sec worth of simple sends). With the latest optimizations, this could be pushed even higher. However, since we do care about safety factors and prefer to have a lower uncle rate to alleviate centralization risks, 5.5 million is likely an optimal level for the gas limit, though in the medium term a "dynamic gas limit" formula that targets a particular block processing time would be a better approach, as it would be able to quickly and automatically adjust in response to attacks and risks.

Note that the concern about the centralization risks and the need for safety factors do not stack on top of each other. The reason is that during an active denial-of-service attack, the blockchain needs to survive, not be long-term economically centralization-resistant; the argument is that if the attacker's goal was to economically encourage centralization, then the attacker could just donate money to the biggest pool in order to bribe other miners to join it.

In the future, we can expect virtual machine improvements to decrease uncle rates further, though improvements to networking are eventually going to be required as well. There is a limit to how much scalability is possible on a single chain, with the primary bottleneck being disk reads and writes, so after some point (likely 10-40 million gas) sharding will be the only way to process more transactions. If we just want to decrease equilibrium gas prices, then Casper will help substantially, by making the "slope" of uncle rate to gas consumption near-zero at least up to a certain point.