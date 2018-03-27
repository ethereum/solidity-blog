---
id: 1053
title: The Search for a Stable Cryptocurrency
date: 2014-11-11T21:24:21+00:00
author: Vitalik Buterin
layout: post
guid: https://blog.ethereum.org/?p=1053
permalink: /2014/11/11/search-stable-cryptocurrency/
dsq_thread_id:
  - "3214937748"
---
<i><small>Special thanks to Robert Sams for the development of Seignorage Shares and insights regarding how to correctly value volatile coins in multi-currency systems</small></i>

<i><small>Note: we are not planning on adding price stabilization to ether; our philosophy has always been to keep ether simple to minimize black-swan risks. Results of this research will likely go into either subcurrencies or independent blockchains</small></i>

<p>One of the main problems with Bitcoin for ordinary users is that, while the network may be a great way of sending payments, with lower transaction costs, much more expansive global reach, and a very high level of censorship resistance, Bitcoin the currency is a very volatile means of storing value. Although the currency had by and large grown by leaps and bounds over the past six years, especially in financial markets past performance is no guarantee (and by efficient market hypothesis not even an indicator) of future results of expected value, and the currency also has an established reputation for extreme volatility; over the past eleven months, Bitcoin holders have lost about 67% of their wealth and quite often the price moves up or down by as much as 25% in a single week. Seeing this concern, there is a growing interest in a simple question: can we get the best of both worlds? Can we have the full decentralization that a cryptographic payment network offers, but at the same time have a higher level of price stability, without such extreme upward and downward swings?</p>

<img src="https://blog.ethereum.org/wp-content/uploads/2014/11/chart.png"></img>

<p>Last week, a team of Japanese researchers made a <a href="www.coindesk.com/japanese-scholars-draft-proposal-better-bitcoin/">proposal</a> for an "<a href="http://www.scribd.com/doc/245827939/SSRN-id2519367-Japan-Improved-Bitcoin-IBC">improved Bitcoin</a>", which was an attempt to do just that: whereas Bitcoin has a fixed supply, and a volatile price, the researchers' Improved Bitcoin would vary its supply in an attempt to mitigate the shocks in price. However, the problem of making a price-stable cryptocurrency, as the researchers realized, is much different from that of simply setting up an inflation target for a central bank. The underlying question is more difficult: how do we target a fixed price in a way that is both decentralized and robust against attack?</p>

<p>To resolve the issue properly, it is best to break it down into two mostly separate sub-problems:</p>

<ol>
<li>How do we measure a currency's price in a decentralized way?</li>
<li>Given a desired supply adjustment to target the price, to whom do we issue and how do we absorb currency units?</li>
</ol>

<h3>Decentralized Measurement</h3>

<p>For the decentralized measurement problem, there are two known major classes of solutions: <strong>exogenous solutions</strong>, mechanisms which try to measure the price with respect to some precise index from the outside, and <strong>endogenous solutions</strong>, mechanisms which try to use internal variables of the network to measure price. As far as exogenous solutions go, so far the only reliable known class of mechanisms for (possibly) cryptoeconomically securely determining the value of an exogenous variable are the different variants of <a href="https://blog.ethereum.org/2014/03/28/schellingcoin-a-minimal-trust-universal-data-feed/">Schellingcoin</a> - essentially, have everyone vote on what the result is (using some set chosen randomly based on mining power or stake in some currency to prevent sybil attacks), and reward everyone that provides a result that is close to the majority consensus. If you assume that everyone else will provide accurate information, then it is in your interest to provide accurate information in order to be closer to the consensus - a self-reinforcing mechanism much like cryptocurrency consensus itself.</p>

<center>
<img src="https://blog.ethereum.org/wp-content/uploads/2014/11/schellingcoin.png" width="450px"></img>
</center>

<p>The main problem with Schellingcoin is that it's not clear exactly how stable the consensus is. Particularly, what if some medium-sized actor pre-announces some alternative value to the truth that would be beneficial for most actors to adopt, and the actors manage to coordinate on switching over? If there was a large incentive, and if the pool of users was relatively centralized, it might not be too difficult to coordinate on switching over.</p>

<p>There are three major factors that can influence the extent of this vulnerability:</p>

<ol>
<li>Is it likely that the participants in a schellingcoin actually have a common incentive to bias the result in some direction?</li>
<li>Do the participants have some common stake in the system that would be devalued if the system were to be dishonest?</li>
<li>Is it possible to "credibly commit" to a particular answer (ie. commit to providing the answer in a way that obviously can't be changed)?</li>
</ol>

<p>(1) is rather problematic for single-currency systems, as if the set of participants is chosen by their stake in the currency then they have a strong incentive to pretend the currency price is lower so that the compensation mechanism will push it up, and if the set of participants is chosen by mining power then they have a strong incentive to pretend the currency's price is too high so as to increase the issuance. Now, if there are two kinds of mining, one of which is used to select Schellingcoin participants and the other to receive a variable reward, then this objection no longer applies, and multi-currency systems can also get around the problem. (2) is true if the participant selection is based on either stake (ideally, long-term bonded stake) or ASIC mining, but false for CPU mining. However, we should not simply count on this incentive to outweigh (1).</p>

<p>(3) is perhaps the hardest; it depends on the precise technical implementation of the Schellingcoin. A simple implementation involving simply submitting the values to the blockchain is problematic because simply submitting one's value early is a credible commitment. The original SchellingCoin used a mechanism of having everyone submit a hash of the value in the first round, and the actual value in the second round, sort of a cryptographic equivalent to requiring everyone to put down a card face down first, and then flip it at the same time; however, this too allows credible commitment by revealing (even if not submitting) one's value early, as the value can be checked against the hash.</p>

<p>A third option is requiring all of the participants to submit their values directly, but only during a specific block; if a participant does release a submission early they can always "double-spend" it. The 12-second block time would mean that there is almost no time for coordination. The creator of the block can be strongly incentivized (or even, if the Schellingcoin is an independent blockchain, required) to include all participations, to discourage or prevent the block maker from picking and choosing answers. A fourth class of options involves some <a href="https://blog.ethereum.org/2014/08/16/secret-sharing-erasure-coding-guide-aspiring-dropbox-decentralizer/">secret sharing</a> or <a href="https://en.wikipedia.org/wiki/Secure_multi-party_computation">secure multiparty computation</a> mechanism, using a collection of nodes, themselves selected by stake (perhaps even the participants themselves), as a sort of decentralized substitute for a centralized server solution, with all the privacy that such an approach entails.</p>

<p>Finally, a fifth strategy is to do the schellingcoin "blockchain-style": every period, some random stakeholder is selected, and told to provide their vote as a <code>[id, value]</code> pair, where <code>value</code> is the actual valid and <code>id</code> is an identifier of the previous vote that looks correct. The incentive to vote correctly is that only tests that remain in the main chain after some number of blocks are rewarded, and future voters will note attach their vote to a vote that is incorrect fearing that if they do voters after them will reject their vote.</p>

<p>Schellingcoin is an untested experiment, and so there is legitimate reason to be skeptical that it will work; however, if we want anything close to a perfect price measurement scheme it's currently the only mechanism that we have. If Schellingcoin proves unworkable, then we will have to make do with the other kinds of strategies: the endogenous ones.</p>

<h3>Endogenous Solutions</h3>

<p>To measure the price of a currency endogenously, what we essentially need is to find some service inside the network that is known to have a roughly stable real-value price, and measure the price of that service inside the network as measured in the network's own token. Examples of such services include:</p>

<ul>
<li>Computation (measured via mining difficulty)</li>
<li>Transaction fees</li>
<li>Data storage</li>
<li>Bandwidth provision</li>
</ul>

<p>A slightly different, but related, strategy, is to measure some statistic that correllates indirectly with price, usually a metric of the level of usage; one example of this is transaction volume.</p>

<p>The problem with all of these services is, however, that none of them are very robust against rapid changes due to technological innovation. Moore's Law has so far guaranteed that most forms of computational services become cheaper at a rate of 2x every two years, and it could easily speed up to 2x every 18 months or 2x every five years. Hence, trying to peg a currency to any of those variables will likely lead to a system which is hyperinflationary, and so we need some more advanced strategies for using these variables to determine a more stable metric of the price.</p>

<p>First, let us set up the problem. Formally, we define an <strong>estimator</strong> to be a function which receives a data feed of some input variable (eg. mining difficulty, transaction cost in currency units, etc) <code>D[1]</code>, <code>D[2]</code>, <code>D[3]</code>..., and needs to output a stream of estimates of the currency's price, <code>P[1]</code>, <code>P[2]</code>, <code>P[3]</code>... The estimator obviously cannot look into the future; <code>P[i]</code> can be dependent on <code>D[1]</code>, <code>D[2]</code> ... <code>D[i]</code>, but not <code>D[i+1]</code>. Now, to start off, let us graph the simplest possible estimator on Bitcoin, which we'll call the <strong>naive estimator</strong>: difficulty equals price.</p>

<center>
<img src="https://blog.ethereum.org/wp-content/uploads/2014/11/endo3.svg_.png" width="450px"></img>
</center>

<p>Unfortunately, the problem with this approach is obvious from the graph and was already mentioned above: difficulty is a function of both price and Moore's law, and so it gives results that depart from any accurate measure of the price exponentially over time. The first immediate strategy to fix this problem is to try to compensate for Moore's law, using the difficulty but artificially reducing the price by some constant per day to counteract the expected speed of technological progress; we'll call this the <strong>compensated naive estimator</strong>. Note that there are an infinite number of versions of this estimator, one for each depreciation rate, and all of the other estimators that we show here will also have parameters.</p>

<p>The way that we will select the parameter for our version is by using a variant of <a href="http://en.wikipedia.org/wiki/Simulated_annealing">simulated annealing</a> to find the optimal values, using the first 780 days of the Bitcoin price as "training data". The estimators are then left to perform as they would for the remaining 780 days, to see how they would react to conditions that were unknown when the parameters were optimized (this technique, knows as "cross-validation", is standard in machine learning and optimization theory). The optimal value for the compensated estimator is a drop of 0.48% per day, leading to this chart:</p>

<center>
<img src="https://blog.ethereum.org/wp-content/uploads/2014/11/endo4.svg_.png" width="450px"></img>
</center>

<p>The next estimator that we will explore is the <strong>bounded estimator</strong>. The way the bounded estimator works is somewhat more complicated. By default, it assumes that all growth in difficulty is due to Moore's law. However, it assumes that Moore's law cannot go backwards (ie. technology getting worse), and that Moore's law cannot go faster than some rate - in the case of our version, 5.88% per two weeks, or roughly quadrupling every year. Any growth outside these bounds it assumes is coming from price rises or drops. Thus, for example, if the difficulty rises by 20% in one period, it assumes that 5.88% of it is due to technological advancements, and the remaining 14.12% is due to a price increase, and thus a stabilizing currency based on this estimator might increase supply by 14.12% to compensate. The theory is that cryptocurrency price growth to a large extent happens in rapid bubbles, and thus the bounded estimator should be able to capture the bulk of the price growth during such events.</p>

<center>
<img src="https://blog.ethereum.org/wp-content/uploads/2014/11/endo5.svg_.png" width="450px"></img>
</center>

<p>There are more advanced strategies as well; the best strategies should take into account the fact that ASIC farms take time to set up, and also follow a <a href="https://en.wikipedia.org/wiki/Hysteresis">hysteresis</a> effect: it's often viable to keep an ASIC farm online if you already have it even when under the same conditions it would not be viable to start up a new one. A simple approach is looking at the rate of increase of the difficulty, and not just the difficulty itself, or even using a linear regression analysis to project difficulty 90 days into the future. Here is a chart containing the above estimators, plus a few others, compared to the actual price:</p>

<img src="https://blog.ethereum.org/wp-content/uploads/2014/11/endo30.svg_.png"></img>

<p>Note that the chart also includes three estimators that use statistics other than Bitcoin mining: a simple and an advanced estimator using transaction volume, and an estimator using the average transaction fee. We can also split up the mining-based estimators from the other estimators:</p>

<center>
<table>
<tr>
<td><img src="https://blog.ethereum.org/wp-content/uploads/2014/11/endo10.svg_.png" width="330px"></img></td>
<td><img src="https://blog.ethereum.org/wp-content/uploads/2014/11/endo20.svg_.png" width="330px"></img></td>
</tr>
</table>
</center>

See <a href="https://github.com/ethereum/economic-modeling/tree/master/stability">https://github.com/ethereum/economic-modeling/tree/master/stability</a> for the source code that produced these results.

<p>Of course, this is only the beginning of endogenous price estimator theory; a more thorough analysis involving dozens of cryptocurrencies will likely go much further. The best estimators may well end up using a combination of different measures; seeing how the difficulty-based estimators overshot the price in 2014 and the transaction-based estimators undershot the price, the two combined could end up being substantially more accurate. The problem is also going to get easier over time as we see the Bitcoin mining economy stabilize toward something closer to an equilibrium where technology improves only as fast as the general Moore's law rule of 2x every 2 years.</p>

<p>To see just how good these estimators can get, we can note from the charts that they can cancel out at least 50% of cryptocurrency price volatility, and may increase to ~67% once the mining industry stabilizes. Something like Bitcoin, if it becomes mainstream, will likely be somewhat more unstable than <a href="http://www.xe.com/currencycharts/?from=XAU&to=USD&view=10Y">gold</a>, but not that much more unstable - the only difference between BTC and gold is that the supply of gold can actually increase as the price goes higher since more can be mined if miners are willing to pay higher costs, so there is an implicit dampening effect, but the supply elasticity of gold is surprisingly <a href="http://goldratefortoday.org/wp-content/uploads/2011/03/world-gold-production-1900-2010.png">not</a> that <a href="http://www.goldsheetlinks.com/goldhist.htm">high</a>; production barely increased at all during the run-ups in price during the 1970s and 2000s. The price of gold stayed within a range of 4.63x ($412 to $1980) in the last decade; logarithmically reducing that by two thirds gives a range of 1.54x, not much higher than <a href="http://www.xe.com/currencycharts/?from=EUR&to=USD&view=10Y#">EUR/USD</a> (1.37x), <a href="http://www.xe.com/currencycharts/?from=JPY&to=USD&view=10Y#">JPY/USD</a> (1.64x) or <a href="http://www.xe.com/currencycharts/?from=CAD&to=USD&view=10Y#">CAD/USD</a> (1.41x); thus, endogenous stabilization may well prove quite viable, and may be preferred due to its lack of tie to any specific centralized currency or authority.</p>

<p>The other issue that all of these estimators have to contend with is exploitability: if transaction volume is used to determine the currency's price, then an attacker can manipulate the price very easily by simply sending very many transactions. The average transaction fees paid in Bitcoin are about $5000 per day; at that price in a stabilized currency the attacker would be able to halve the price. Mining difficulty, however, is much more difficult to exploit simply because the market is so large. If a platform does not want to accept the inefficiencies of wasteful proof of work, an alternative is to build in a market for other resources, such as storage, instead; <a href="http://filecoin.io">Filecoin</a> and <a href="http://research.microsoft.com/apps/pubs/default.aspx?id=217984">Permacoin</a> are two efforts that attempt to use a decentralized file storage market as a consensus mechanism, and the same market could easily be dual-purposed to serve as an estimator.</p>

<h3>The Issuance Problem</h3>

<p>Now, even if we have a reasonably good, or even perfect, estimator for the currency's price, we still have the second problem: how do we issue or absorb currency units? The simplest approach is to simply issue them as a mining reward, as proposed by the Japanese researchers. However, this has two problems:</p>

<ol>
<li>Such a mechanism can only issue new currency units when the price is too high; it cannot absorb currency units when the price is too low.</li>
<li>If we are using mining difficulty in an endogenous estimator, then the estimator needs to take into account the fact that some of the increases in mining difficulty will be a result of an increased issuance rate triggered by the estimator itself.</li>
</ol>

<p>If not handled very carefully, the second problem has the potential to create some rather dangerous feedback loops in either direction; however, if we use a different market as an estimator and as an issuance model then this will not be a problem. The first problem seems serious; in fact, one can interpret it as saying that any currency using this model will always be strictly worse than Bitcoin, because Bitcoin will eventually have an issuance rate of zero and a currency using this mechanism will have an issuance rate always above zero. Hence, the currency will always be more inflationary, and thus less attractive to hold. However, this argument is not quite true; the reason is that when a user purchases units of the stabilized currency then they have more confidence that at the time of purchase <em>the units are not already overvalued</em> and therefore will soon decline. Alternatively, one can note that extremely large swings in price are justified by changing estimations of the probability the currency will become thousands of times more expensive; clipping off this possibility will reduce the upward and downward extent of these swings. For users who care about stability, this risk reduction may well outweigh the increased general long-term supply inflation.</p>

<h3>BitAssets</h3>

<p>A second approach is the (original implementation of the) "bitassets" strategy used by Bitshares. This approach can be described as follows:</p>

<ol>
<li>There exist two currencies, "vol-coins" and "stable-coins".</li>
<li>Stable-coins are understood to have a value of $1.</li>
<li>Vol-coins are an actual currency; users can have a zero or positive balance of them. Stable-coins exist only in the form of <a href="https://en.wikipedia.org/wiki/Contract_for_difference">contracts-for-difference</a> (ie. every negative stable-coin is really a debt to someone else, collateralized by at least 2x the value in vol-coins, and every positive stable-coin is the ownership of that debt).</li>
<li>If the value of someone's stable-coin debt exceeds 90% of the value of their vol-coin collateral, the debt is cancelled and the entire vol-coin collateral is transferred to the counterparty ("margin call")</li>
<li>Users are free to trade vol-coins and stable-coins with each other.</li>
</ol>

<p>And that's it. The key piece that makes the mechanism (supposedly) work is the concept of a "market peg": because everyone understands that stable-coins are supposed to be worth $1, if the value of a stable-coin drops below $1, then everyone will realize that it will eventually go back to $1, and so people will buy it, so it actually will go back to $1 - a self-fulfilling prophecy argument. And for a similar reason, if the price goes above $1, it will go back down. Because stable-coins are a zero-total-supply currency (ie. each positive unit is matched by a corresponding negative unit), the mechanism is not intrinsically unworkable; a price of $1 could be stable with ten users or ten billion users (remember, <a href="http://thoughtinfection.com/2014/02/09/on-the-blockchain-nobody-knows-youre-a-fridge-and-9-other-amazing-things-about-bitcoin/">fridges</a> are users too!).</p>

<p>However, the mechanism has some rather serious fragility properties. Sure, if the price of a stable-coin goes to $0.95, and it's a small drop that can easily be corrected, then the mechanism will come into play, and the price will quickly go back to $1. However, if the price suddenly drops to $0.90, or lower, then users may interpret the drop as a sign that the peg is actually breaking, and will start scrambling to get out while they can - thus making the price fall even further. At the end, the stable-coin could easily end up being worth nothing at all. In the real world, markets do often show positive feedback loops, and it is quite likely that the only reason the system has not fallen apart already is because everyone knows that there exists a large centralized organization (BitShares Inc) which is willing to act as a buyer of last resort to maintain the "market" peg if necessary.</p>

<p>Note that BitShares has now moved to a somewhat different model involving price feeds provided by the delegates (participants in the consensus algorithm) of the system; hence the fragility risks are likely substantially lower now.</p>

<h3>SchellingDollar</h3>

<p>An approach vaguely similar to BitAssets that arguably works much better is the <a href="https://github.com/ethereum/serpent/blob/master/examples/schellingdollar.se">SchellingDollar</a> (called that way because it was originally intended to work with the SchellingCoin price detection mechanism, but it can also be used with endogenous estimators), defined as follows:</p>

<ol>
<li>There exist two currencies, "vol-coins" and "stable-coins". Vol-coins are initially distributed somehow (eg. pre-sale), but initially no stable-coins exist.</li>
<li>Users may have only a zero or positive balance of vol-coins. Users may have a negative balance of stable-coins, but can only acquire or increase their negative balance of stable-coins if they have a quantity of vol-coins equal in value to twice their new stable-coin balance (eg. if a stable-coin is $1 and a vol-coin is $5, then if a user has 10 vol-coins ($50) they can at most reduce their stable-coin balance to -25)</li>
<li>If the value of a user's negative stable-coins exceeds 90% of the value of the user's vol-coins, then the user's stable-coin and vol-coin balances are both reduced to zero ("margin call"). This prevents situations where accounts exist with negative-valued balances and the system goes bankrupt as users run away from their debt.</li>
<li>Users can convert their stable-coins into vol-coins or their vol-coins into stable-coins at a rate of $1 worth of vol-coin per stable-coin, perhaps with a 0.1% exchange fee. This mechanism is of course subject to the limits described in (2).</li>
<li>The system keeps track of the total quantity of stable-coins in circulation. If the quantity exceeds zero, the system imposes a negative interest rate to make positive stable-coin holdings less attractive and negative holdings more attractive. If the quantity is less than zero, the system similarly imposes a positive interest rate. Interest rates can be adjusted via something like a <a href="https://en.wikipedia.org/wiki/PID_controller">PID controller</a>, or even a simple "increase or decrease by 0.2% every day based on whether the quantity is positive or negative" rule.</li>
</ol>

<p>Here, we do not simply assume that the market will keep the price at $1; instead, we use a central-bank-style interest rate targeting mechanism to artificially discourage holding stable-coin units if the supply is too high (ie. greater than zero), and encourage holding stable-coin units if the supply is too low (ie. less than zero). Note that there are still fragility risks here. First, if the vol-coin price falls by more than 50% very quickly, then many margin call conditions will be triggered, drastically shifting the stable-coin supply to the positive side, and thus forcing a high negative interest rate on stable-coins. Second, if the vol-coin market is too thin, then it will be easily manipulable, allowing attackers to trigger margin call cascades.</p>

<p>Another concern is, why would vol-coins be valuable? Scarcity alone will not provide much value, since vol-coins are inferior to stable-coins for transactional purposes. We can see the answer by modeling the system as a sort of decentralized corporation, where "making profits" is equivalent to absorbing vol-coins and "taking losses" is equivalent to issuing vol-coins. The system's profit and loss scenarios are as follows:</p>

<ul>
<li><strong>Profit</strong>: transaction fees from exchanging stable-coins for vol-coins</li>
<li><strong>Profit</strong>: the extra 10% in margin call situations</li>
<li><strong>Loss</strong>: situations where the vol-coin price falls while the total stable-coin supply is positive, or rises while the total stable-coin supply is negative (the first case is more likely to happen, due to margin-call situations)</li>
<li><strong>Profit</strong>: situations where the vol-coin price rises while the total stable-coin supply is positive, or falls while it's negative</li>
</ul>

<p>Note that the second profit is in some ways a phantom profit; when users hold vol-coins, they will need to take into account the risk that they will be on the receiving end of this extra 10% seizure, which cancels out the benefit to the system from the profit existing. However, one might argue that because of the <a href="https://en.wikipedia.org/wiki/Dunning%E2%80%93Kruger_effect">Dunning-Kruger effect</a> users might underestimate their susceptibility to eating the loss, and thus the compensation will be less than 100%.</p>

<p>Now, consider a strategy where a user tries to hold on to a constant percentage of all vol-coins. When x% of vol-coins are absorbed, the user sells off x% of their vol-coins and takes a profit, and when new vol-coins equal to x% of the existing supply are released, the user increases their holdings by the same portion, taking a loss. Thus, the user's net profit is proportional to the total profit of the system.</p>

<h3>Seignorage Shares</h3>

<p>A fourth model is "seignorage shares", <a href="https://github.com/rmsams/stablecoins">courtesy</a> of <a href="http://cryptonomics.org/">Robert Sams</a>. Seignorage shares is a rather elegant scheme that, in my own simplified take on the scheme, works as follows:</p>

<ol>
<li>There exist two currencies, "vol-coins" and "stable-coins" (Sams uses "shares" and "coins", respectively)</li>
<li>Anyone can purchase vol-coins for stable-coins or vol-coins for stable-coins from the system at a rate of $1 worth of vol-coin per stable-coin, perhaps with a 0.1% exchange fee</li>
</ol>

<p>Note that in Sams' version, an auction was used to sell off newly-created stable-coins if the price goes too high, and buy if it goes too low; this mechanism basically has the same effect, except using an always-available fixed price in place of an auction. However, the simplicity comes at the cost of some degree of fragility. To see why, let us make a similar valuation analysis for vol-coins. The profit and loss scenarios are simple:</p>

<ul>
<li><strong>Profit</strong>: absorbing vol-coins to issue new stable-coins</li>
<li><strong>Loss</strong>: issuing vol-coins to absorb stable-coins</li>
</ul>

<p>The same valuation strategy applies as in the other case, so we can see that the value of the vol-coins is proportional to the expected total future increase in the supply of stable-coins, adjusted by some discounting factor. Thus, here lies the problem: if the system is understood by all parties to be "winding down" (eg. users are abandoning it for a superior competitor), and thus the total stable-coin supply is expected to go down and never come back up, then the value of the vol-coins drops below zero, so vol-coins hyperinflate, and then stable-coins hyperinflate. In exchange for this fragility risk, however, vol-coins can achieve a much higher valuation, so the scheme is much more attractive to cryptoplatform developers looking to earn revenue via a token sale.</p>

<p>Note that both the SchellingDollar and seignorage shares, if they are on an independent network, also need to take into account transaction fees and consensus costs. Fortunately, with proof of stake, it should be possible to make consensus cheaper than transaction fees, in which case the difference can be added to profits. This potentially allows for a larger market cap for the SchellingDollar's vol-coin, and allows the market cap of seignorage shares' vol-coins to remain above zero even in the event of a substantial, albeit not total, permanent decrease in stable-coin volume. Ultimately, however, some degree of fragility is inevitable: at the very least, if interest in a system drops to near-zero, then the system can be double-spent and estimators and Schellingcoins exploited to death. Even sidechains, as a scheme for preserving one currency across multiple networks, are susceptible to this problem. The question is simply (1) how do we minimize the risks, and (2) given that risks exist, how do we present the system to users so that they do not become overly dependent on something that could break?</p>

<h3>Conclusions</h3>

<p>Are stable-value assets necessary? Given the high level of interest in "blockchain technology" coupled with disinterest in "Bitcoin the currency" that we see among so many in the mainstream world, perhaps the time is ripe for stable-currency or multi-currency systems to take over. There would then be multiple separate classes of cryptoassets: stable assets for trading, speculative assets for investment, and Bitcoin itself may well serve as a unique Schelling point for a universal fallback asset, similar to the current and historical functioning of gold.</p>

<p>If that were to happen, and particularly if the stronger version of price stability based on Schellingcoin strategies could take off, the cryptocurrency landscape may end up in an interesting situation: there may be thousands of cryptocurrencies, of which many would be volatile, but many others would be stable-coins, all adjusting prices nearly in lockstep with each other; hence, the situation could even end up being expressed in interfaces as a single super-currency, but where different blockchains randomly give positive or negative interest rates, much like <a href="http://papers.ssrn.com/sol3/papers.cfm?abstract_id=2425270">Ferdinando Ametrano's "Hayek Money"</a>. The true cryptoeconomy of the future may have not even begun to take shape.</p>