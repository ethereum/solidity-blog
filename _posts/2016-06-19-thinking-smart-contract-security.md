---
id: 2746
title: Thinking About Smart Contract Security
date: 2016-06-19T01:30:06+00:00
author: Vitalik Buterin
layout: post
guid: https://blog.ethereum.org/?p=2746
permalink: /2016/06/19/thinking-smart-contract-security/
dsq_thread_id:
  - "4921272859"
---
Over the last day with the community's help we have crowdsourced a list of all of the major bugs with smart contracts on Ethereum so far, including both the DAO as well as various smaller 100-10000 ETH thefts and losses in games and token contracts.

This list (original source <a href="https://www.reddit.com/r/ethereum/comments/4omdlf/to_kickstart_the_building_safer_smart_contracts/">here</a>) is as follows:
<ul>
 	<li>The DAO (obviously)</li>
 	<li>The "payout index without the underscore" <a href="https://www.reddit.com/r/ethereum/comments/4e5y30/live_example_of_underhanded_solidity_coding_on/">ponzi</a> ("FirePonzi")</li>
 	<li>The casino with a <a href="http://martin.swende.se/blog/Breaking_the_house.html">public RNG seed</a></li>
 	<li><a href="https://www.reddit.com/r/ethereum/comments/4ghzhv/governmentals_1100_eth_jackpot_payout_is_stuck/">Governmental</a> (1100 ETH stuck because payout exceeds gas limit)</li>
 	<li><a href="https://www.reddit.com/r/MakerDAO/comments/4niu10/critical_ether_token_wrapper_vulnerability_eth/">5800 ETH swiped</a> (by whitehats) from an ETH-backed ERC20 token</li>
 	<li>The <a href="http://www.kingoftheether.com/postmortem.html">King of the Ether game</a></li>
 	<li>Rubixi : Fees stolen because the <a href="https://etherscan.io/address/0xe82719202e5965Cf5D9B6673B7503a3b92DE20be#code">constructor function</a> had an incorrect name, allowing <a href="https://bitcointalk.org/index.php?topic=1400536.60">anyone to become the owner</a></li>
 	<li>Rock paper scissors <a href="https://www.reddit.com/r/ethtrader/comments/4fpn6o/play_rockpaperscissors_for_1_eth_via_mist_wallet/">trivially cheatable</a> because the first to move shows their hand</li>
 	<li>Various instances of funds lost because a recipient contained a fallback function that consumed more than 2300 gas, causing sends to them to fail.</li>
 	<li>Various instances of call stack limit exceptions.</li>
</ul>
We can categorize the list by categories of bugs:
<ul>
 	<li>Variable/function naming mixups: FirePonzi, Rubixi</li>
 	<li>Public data that should not have been public: the public RNG seed casino, cheatable RPS</li>
 	<li>Re-entrancy (A calling B calling A): the DAO, Maker's ETH-backed token</li>
 	<li>Sends failing due to 2300 gas limit: King of the Ether</li>
 	<li>Arrays/loops and gas limits: Governmental</li>
 	<li>Much more subtle game-theoretic weaknesses where at the limit people even debate whether or not they're bugs: the DAO</li>
</ul>
There have been many solutions proposed to smart contract safety, ranging from better development environments to better programming languages to formal verification and symbolic execution, and researchers have <a href="http://hackingdistributed.com/2016/06/16/scanning-live-ethereum-contracts-for-bugs/">started developing such tools</a>. My personal opinion regarding the topic is that an important primary conclusion is the following: <strong>progress in smart contract safety is necessarily going to be layered, incremental, and necessarily dependent on defense-in-depth</strong>. There <strong>will</strong> be further bugs, and we will learn further lessons; there <strong>will not</strong> be a single magic technology that solves everything.

The reason for this fundamental conclusion is as follows. All instances of smart contract theft or loss - in fact, <em>the very definition</em> of smart contract theft or loss, is fundamentally about differences between implementation and intent. If, in a given case, implementation and intent are the same thing, then any instance of "theft" is in fact a donation, and any instance of "loss" is voluntary money-burning, economically equivalent to a proportional donation to the ETH token holder community by means of deflation. This leads to the next challenge: <em>intent is fundamentally complex</em>.

The philosophy behind this fact has been best formalized by the friendly AI research community, where is bears the names of "<a href="https://wiki.lesswrong.com/wiki/Complexity_of_value">complexity of value</a>" and "<a href="http://lesswrong.com/lw/y3/value_is_fragile/">fragility of value</a>". The thesis is simple: we as human beings have very many values, and very complex values - so complex that we ourselves are not capable of fully expressing them, and any attempt to will inevitably contain some uncovered corner case. The utility of the concept to AI research is important because a super-intelligent AI would in fact search through every corner, including corners that we find so unintuitive that we do not even think of them, to maximize its objective. Tell a superintelligent AI to cure cancer, and it will get 99.99% of the way there through some moderately complex tweaks in molecular biology, but it will soon realize that it can bump that up to 100% by triggering human extinction through a nuclear war and/or biological pandemic. Tell it to cure cancer without killing humans, and it will simply force all humans to freeze themselves, reasoning that it's not technically killing because it could wake the humans up if it wanted to - it just won't. And so forth.

In smart contract land, the situation is similar. We believe that we value things like "fairness", but it's hard to define what fairness even means. You may want to say things like "it should not be possible for someone to just steal 10000 ETH from a DAO", but what if, for a given withdrawal transaction, the DAO actually approved of the transfer because the recipient provided a valuable service? But then, if the transfer was approved, how do we know that the mechanism for deciding this wasn't fooled through a game-theoretic vulnerability? What is a game-theoretic vulnerability? What about "splitting"? In the case of a blockchain-based market, what about front-running? If a given contract specifies an "owner" who can collect fees, what if the ability for anyone to become the owner was actually part of the rules, to add to the fun?

All of this is not a strike against experts in formal verification, type theory, weird programming languages and the like; the smart ones already know and appreciate these issues. However, it does show that there is a fundamental barrier to what can be accomplished, and "fairness" is not something that can be mathematically proven in a theorem - in some cases, the set of fairness claims is so long and complex that you have to wonder if the set of claims itself might have a bug.

<h3>Toward a Mitigation Path</h3>

That said, there are plenty of areas where divergence between intent and implementation can be greatly reduced. One category is to try to take common patterns and hardcode them: for example, the Rubixi bug could have been avoided by making <code>owner</code> a keyword that could only be initialized to equal <code>msg.sender</code> in the constructor and possibly transferred in a <code>transferOwnership</code> function. Another category is to try to create as many standardized mid-level components as possible; for example, we may want to discourage every casino from creating its own random number generator, and instead direct people to <a href="https://github.com/randao/randao">RANDAO</a> (or something like <a href="https://www.reddit.com/r/ethereum/comments/4mdkku/could_ethereum_do_this_better_tor_project_is/d3v6djb">my RANDAO++ proposal</a>, once implemented).

A more important category of solutions, however, involve mitigating the specific and unintuitive quirks of the EVM execution environment. These include: the gas limit (responsible for the Governmental loss, as well as the losses due to recipients consuming too much gas when accepting a send), re-entrancy (responsible for the DAO and the Maker ETH contract), and the call stack limit. The call stack limit, for example, can be mitigated through <a href="https://github.com/ethereum/EIPs/issues/114">this EIP</a>, which essentially removes it from consideration by substituting its purpose with a change to gas mechanics. Re-entrancy could be banned outright (ie. only one execution instance of each contract allowed at a time), but this would likely introduce new forms of unintuitiveness, so a better solution is likely required.

The gas limit, however, is not going away; hence, the only solutions there are likely to be inside of the development environment itself. Compilers should throw a warning if a contract does not provably consume less than 2300 gas if called with no data; they should also throw a warning if a function does not provably terminate within a safe amount of gas. Variable names might be colored (eg. RGB based on the first three bytes of the hash of the name), or perhaps a heuristic warning might be given if two variable names are too close to each other.

Additionally, there are coding patterns that are more dangerous than others, and while they should not be banned, they should be clearly highlighted, requiring developers to justify their use of them. A particularly involved example is as follows. There are two types of call operations that are clearly safe. The first is a send that contains 2300 gas (provided we accept the norm that it is the recipient's responsibility not to consume more than 2300 gas in the case of empty data). The second is a call to a contract that you trust and that is itself already determined to be safe (note that this definition bans re-entrancy as you would then have to prove A is safe before proving A is safe).

As it turns out, very many contracts can be covered by this definition. However, not all of them can; an exception is the idea of a "general purpose decentralized exchange" contract where anyone can place orders offering to trade a given amount of asset A for a given amount of asset B, where A and B are arbitrary <a href="https://github.com/ethereum/EIPs/issues/20">ERC20-compatible</a> tokens. One could make a special-purpose contract just for a few assets, and thereby fall under the "trusted callee" exemption, but having a generic one seems like a very valuable idea. But in that case, the exchange would need to call <code>transfer</code> and <code>transferFrom</code> of unknown contracts and, yes, give them enough gas to run and possibly make a re-entrant call to try to exploit the exchange. In this case, the compiler may want to throw a clear warning unless a "mutex lock" is used preventing the contract from being accessed again during those calls.

A third category of solutions is defense in depth. One example, to prevent losses (but not thefts) is to encourage all contracts that are not intended to be permanent to have an expiry date, after which the owner can take arbitrary actions on behalf of the contract; this way, losses would be possible only if (i) the contract screws up, and simultaneously (ii) the owner is missing or dishonest. Trusted multisig "owners" may emerge to mitigate (ii). Thefts could be mitigated by adding waiting periods. The DAO issue was greatly mitigated in scope precisely because the child DAO was locked down for 28 days. A proposed feature in the MakerDAO is to create a delay before any governance change becomes active, allowing token holders unhappy with the change time to sell their tokens; this is also a good approach.

Formal verification can be layered on top. One simple use case is as a way of proving termination, greatly mitigating gas-related issues. Another use case is proving specific properties - for example, "if all participants collude, they can get their money out in all cases", or "if you send your tokens A to this contract, you are guaranteed to either get the amount of token B that you want or be able to fully refund yourself". Or "this contract fits into a restricted subset of Solidity that makes re-entrancy, gas issues and call stack issues impossible".

A final note is that while all of the concerns so far have been about accidental bugs, malicious bugs are an additional concern. How confident can we really be that the MakerDAO decentralized exchange does not have a loophole that lets them take out all of the funds? Some of us in the community may know the MakerDAO team and consider them to be nice people, but the entire purpose of the smart contract security model is to provide guarantees that are strong enough to survive even if that is not the case, so that entities that are not well-connected and established enough for people to trust them automatically and do not have the resources to establish their trustworthiness via a multimillion-dollar licensing process are free to innovate, and have consumers use their services feeling confident about their safety. Hence, any checks or highlights should not just exist at the level of the development environment, they should also exist at the level of block explorers and other tools where independent observers can verify the source code.

Particular action steps that can be taken by the community are:
<ol>
 	<li>Taking on the project of making a superior development environment, as well as a superior block/source code explorer, that includes some of these features</li>
 	<li>Standardization of as many components as possible</li>
 	<li>Taking on the project of experimenting with different smart contract programming languages, as well as formal verification and symbolic execution tools</li>
 	<li>Discussing coding standards, EIPs, changes to Solidity, etc that can mitigate the risk of accidental or deliberate errors</li>
 	<li>If you are developing a multimillion-dollar smart contract application, consider reaching out to security researchers and work with them on using your project as a test case for various verification tools</li>
</ol>
Note that, as stated in a previous blog post, DEVGrants and other grants are available for much of the above.