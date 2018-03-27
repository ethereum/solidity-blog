---
id: 1939
title: Chain Reorganisation Depth Expectations
date: 2015-08-08T11:08:19+00:00
author: Gavin Wood
layout: post
guid: https://blog.ethereum.org/?p=1939
permalink: /2015/08/08/chain-reorganisation-depth-expectations/
dsq_thread_id:
  - "4014005918"
---
This following shows our current and planned expectations concerning maximum likely chain-reorganisation depth. We would not consider transactions within this depth to have an exceptionally high chance of being permanent. These are our own expectations only and <em>do not constitute any sort of guarantee</em>. They are derived from theoretical considerations, ongoing empirical data, human factors in contingency planning and the past experience of our security team. As with all things in the peer-to-peer space the risk is entirely with the individual operator. 

In much the same way as many in the space, we will be monitoring the chain for any signs of protocol-level issues. If we have any reason to suspect that there is a protocol level issue <strong>we will update these expectations</strong> accordingly; the updates will be posted in the forums and on the official blog. All those who are interested in our expectations and recommendations would do well to keep themselves abreast of the blog.

<strong>ROADMAP</strong>

Until 2015/08/08 18:00:00 CEST: <strong>6000</strong>

From 2015/08/08 18:00:00 CEST, <strong>3000</strong> (approx 12 hours)

(1 day)

From 2015/08/09 18:00:00 CEST, <strong>1500</strong> (approx 6 hours)

(3 days)

From 2015/08/12 18:00:00 CEST, <strong>750</strong> (approx 3 hours)

(3 days)

From 2015/08/15 18:00:00 CEST, <strong>375</strong> (approx 90 minutes)

(Rest of Frontier)

---
ADDENDUM 2015/08/08: You may be slightly perplexed as to the meaning of the "chain reorganisation depth". Chain reorganisations happen when a node on the Ethereum network (one which could belong to you, me, an exchange, a miner, whoever) realises that what it *thought* was the canonical chain turned out not to be. When this happens, the transactions in the latter part of its chain (i.e. the most recent transactions) are reverted and rather the transactions in the newer replacement are executed.

With Ethereum having a short target block time of 15s, this actually happens naturally rather often. Because it takes time for the blocks to percolate through the network, it's easy for different parts of the network to have a different final block (or two, or perhaps even three) in normal operation since the miners often come up with them at roughly the same time. This is what we might call <em>ephemeral forking</em>. Indeed, many of the ommers (né uncles) that you see in [stats.ethdev.com](Ethereum's network monitor) were once assumed by some nodes to be the final block in canonical chain.

When a re-organisation happens, or put another way, when the network reaches a more global consensus that it had earlier and a <em>fork</em> is resolved, the nodes that had the now out-dated chain "reorganise" their chain, throwing away the recent and no-longer canonical blocks. Transactions are reverted and others executed to get in line with the other path of the fork.

Transactions can be mutually exclusive, like cheques; if I have $100 in my account and I write two cheques each for $100, the order is very important since they can't both be paid. This means that a reorganisation could result in the reversion of one transaction and the execution of another, mutually exclusive transaction. As such if you're going to do an irreversible action on the back of a transaction being in the chain, it's very important to know the risks regarding reorganisation.

Roughly speaking, the chances of a reorganisation occurring reduce substantially the farther from the end you get. That is, the chance of a reorganisation happening that alters the final three blocks is much less than the chance of one that alters the final block alone. This is because the consensus algorithm is constantly striving to end up at a common agreement over what the chain is. As long as there isn't consensus (and thus potential for a reorganisation), it's not in a stable state and will sooner or later topple into agreement. We call the number of blocks affected by the reorganisation the *depth* of the reorganisation.

In general reorganisations happen automatically and safely, however, anyone making real-world decisions based upon transactions on the chain needs to be aware of reorganisations happening and, most importantly, must make a judgement decision on how deep a transaction must get in the apparent chain before they decide it is the *final* chain and not merely a temporary fork than will eventually be reverted and resolved. The decision of how deep to wait is, in Bitcoin terms, called the <em>number of confirmations</em>.

Our (somewhat large) expectations of possible reorganisation depth (which may very well inform confirmation numbers) come from the fact that the protocol is immature, that human factors are involved in any remedial action and that substantial amounts could be at stake. Basically, it's the Frontier. There are scenarios, especially those involving adversaries ("51%" attackers) that we have devised in which we believe fairly large numbers are indeed warranted at this initial stage.

Ultimately, of course, we can only advise and inform: The risk on how many "confirmations" to wait (or not) as with that of all operational decisions, lies with you. Welcome to freedom :-)

