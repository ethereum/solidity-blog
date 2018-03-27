---
id: 46
title: The Question of Mining
date: 2014-03-20T10:00:56+00:00
author: Vitalik Buterin
layout: post
guid: http://blog.ethereum.org/?p=46
permalink: /2014/03/20/the-question-of-mining/
dsq_thread_id:
  - "2790050061"
---
There are a lot of interesting changes to the Ethereum protocol that are in the works, which will hopefully improve the power of the system, add further features such as light-client friendliness and a higher degree of extensibility, and make Ethereum contracts easier to code. Theoretically, none of these changes are necessary; the Ethereum protocol is fine as it stands today, and can theoretically be released as is once the clients are further built up somewhat; rather, the changes are there to make Ethereum better. However, there is one design objective of Ethereum where the light at the end of the tunnel is a bit further: mining decentralization. Although we always have the backup option of simply sticking with Dagger, <a href="http://blog.ethereum.org/2014/01/15/slasher-a-punitive-proof-of-stake-algorithm/">Slasher</a> or SHA3, it is entirely unclear that any of those algorithms can truly remain decentralized and mining pool and ASIC-resistant in the long term (Slasher is guaranteed to be decentralized because it’s proof of stake, but has its own moderately problematic flaws).

The basic idea behind the mining algorithm that we want to use is essentially in place; however, as in many cases, the devil is in the details.

This version of the Ethereum mining algorithm is a Hashcash-based implementation, similar to Bitcoin’s SHA256 and Litecoin’s scrypt; the idea is for the miner to repeatedly compute a pseudorandom function on a block and a nonce, trying a different nonce each time, until eventually some nonce produces a result which starts with a large number of zeroes. The only room to innovate in this kind of implementation is changing the function; in Ethereum’s case, the rough outline of the function, taking the blockchain state (defined as the header, the current state tree, and all the data of the last 16 blocks), is as follows:

1. Let `h[i] = sha3(sha3(block_header) ++ nonce ++ i)` for `0 &lt;= i &lt;= 15`

2. Let `S` be the blockchain state 16 blocks ago.

3. Let `C[i]` be the transaction count of the block `i` blocks ago. Let `T[i]` be the `(h[i] mod C[i])`th transaction from the block `i` blocks ago.

4. Apply `T[0]`, `T[1]` … `T[15]` sequentially to `S`. However, every time the transaction leads to processing a contract, (pseudo-)randomly make minor modifications to the code of all contracts affected.

5. Let `S'` be the resulting state. Let `r` be the sha3 of the root of `S'`.

If `r &lt;= 2^256 / diff`, then `nonce` is a valid nonce.

To summarize in non-programmatic language, the mining algorithm requires the miner to grab a few random transactions from the last 16 blocks, run the computation of applying them to the state 16 blocks ago with a few random modifications, and then take the hash of the result. Every new nonce that the miner tries, the miner would have to repeat this process over again, with a new set of random transactions and modifications each time.

The benefits of this are:

1. It requires the entire blockchain state to mine, essentially requiring every miner to be a full node. This helps with network decentralization, because a larger number of full nodes exist.

2. Because every miner is now required to be a full node, mining pools become much less useful. In the Bitcoin world, mining pools serve two key purposes. First, pools even out the mining reward; instead of every block providing a miner with a 0.0001% chance of mining a $16,000 block, a miner can mine into the pool and the pool gives the miner a 1% chance of receiving a payout of $1.60. Second, however, pools also provide centralized block validation. Instead of having to run a full Bitcoin client themselves, a miner can simply grab block header data from the pool and mine using that data without actually verifying the block for themselves. With this algorithm, the second argument is moot, and the first concern can be adequately met by peer-to-peer pools that do not give control of a significant portion of network hashpower to a centralized service.

3. It's ASIC-resistant almost by definition. Because the EVM language is Turing-complete, any kind of computation that can be done in a normal programming language can be encoded into EVM code. Therefore, an ASIC that can run all of EVM is by necessity an ASIC for generalized computation – in other words, a CPU. This also has a Primecoin-like social benefit: effort spent toward building EVM ASICs also havs the side benefit of building hardware to make the network faster.

4. The algorithm is relatively computationally quick to verify, although there is no “nice” verification formula that can be run inside EVM code.

However, there are still several major challenges that remain. First, it is not entirely clear that the system of picking random transactions actually ends up requiring the miner to use the entire blockchain. Ideally, the blockchain accesses would be random; in such a setup, a miner with half the blockchain would succeed only on about 1 in 216 nonces. In reality, however, 95% of all transactions will likely use 5% of the blockchain; in such a system, a node with 5% of the memory will only take a slowdown penalty of about 2x.

Second, and more importantly, however, it is difficult to say how much an EVM miner could be optimized. The algorithm definition above asks the miner to “randomly make minor modifications” to the contract. This part is crucial. The reason is this: most transactions have results that are independent of each other; the transactions might be of the form “A sends to B”, “C sends to D”, “E sends to contract F that affects G and H”, etc, with no overlap. Hence, without random modification there would be little need for an EVM miner to actually do much computation; the computation would happen once, and then the miner would just precompute and store the deltas and apply them immediately. The random modifications mean that the miner has to actually make new EVM computations each time the algorithm is run. However, this solution is itself imperfect in two ways. First of all, random modifications can potentially easily result in what would otherwise be very complex and intricate calculations simply ending early, or at least calulations for which the optimizations are very different from the optimizations applied to standard transactions. Second, mining algorithms may deliberately skip complex contracts in favor of simple or easily optimizable ones. There are heuristic tricks for battling both problems, but it is entirely unclear exactly what those heuristics would be.

Another interesting point in favor of this kind of mining is that even if optimized hardware miners emerge, the community has the ability to work together to essentially change the mining algorithm by “poisoning” the transaction pool. Engineers can analyze existing ASICs, determine what their optimizations are, and dump transactions into the blockchain that such optimizations simply do not work with. If 5% of all transactions are effectively poisoned, then ASICs cannot possibly have a speedup of more than 20x. The nice thing is that there is a reason why people would pay the transaction fees to do this: each individual ASIC company has the incentive to poison the well for its competitors.

These are all challenges that we will be working on heavily in the next few months.