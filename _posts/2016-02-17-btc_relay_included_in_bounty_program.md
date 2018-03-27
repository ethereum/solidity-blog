---
id: 2579
title: BTC Relay included in Ethereum Bounty Program
date: 2016-02-17T06:09:04+00:00
author: Gustav Simonsson
layout: post
guid: https://blog.ethereum.org/?p=2579
permalink: /2016/02/17/btc_relay_included_in_bounty_program/
dsq_thread_id:
  - "4586010794"
---
Following hacking @ <a href="http://devcon.ethereum.org/">DEVCON1</a>,  <a href="https://twitter.com/mhswende">Martin Swende</a> is Nr. 1 on the leaderboard of <a href="http://bounty.ethdev.com/">the Ethereum Bounty Program</a>. The bounty program is ongoing and the <a href="http://bounty.ethdev.com/users/jNsSFyEPTSH4ZhraM">last bounty awarded</a> amounted to 5 BTC. The program is open to anyone. With <a href="https://github.com/ethereum/btcrelay">BTC Relay</a> getting ready for launch on Ethereum and its importance for many DApps, we want to highlight its ongoing security audit by including it in the Ethereum Bounty Program.

BTC Relay is an Ethereum contract that implements Bitcoin SPV: <a href="https://en.bitcoin.it/wiki/Thin_Client_Security">https://en.bitcoin.it/wiki/Thin_Client_Security</a>

The chief purpose of BTC Relay is to pass along any sufficiently confirmed Bitcoin transaction, to a specified Ethereum contract. If someone makes a Bitcoin payment, or any arbitrary transaction on the canonical Bitcoin blockchain, the relay should be able to send it to any specified Ethereum contract. <a href="https://docs.google.com/document/d/17e5wVi1r5iBZc-kkv8mBFQLATF3q5hPoTI9mdAd3qKg/edit?usp=sharing">More details in the spec</a>.

The goal is to identify security issues such as accepting invalid blockheaders, false proofs, or invalid Bitcoin transactions. Similarly, if there is a valid Bitcoin transaction which BTC Relay does not fully relay, that would also be eligible for bounties.

Please note that since BTC Relay has a separate open-source grant for bounties, major bugs will be rewarded up to <b>1 BTC</b>.  Much higher rewards are possible (up to <b>5 BTC</b>) in case of very severe vulnerabilities. Rewards are eligible for everyone except bounty program judges and developers of BTC Relay.

The scope is on the contract, the 5 “.se” files in the root directory of:

<a href="https://github.com/ethereum/btcrelay/tree/1466934855225b1e4a87031d299c1209ba12d503">https://github.com/ethereum/btcrelay/tree/1466934855225b1e4a87031d299c1209ba12d503</a>

(This is a commit on<a href="https://github.com/ethereum/btcrelay"> https://github.com/ethereum/btcrelay</a> develop branch).

Not in scope is complete SPV client functionality (for example Bitcoin block timestamps are not checked to save gas costs). Better mechanisms for incentivization, gas cost and other algorithm optimization are not in scope. That said, any such feedback will still be gladly considered.

With BTC Relay now included in the Ethereum bounty program, most of the rules on<a href="http://bounty.ethdev.com"> http://bounty.ethdev.com</a> apply. For examples, websites are not part of the bounty program and first come, first serve -- issues that have already been submitted by another user or are<a href="https://github.com/ethereum/btcrelay/issues"> already known</a> to the team are not eligible for bounty rewards. But, this also means that beyond monetary rewards, every bounty is also eligible for:
<ul>
	<li>Listing on the the Ethereum bounty leaderboard with points accumulating over the course of the program.</li>
	<li>Personal inscription in the Ethereum namereg once it's live.</li>
	<li>An exclusive, limited edition Ethereum Bountyhunter t-shirt</li>
</ul>
If you’d like to join the channel for BTC Relay, it is open to all at <a href="https://gitter.im/ethereum/btcrelay">https://gitter.im/ethereum/btcrelay</a>.  The bounty program will run for a few weeks before launching BTC Relay to Frontier.  Here are some items to discuss with the community and open questions for the Frontier launch:
<ul>
	<li>what should be the first block in BTC Relay?
<ul>
	<li>for technical and practical reasons, the earliest block that can be stored in BTC Relay is block 2016 (first difficulty retarget).  BTC Relay’s first block must be on a difficulty retarget, ie a block divisible by 2016.</li>
</ul>
</li>
	<li>how likely are you to verify Bitcoin transactions from a while ago?</li>
	<li>how useful would it be if BTC Relay started with the block two difficulty retargets ago?
<ul>
	<li>currently, that would be block 389088</li>
</ul>
</li>
	<li>there is a script that anyone can run to submit block headers to BTC Relay and what do you think its default fee, which verifiers of a Bitcoin transaction pay in ETH, should be?
<ul>
	<li><a href="https://github.com/ConsenSys/btcrelay-fetchd/blob/master/fetchd.py#L48">script’s current fee is 0</a></li>
	<li>it usually costs less than 0.01 ETH to submit a block header.  should the default fee be 0.01 ETH?</li>
	<li>this default fee can be overridden to whatever submitter desires, although the incentive mechanism makes it so that setting the fee excessively is unlikely to be rewarding</li>
</ul>
</li>
</ul>
Finally, the BTC Relay Bounty Program was added in “news &amp; updates” to bounty.ethdev.com a couple of weeks ago, and has already attracted 1 bounty submission!