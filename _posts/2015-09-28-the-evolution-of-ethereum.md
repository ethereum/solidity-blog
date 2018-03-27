---
id: 2179
title: The Evolution of Ethereum
date: 2015-09-28T17:43:55+00:00
author: Vitalik Buterin
layout: post
guid: https://blog.ethereum.org/?p=2179
permalink: /2015/09/28/the-evolution-of-ethereum/
dsq_thread_id:
  - "4173878727"
---
Many of you know that the Ethereum platform grew out of the realization that blockchains can go far beyond currency, together with a frustration with the limitations of previous projects. The core idea was simple: a blockchain with a built-in Turing-complete programming language, allowing users to build any kind of applications on top.

Over time, the vision evolved and expanded. The blockchain remains a crucial centerpiece, but it is ultimately only part of a larger vision of “web 3.0” <a href="https://bitcoinmagazine.com/articles/web-3-0-chat-ethereums-gavin-wood-1398455401">as described by Gavin Wood here</a>: a more secure, trustworthy and globally accessible internet for agreements, finance, auditing, tracking and simple websites and web applications that use decentralized technology to overcome some of the practical, political and technological inefficiencies of previous approaches. Together with the Ethereum blockchain, we see an entire suite of internally and externally developed, low-level and high-level protocols including Solidity, Whisper, IPFS, zero knowledge proof systems, account management systems, dedicated browsers and much more, all with the goal of providing a coherent vision of the internet as it should be.

With such an ambitious vision, come some challenges. Right now, the Ethereum project is in a time of complicated transition. Many of the difficult initial work - myself, Gavin, Jeff, Martin, Lefteris, Felix, Vlad and many others <a href="http://github.com/ethereum/cpp-ethereum/">developing</a> <a href="http://github.com/ethereum/go-ethereum/">four</a> <a href="http://github.com/ethereum/pyethereum/">compatible</a> <a href="http://github.com/ethereum/ethereumjs-lib">versions</a> of a project that our security auditors have described as having “testing needs … more complex than anything [they’ve] looked at before”, Christoph and Dmitry’s tireless efforts setting up over ten thousand <a href="https://github.com/ethereum/tests/">tests</a>, Marian, Taylor and Konstantin's work on <a href="http://stats.ethdev.com/">network analysis</a> and emergency response architecture, Christian, Liana and Gavin’s work on getting <a href="https://github.com/ethereum/solidity/">Solidity</a> off the ground, IMAPP's work on the <a href="https://github.com/ethereum/evmjit">JIT EVM</a>, and the many other projects of contributors to the Ethereum platform  of which there are too many to mention, all culminating with the successful launch of a blockchain with over $50 million worth of value floating around on it starting from day one, are now behind us. Ethereum now actually exists, and those who were lucky enough to take up <a href="http://www.contravex.com/2014/07/23/a-guide-to-buying-5000-ether-bitcoin-2-5x-more-than-ethereums-genesis-sale/">Mircea Popescu's offer</a> to buy ETH from him that he doesn't have at a price of $0.12 a piece are welcome to try their best to collect. Today, we can all be proud that the Ethereum developer ecosystem has grown large enough to include <a href="http://www.ibtimes.co.uk/ubs-barclays-bnp-paribas-are-front-runners-bitcoin-2-0-technology-ethereum-1514138">major banks</a>, corporations, governments, <a href="http://dapps.ethercasts.com/">over a hundred dapps</a> and individuals and businesses in dozens of countries speaking dozens of languages.

At the same time, however, there are some difficult challenges that remain: some technical, some organizational, and some of almost every kind. The core of the problem is simple. Up until fairly recently, almost all of the work that has been done on the Ethereum project has been done by subsidiaries of the foundation. In the future, however, although the foundation and its subsidiaries are going to continue to take on a strong and leading role, it will be the community that will gradually be the primary driver in making it succeed. This is true for several reasons, some unplanned and some positive.

First of all, it is indeed true that the foundation’s finances are limited, and a large part of this was the result of our failure to sell nearly as much of our BTC holdings as we were planning to before the price dropped to $220; as a result, we suffered roughly $9m in lost potential capital , and a hiring schedule that was meant to last over three years ended up lasting a little under two (although bolstered by a “second wind” from our ETH holdings).

Second, the project’s needs have grown. Over the past twenty months, the project has grown from being a simple attempt to improve on Mastercoin by adding a programming language into an effort to push forward a powerful and expansive vision of "web 3.0" that includes multiple technologies, some built by ourselves and some by others, and a complex software stack that integrates them all with one simple aim: to make it as easy to build secure, globally accessible and trust-minimized decentralized applications as it is to build a website - and hopefully even easier.

The foundation and its subsidiaries alone simply do not have the manpower to push the entirety of this vision through to its ultimate completion, including proof-of-stake driven scalable blockchains, seamlessly integrated distributed hash tables, programming languages with formal verification systems backed by state-of-the-art theorem provers and dozens of categories of middleware, all by itself; although the foundation and its subsidiaries can, and will, continue to be the primary driver of technology at the core, a highly community-driven model is necessary and essential, both to help the Ethereum ecosystem maximally grow and flourish and to establish Ethereum as a decentralized project which is ultimately owned by all of humanity, and not any one group.

And fortunately, the community has already stepped up. Just to give a few examples, here are a few parts of the Ethereum ecosystem that the Ethereum Foundation and its subsidiaries  have had nothing to do with:
<ul>
<ul>
	<li><a href="http://augur.net"><b>Augur</b></a>: a prediction market that has earned $4.5 million in its recent (and still ongoing) crowdsale</li>
	<li><a href="http://groupgnosis.com/"><b>GroupGnosis</b></a>: another prediction market being developed by Consensys which is already processing bets on the Ethereum block difficulty, sports games, and soon presidential elections</li>
	<li><a href="https://github.com/iurimatias/embark-framework/releases/tag/0.9.0"><b>Embark</b></a>: a NodeJS-based dapp development, testing and deployment framework</li>
	<li><a href="https://github.com/ConsenSys/truffle"><b>Truffle</b></a>: another dapp development, testing and deployment framework</li>
	<li><a href="http://frontier.ether.camp"><b>Ether.camp</b></a>: a block explorer</li>
	<li><a href="http://etherscan.io"><b>etherscan.io</b></a>: another block explorer</li>
	<li><a href="https://tradeblock.com/blog"><b>TradeBlock</b></a>: did I forget to say there’s another ethereum block explorer?</li>
	<li><a href="https://etherex.org/"><b>EtherEx</b></a>: an Ethereum-based asset exchange</li>
	<li>The Ether.camp web-based integrated development environment (coming soon)</li>
	<li><a href="http://ethereumwallet.com"><b>EthereumWallet.com</b></a>: an online ether wallet</li>
	<li><strong><a href="http://github.com/ethereum/ethereumj">The Ethereum Java implementation</a></strong> (for which original work was done under the Foundation, but which is now continuing completely independently)</li>
	<li><strong><a href="https://github.com/jamshidh/ethereum-client-haskell">And the Ethereum Haskell implementation</a></strong>, this time with none of our involvement at all!</li>
	<li><a href="http://myetherwallet.com/"><b>MyEtherWallet</b></a>: another ether wallet</li>
	<li><a href="https://www.youtube.com/watch?v=A6PwkIneLXU"><b>Metamask</b></a>: an ethereum browser-in-a-browser</li>
	<li><a href="https://eng.erisindustries.com/tutorials/2015/03/11/solidity-1/"><b>Andreas Oloffson’s development tutorials</b></a></li>
	<li><a href="http://feed.ether.camp/"><b>The first data feed contract</b></a></li>
	<li><b><a href="http://www.ethereum-alarm-clock.com/"><b>Ethereum Alarm Clock</b></a><b>,</b></b> an implementation of one our <a href="https://github.com/ethereum/wiki/wiki/Serenity_Wishlist">major planned features</a> for Ethereum 1.1, but as a decentralized middleware service right on the 1.0 Ethereum blockchain!</li>
	<li><a href="http://dapps.ethercasts.com"><b>dapps.ethercasts.com</b></a>: a webpage listing many of the above, and more (no, I won't mention the ponzies and gambling sites, except insofar as to credit Martin Holst Swende's <a href="http://martin.swende.se/blog/Breaking_the_house.html">wonderful work</a> in documenting the perils of building a blockchain-based casino with a bad random number generator, and Qian Youcai's <a href="https://forum.ethereum.org/discussion/2031/randao-a-dao-working-as-rng">ongoing work on RANDAO</a> to make this situation better).</li>
</ul>
</ul>
Actually, the Ethereum ecosystem is maturing nicely, and looks unrecognizable from what it was barely a year ago.

On the inside, we have Ethereum Foundation subsidiary developers building <a href="http://etherchain.org">yet more block explorers</a> and other tools in their spare time, and some developers are already working on implementing Ethereum-based lightning networks, identity and reputation systems, and more. In the near future, there will be several more non-profit and for-profit entities emerging in and around the space, some with the involvement of Ethereum team members, and many with partial involvement from myself. The first of these to announce itself is the <a href="http://www.8btc.com/wanxiang-blockchain-labs-20151015">Wanxiang blockchain research institute and fund based in Shanghai</a> (yes, this is the “major collaboration” I hinted on recently, and is also my much delayed answer to “<a href="https://www.reddit.com/r/ethereum/comments/3iaq7k/ethereum_update/">how did your China trip go?</a>”), which includes (i) an agreement to purchase 416K ETH, which has <a href="https://etherchain.org/tx/0xdd3c55c3e25e30d2c81eb53ff770189364390c5ec0eb85c8a2d5ec9aa8155958">already</a> <a href="https://etherchain.org/tx/0x8d8c49d7855c248ca3b149d853d8d343f23fb0df8a54215a24dedeee32258b88">concluded</a>, (ii) an upcoming conference in October, (iii) a non-profit blockchain research institute, and (iv) a $50m blockchain venture-capital fund, all with emphasis on Ethereum development. I fully expect that within six months the Ethereum for-profit ecosystem may well be much more well-capitalized than the Foundation itself.

Note that a substantial number of Ethereum foundation subsidiary staff is going to be moving over to the rapidly growing for-profit Ethereum ecosystem over the next half year in order to bring more funds, interest and development effort into Ethereum-land; so far, everyone I have talked to who is leaving the foundation subsidiaries is intending to do this, and they will in many cases simply be continuing, and expanding, the same work that they have started on now either under foundation subsidiary employment or as personal side projects, under a different banner. <a href="https://twitter.com/mingchan88">Ming Chan</a>, who has recently joined the foundation, will be managing the foundation’s administrative matters, helping to develop an updated and more detailed strategic plan, oversee DEVcon 1 setup, and generally make sure that things on the foundation side work smoothly throughout the many simultaneous transitions that are taking place; we have also expanded our advisory board, and the new advisors will be announced soon.

Under these circumstances, we must thus ask, what is the Foundation going to do (and not do)?
<h3><b>Finances</b></h3>
Let us start off by providing an overview of the Foundation's financial situation. Its current holdings are roughly:
<ul>
	<li>200,000 CHF</li>
	<li>1,800 BTC</li>
	<li>2,700,000 ETH</li>
</ul>
Plus a 490,000 CHF legal fund that will be reserved to cover possible legal defense (it’s like insurance). The foundation's monthly expenditures are currently ~410,000 CHF and starting Oct 1 are projected to fall to 340,000 CHF; a mid-term goal has been placed of 200,000 - 250,000 CHF as a good target that allows us to deliver on our remaining, but important responsibilities. Assuming that we get there in three months and that ether and bitcoin prices stay the same (heh), we have enough to last until roughly Jun 2016 at the 340,000 rate, and perhaps up to Sep-Dec 2016 given planned transitions; by that point, the intent is for the foundation to secure alternative revenue sources.

Possible revenue sources past that point include:
<ul>
	<li>Developer workshops (including extended in-person “courses”)</li>
	<li>Conference tickets and sponsorships</li>
	<li>Third-party donations and grants (whether to the Foundation or to projects that the Foundation would otherwise be spending resources on)</li>
</ul>
Another action that may be taken is, when Ethereum switches to proof of stake, keeping 50% of the old issuance rate active for a year and directing the issuance into some kind of mechanism, perhaps a simple voting scheme or perhaps something more complex incorporating delegated voting, decision markets and potentially other revealed-preference tricks from game theory, in order to pay developers. In any case, our original promise that the issuance rate will not exceed 26.00% per year, and the goal that the eventual final issuance will be much lower (likely 0-3% per year) with proof of stake, will both be kept. We highly welcome community input on whether and how to go down this path; if there is large opposition we will not do this, though the community should understand that NOT doing this comes with a risk of greater reliance on the for-profit ethereum ecosystem.
<h3><b>Focus</b></h3>
Up until perhaps six months ago, the Foundation and its subsidiaries have been doing almost everything in the ecosystem; right now, the foundation and its subsidiaries are still doing much of everything though some community members have stepped up to compete with its own offerings - in some cases, in my own humble opinion, quite excellently. Going forward, the Foundation and its subsidiaries will aim for a more focused approach where it carries out only some of the work in the ecosystem, but does it well. An approximate outline of the Foundation's activities can be described as follows:
<ul>
	<li>Education
<ul>
	<li>Online documentation and developer resources (new documentation site coming soon!)</li>
	<li>Conferences (<a href="https://blog.ethereum.org/2015/09/24/devcon-is-back/">DEVcon 1</a> coming in November!)</li>
	<li>Hackathons, workshops</li>
	<li>Possibly paid in-person development courses</li>
</ul>
</li>
	<li>Conferences, events, Meetups co-ordination</li>
	<li>Outreach, marketing and evangelism, both to the media/public and to institutions</li>
	<li>Compliance and regulatory maintenance</li>
	<li>Certifying businesses, individuals, etc (whether ourselves or through a third-party partner)</li>
	<li>Highly targeted core development tasks including:
<ul>
	<li>Some core client code</li>
	<li>Network observation and coordinating emergency response</li>
	<li>Maintaining test suites, certifying clients</li>
	<li>Paying for some security audits</li>
</ul>
</li>
	<li>Research, including:
<ul>
	<li>Proof of stake (Casper)</li>
	<li>Scalability</li>
	<li>Virtual machine upgrades</li>
	<li>Abstraction</li>
	<li>Formal verification</li>
	<li>Zero-knowledge proof integration</li>
</ul>
</li>
	<li>Official protocol and sub-protocol specifications</li>
</ul>
Higher-level development tasks will in the medium term be done largely by for-profit entities, volunteers and other members of the community, although the Foundation’s subsidiaries will continue to employ many of the developers in the short term.
<h3><b>Transparency</b></h3>
The Ethereum Foundation would like to express a renewed interest in being maximally transparent in its affairs; to that end, we are publishing the information above, and as an initial trial in going further we are working with<a href="http://consensys.net"> Consensys</a> to use their (Ethereum) blockchain-based accounting software Balanc3 to record all expenses relating to Devcon 1.

Another important aspect of transparency is more open and inclusive development; to that end, we are making a renewed push to move conversations from Skype to Gitter where they are more publicly visible (eg. you can check out <a href="https://gitter.im/ethereum/web3.js">this room</a> right now) and members of the public can more easily participate. We are also evaluating the possibility of introducing a more formal and inclusive process for agreeing on protocol upgrades and welcome input from client developers on this. And there are more announcements both from ourselves and others that will be following soon.

In sum, despite the evidence of growing pains, the state of the Ethereum nation is good, its ecosystem is vibrant, and its future is bright.

As a Foundation, we will continue to focus on promoting and supporting research, development and education to bring decentralized protocols and tools to the world that empower developers to produce next generation (d)apps, and together build a more globally accessible, more free and more trustworthy Internet.