---
id: 2364
title: 'Understanding Serenity, Part 2: Casper'
date: 2015-12-28T01:54:30+00:00
author: Vitalik Buterin
layout: post
guid: https://blog.ethereum.org/?p=2364
permalink: /2015/12/28/understanding-serenity-part-2-casper/
dsq_thread_id:
  - "4439097518"
---
<em>Special thanks to Vlad Zamfir for introducing the idea of by-block consensus and convincing me of its merits, alongside many of the other core ideas of Casper, and to Vlad Zamfir and Greg Meredith for their continued work on the protocol</em>

<p>In the last post in this series, we discussed one of the two flagship feature sets of Serenity: a heightened degree of abstraction that greatly increases the flexibility of the platform and takes a large step in moving Ethereum from "Bitcoin plus Turing-complete" to "general-purpose decentralized computation". Now, let us turn our attention to the other flagship feature, and the one for which the Serenity milestone was originally created: the Casper proof of stake algorithm.</p>

<h3>Consensus By Bet</h3>

<p>The keystone mechanism of Casper is the introduction of a fundamentally new philosophy in the field of public economic consensus: the concept of <b>consensus-by-bet</b>. The core idea of consensus-by-bet is simple: the protocol offers opportunities for validators to bet <i>against the protocol</i> on which blocks are going to be finalized. A bet on some block X in this context is a transaction which, by protocol rules, gives the validator a reward of Y coins (which are simply printed to give to the validator out of thin air, hence "against the protocol") <i>in all universes in which block X was processed</i> but which gives the validator a penalty of Z coins (which are destroyed) in all universes in which block X was not processed.</p>

<p>The validator will wish to make such a bet only if they believe block X is likely enough to be processed in <i>the universe that people care about</i> that the tradeoff is worth it. And then, here's the economically recursive fun part: the universe that people care about, ie. the state that users' clients show when users want to know their account balance, the status of their contracts, etc, <i>is itself derived by looking at which blocks people bet on the most</i>. Hence, each validator's incentive is to bet in the way that they expect others to bet in the future, driving the process toward convergence.</p>

<p>A helpful analogy here is to look at proof of work consensus - a protocol which seems highly unique when viewed by itself, but which can in fact be perfectly modeled as a very specific subset of consensus-by-bet. The argument is as follows. When you are mining on top of a block, you are expending electricity costs <code>E</code> per second in exchange for receiving a chance <code>p</code> per second of generating a block and receiving <code>R</code> coins <i>in all forks containing your block</i>, and zero rewards in all other chains:</p>

<center><img src="https://blog.ethereum.org/wp-content/uploads/2015/12/drawing-7-1.png" style="width:500px"/></center>

<p>Hence, every second, you receive an expected gain of <code>p*R-E</code> on the chain you are mining on, and take a loss of <code>E</code> on all other chains; this can be interpreted as taking a bet at <code>E:p*R-E</code> odds that the chain you are mining on will "win"; for example, if p is 1 in 1 million, R is 25 BTC ~= $10000 USD and E is $0.007, then your gains per second on the winning chain are <code>0.000001 * 10000 - 0.007 = 0.003</code>, your losses on the losing chain are the electricity cost of <code>0.007</code>, and so you are betting at 7:3 odds (or 70% probability) that the chain you are mining on will win. Note that proof of work satisfies the requirement of being economically "recursive" in the way described above: users' clients will calculate their balances by processing the chain that has the most proof of work (ie. bets) behind it.</p>

<p>Consensus-by-bet can be seen as a framework that encompasses this way of looking at proof of work, and yet also can be adapted to provide an economic game to incentivize convergence for many other classes of consensus protocols. Traditional <a href="https://en.wikipedia.org/wiki/Byzantine_fault_tolerance">Byzantine-fault-tolerant</a> consensus protocols, for example, tend to have a concept of "pre-votes" and "pre-commits" before the final "commit" to a particular result; in a consensus-by-bet model, one can make each stage be a bet, so that participants in the later stages will have greater assurance that participants in the earlier stages "really mean it".</p>

<p>It can also be used to incentivize correct behavior in out-of-band human consensus, if that is needed to overcome extreme circumstances such as a 51% attack. If someone buys up half the coins on a proof-of-stake chains, and attacks it, then the community simply needs to coordinate on a patch where clients ignore the attacker's fork, and the attacker and anyone who plays along with the attacker automatically loses all of their coins. A very ambitious goal would be to generate these forking decisions automatically by online nodes - if done successfully, this would also subsume into the consensus-by-bet framework the underappreciated but important result from traditional fault tolerance research that, under strong synchrony assumptions, even if almost all nodes are trying to attack the system the remaining nodes <a href="http://research.microsoft.com/en-us/um/people/lamport/pubs/byz.pdf">can still come to consensus</a>.</p>

<p>In the context of consensus-by-bet, different consensus protocols differ in only one way: who is allowed to bet, at what odds and how much? In proof of work, there is only one kind of bet offered: the ability to bet on the chain containing one's own block at odds <code>E:p*R-E</code>. In generalized consensus-by-bet, we can use a mechanism known as a <a href="https://en.wikipedia.org/wiki/Scoring_rule">scoring rule</a> to essentially offer an infinite number of betting opportunities: one infinitesimally small bet at 1:1, one infinitesimally small bet at 1.000001:1, one infinitesimally small bet at 1.000002:1, and so forth.</p>

<center><img src="https://blog.ethereum.org/wp-content/uploads/2015/12/drawing-11.png" style="width:500px"/><br><small>A scoring rule as an infinite number of bets.</small></center>

<p>One can still decide exactly how large these infinitesimal marginal bets are at each probability level, but in general this technique allows us to elicit a very precise reading of the probability with which some validator thinks some block is likely to be confirmed; if a validator thinks that a block will be confirmed with probability 90%, then they will accept all of the bets below 9:1 odds and none of the bets above 9:1 odds, and seeing this the protocol will be able to infer this "opinion" that the chance the block will be confirmed is 90% with exactness. In fact, the <a href="https://en.wikipedia.org/wiki/Revelation_principle">revelation principle</a> tells us that we may as well ask the validators to supply a signed message containing their "opinion" on the probability that the block will be confirmed directly, and let the protocol calculate the bets on the validator's behalf.</p>

<p><center>
<img src="https://blog.ethereum.org/wp-content/uploads/2015/12/scoringrule.png" style="width:500px"/>
<small>Thanks to the wonders of calculus, we can actually come up with fairly simple functions to compute a total reward and penalty at each probability level that are mathematically equivalent to summing an infinite set of bets at all probability levels below the validator's stated confidence. A fairly simple example is <code>s(p) = p/(1-p)</code> and <code>f(p) = (p/(1-p))^2/2</code> where <code>s</code> computes your reward if the event you are betting on takes place and <code>f</code> computes your penalty if it does not.</small>
</center></p>

<p>A key advantage of the generalized approach to consensus-by-bet is this. In proof of work, the amount of "economic weight" behind a given block increases only linearly with time: if a block has six confirmations, then reverting it only costs miners (in equilibrium) roughly six times the block reward, and if a block has six hundred confirmations then reverting it costs six hundred times the block reward. In generalized consensus-by-bet, the amount of economic weight that validators throw behind a block could increase exponentially: if most of the other validators are willing to bet at 10:1, you might be comfortable sticking your neck out at 20:1, and once almost everyone bets 20:1 you might go for 40:1 or even higher. Hence, a block may well reach a level of "de-facto complete finality", where validators' entire deposits are at stake backing that block, in as little as a few minutes, depending on how brave the validators are (and how much the protocol incentivizes them to be).</p>

<br style="margin-top:20px" />

<big><center><em>50000-foot view summary: the blockchain is a prediction market on itself.</em></center></big>

<br style="margin-top:30px" />

<h3>Blocks, Chains and Consensus as Tug of War</h3>

<p>Another unique component of the way that Casper does things is that rather than consensus being <em>by-chain</em> as is the case with current proof of work protocols, consensus is <em>by-block</em>: the consensus process comes to a decision on the status of the block at each height independently of every other height. This mechanism does introduce some inefficiencies - particularly, a bet must register the validator's opinion on the block at every height rather than just the head of the chain - but it proves to be much simpler to implement strategies for consensus-by-bet in this model, and it also has the advantage that it is much more friendly to high blockchain speed: theoretically, one can even have a block time that is faster than network propagation with this model, as blocks can be produced independently of each other, though with the obvious proviso that block <i>finalization</i> will still take a while longer.</p>

<p>In by-chain consensus, one can view the consensus process as being a kind of tug-of-war between negative infinity and positive infinity <em>at each fork</em>, where the "status" at the fork represents the number of blocks in the longest chain on the right side minus the number of blocks on the left side:</p>

<center><img src="https://blog.ethereum.org/wp-content/uploads/2015/12/bchain_tugofwar.png" style="width:450px"/></center>

<p>Clients trying to determine the "correct chain" simply move forward starting from the genesis block, and at each fork go left if the status is negative and right if the status is positive. The economic incentives here are also clear: once the status goes positive, there is a strong economic pressure for it to converge to positive infinity, albeit very slowly. If the status goes negative, there is a strong economic pressure for it to converge to negative infinity.</p>

<p>Incidentally, note that under this framework the core idea behind the <a href="https://eprint.iacr.org/2013/881.pdf">GHOST scoring rule</a> becomes a natural generalization - instead of just counting the length of the longest chain toward the status, count every block on each side of the fork:</p>

<center><img src="https://blog.ethereum.org/wp-content/uploads/2015/12/bchain_tugofwar2.png" style="width:500px"/></center>

<p>In by-block consensus, there is once again the tug of war, though this time the "status" is simply an arbitrary number that can be increased or decreased by certain actions connected to the protocol; at every block height, clients process the block if the status is positive and do not process the block if the status is negative. Note that even though proof of work is currently by-chain, it doesn't have to be: one can easily imagine a protocol where instead of providing a parent block, a block with a valid proof of work solution must provide a +1 or -1 vote on every block height in its history; +1 votes would be rewarded only if the block that was voted on does get processed, and -1 votes would be rewarded only if the block that was voted on does not get processed:</p>

<center><img src="https://blog.ethereum.org/wp-content/uploads/2015/12/drawing-6.png" style="width:500px"/></center>

<p>Of course, in proof of work such a design would not work well for one simple reason: if you have to vote on absolutely <em>every</em> previous height, then the amount of voting that needs to be done will increase quadratically with time and fairly quickly grind the system to a halt. With consensus-by-bet, however, because the tug of war can converge to complete finality exponentially, the voting overhead is much more tolerable.</p>

<p>One counterintuitive consequence of this mechanism is the fact that a block can remain unconfirmed even when blocks after that block are completely finalized. This may seem like a large hit in efficiency, as if there is one block whose status is flip-flopping with ten blocks on top of it then each flip would entail recalculating state transitions for an entire ten blocks, but note that in a by-chain model the exact same thing can happen between chains as well, and the by-block version actually provides users with <i>more</i> information: if <i>their</i> transaction was confirmed and finalized in block 20101, and they know that <i>regardless of</i> the contents of block 20100 that transaction will have a certain result, then <i>the result that they care about</i> is finalized even though parts of the history before the result are not. By-chain consensus algorithms can never provide this property.</p>

<h3>So how does Casper work anyway?</h3>

<p>In any security-deposit-based proof of stake protocol, there is a current set of bonded validators, which is kept track of as part of the state; in order to make a bet or take one of a number of critical actions in the protocol, you must be in the set so that you can be punished if you misbehave. Joining the set of bonded validators and leaving the set of bonded validators are both special transaction types, and critical actions in the protocol such as bets are also transaction types; bets may be transmitted as independent objects through the network, but they can also be included into blocks.</p>

<p>In keeping with Serenity's spirit of abstraction, all of this is implemented via a <strong>Casper contract</strong>, which has functions for making bets, joining, withdrawing, and accessing consensus information, and so one can submit bets and take other actions simply by calling the Casper contract with the desired data. The state of the Casper contract looks as follows:</p>

<center><img src="https://blog.ethereum.org/wp-content/uploads/2015/12/drawing-8.png" style="width:600px"/></center>

<p>The contract keeps track of the current set of validators, and for each validator it keeps track of six primary things:</p>

<ul>
<li>The return address for the validator's deposit</li>
<li>The current size of the validator's deposit (note that the bets that the validator makes will increase or decrease this value)</li>
<li>The validator's <b>validation code</b></li>
<li>The sequence number of the most recent bet</li>
<li>The hash of the most recent bet</li>
<li>The validator's <b>opinion</b> table</li>
</ul>

<p>The concept of "validation code" is another abstraction feature in Serenity; whereas other proof of stake protocols require validators to use one specific signature verification algorithm, the Casper implementation in Serenity allows validators to specify a piece of code that accepts a hash and a signature and returns 0 or 1, and before accepting a bet checks the hash of the bet against its signature. The default validation code is an ECDSA verifier, but one can also experiment with other verifiers: multisig, threshold signatures (potentially useful for creating decentralized stake pools!), Lamport signatures, etc.</p>

<p>Every bet must contain a sequence number one higher than the previous bet, and every bet must contain a hash of the previous bet; hence, one can view the series of bets made by a validator as being a kind of "private blockchain"; viewed in that context, the validator's opinion is essentially the state of that chain. An opinion is a table that describes:</p>

<ul>
<li>What the validator thinks the most likely state root is at any given block height</li>
<li>What the validator thinks the most likely block hash is at any given block height (or zero if no block hash is present)</li>
<li>How likely the block with that hash is to be finalized</li>
</ul>

<p>A bet is an object that looks like this:</p>

<center><img src="https://blog.ethereum.org/wp-content/uploads/2015/12/drawing-9.png" style="width:500px"/></center>

<p>The key information is the following:</p>

<ul>
<li>The sequence number of the bet</li>
<li>The hash of the previous bet</li>
<li>A signature</li>
<li>A list of updates to the opinion</li>
</ul>

<p>The function in the Casper contract that processes a bet has three parts to it. First, it validates the sequence number, previous hash and signature of a bet. Next, it updates the opinion table with any new information supplied by the bet. A bet should generally update a few very recent probabilities, block hashes and state roots, so most of the table will generally be unchanged. Finally, it applies the scoring rule to the opinion: if the opinion says that you believe that a given block has a 99% chance of finalization, and if, in the particular universe that this particular contract is running in, the block was finalized, then you might get 99 points; otherwise you might lose 4900 points.</p>

<p>Note that, because the process of running this function inside the Casper contract takes place as part of the state transition function, <strong>this process is fully aware of what every previous block and state root is</strong> at least within the context of its own universe; even if, from the point of view of the outside world, the validators proposing and voting on block 20125 have no idea whether or not block 20123 will be finalized, when the validators come around to <em>processing</em> that block they will be - or, perhaps, they might process both universes and only later decide to stick with one. In order to prevent validators from providing different bets to different universes, we have a simple slashing condition: if you make two bets with the same sequence number, or even if you make a bet that you cannot get the Casper contract to process, you lose your entire deposit.</p>

<p>Withdrawing from the validator pool takes two steps. First, one must submit a bet whose maximum height is -1; this automatically ends the chain of bets and starts a four-month countdown timer (20 blocks / 100 seconds on the testnet) before the bettor can recover their funds by calling a third method, <code>withdraw</code>. Withdrawing can be done by anyone, and sends funds back to the same address that sent the original <code>join</code> transaction.</p>

<h3>Block proposition</h3>

<p>A block contains (i) a number representing the block height, (ii) the proposer address, (iii) a transaction root hash and (iv) a signature. For a block to be valid, the proposer address must be the same as the validator that is scheduled to generate a block for the given height, and the signature must validate when run against the validator's own validation code. The time to submit a block at height N is determined by <code>T = G + N * 5</code> where <code>G</code> is the genesis timestamp; hence, a block should ordinarily appear every five seconds.</p>

<p>An NXT-style random number generator is used to determine who can generate a block at each height; essentially, this involves taking <em>missing block proposers</em> as a source of entropy. The reasoning behind this is that even though this entropy is manipulable, manipulation comes at a high cost: one must sacrifice one's right to create a block and collect transaction fees in order to manipulate it. If it is deemed absolutely necessary, the cost of manipulation can be increased several orders of magnitude further by replacing the NXT-style RNG with a <a href="http://github.com/randao/randao">RANDAO</a>-like protocol.</p>

<h3>The Validator Strategy</h3>

<p>So how does a validator operate under the Casper protocol? Validators have two primary categories of activity: making blocks and making bets. Making blocks is a process that takes place independently from everything else: validators gather transactions, and when it comes time for them to make a block, they produce one, sign it and send it out to the network. The process for making bets is more complicated. The current default validator strategy in Casper is one that is designed to mimic aspects of traditional Byzantine-fault-tolerant consensus: look at how other validators are betting, take the 33rd percentile, and move a step toward 0 or 1 from there.</p>

<p>To accomplish this, each validator collects and tries to stay as up-to-date as possible on the bets being made by all other validators, and keeps track of the current opinion of each one. If there are no or few opinions on a particular block height from other validators, then it follows an initial algorithm that looks roughly as follows:</p>

<ul>
<li>If the block is not yet present, but the current time is still very close to the time that the block should have been published, bet 0.5</li>
<li>If the block is not yet present, but a long time has already passed since the block should have been published, bet 0.3</li>
<li>If the block is present, and it arrived on time, bet 0.7</li>
<li>If the block is present, but it arrived either far too early or far too late, bet 0.3</li>
</ul>

<p>Some randomness is added in order to help prevent "stuck" scenarios, but the basic principle remains the same.</p>

<p>If there are already many opinions on a particular block height from other validators, then we take the following strategy:</p>

<ul>
<li>Let <code>L</code> be the value such that two thirds of validators are betting higher than <code>L</code>. Let <code>M</code> be the median (ie. the value such that half of validators are betting higher than <code>M</code>). Let <code>H</code> be the value such that two thirds of validators are betting lower than <code>H</code>.</li>
<li>Let <code>e(x)</code> be a function that makes <code>x</code> more "extreme", ie. pushes the value away from 0.5 and toward 1. A simple example is the piecewise function <code>e(x) = 0.5 + x / 2 if x &gt; 0.5 else x / 2</code>.</li>
<li>If <code>L &gt; 0.8</code>, bet <code>e(L)</code></li>
<li>If <code>H &lt; 0.2</code>, bet <code>e(H)</code></li>
<li>Otherwise, bet <code>e(M)</code>, though limit the result to be within the range <code>[0.15, 0.85]</code> so that less than 67% of validators can't force another validator to move their bets too far</li>
</ul>

<center><img src="https://blog.ethereum.org/wp-content/uploads/2015/12/drawing-10.png" style="width:500px"/></center>

<p>Validators are free to choose their own level of risk aversion within the context of this strategy by choosing the shape of <code>e</code>. A function where <code>f(e) = 0.99999</code> for <code>e &gt; 0.8</code> could work (and would in fact likely provide the same behavior as Tendermint) but it creates somewhat higher risks and allows hostile validators making up a large portion of the bonded validator set to trick these validators into losing their entire deposit at a low cost (the attack strategy would be to bet 0.9, trick the other validators into betting 0.99999, and then jump back to betting 0.1 and force the system to converge to zero). On the other hand, a function that converges very slowly will incur higher inefficiencies when the system is not under attack, as finality will come more slowly and validators will need to keep betting on each height longer.</p>

<p>Now, how does a <b>client</b> determine what the current state is? Essentially, the process is as follows. It starts off by downloading all blocks and all bets. It then uses the same algorithm as above to construct its own opinion, but it does not publish it. Instead, it simply looks at each height sequentially, processing a block if its probability is greater than 0.5 and skipping it otherwise; the state after processing all of these blocks is shown as the "current state" of the blockchain. The client can also provide a subjective notion of "finality": when the opinion at every height up to some <code>k</code> is either above 99.999% or below 0.001%, then the client considers the first <code>k</code> blocks finalized.</p>

<h3>Further Research</h3>

<p>There is still quite a bit of research to do for Casper and generalized consensus-by-bet. Particular points include:</p>

<ul>
<li>Coming up with results to show that the system economically incentivizes convergence, even in the presence of some quantity of Byzantine validators</li>
<li>Determining optimal validator strategies</li>
<li>Making sure that the mechanism for <em>including the bets in blocks</em> is not exploitable</li>
<li>Increasing efficiency. Currently, the POC1 simulation can handle ~16 validators running at the same time (up from ~13 a week ago), though ideally we should push this up as much as possible (note that the number of validators the system can handle <em>on a live network</em> should be roughly the square of the performance of the POC, as the POC runs all nodes on the same machine).</li>
</ul>

<p>The next article in this series will deal with efforts to add a scaffolding for scalability into Serenity, and will likely be released around the same time as POC2.</p>