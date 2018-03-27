---
id: 70
title: 'Ethereum: Now Going Public'
date: 2014-01-23T10:12:18+00:00
author: Vitalik Buterin
layout: post
guid: http://blog.ethereum.org/?p=70
permalink: /2014/01/23/ethereum-now-going-public/
dsq_thread_id:
  - "2790049953"
---
I first wrote the initial draft of the Ethereum whitepaper on a cold day in San Francisco in November, as a culmination of months of thought and often frustrating work into an area that we have come to call “cryptocurrency 2.0″ – in short, using the Bitcoin blockchain for more than just money. In the months leading up to the development of Ethereum, I had the privilege to work closely with several projects attempting to implement colored coins, smart property, and various types of decentralized exchange. At the time, I was excited by the sheer potential that these technologies could bring, as I was acutely aware that many of the major problems still plaguing the Bitcoin ecosystem, including fraudulent services, unreliable exchanges, and an often surprising lack of security, were not caused by Bitcoin’s unique property of decentralization; rather, these issues are a result of the fact that there was still great centralization left, in places where it could potentially quite easily be removed.

What I soon realized, however, was the sheer difficulty that many of these projects were facing, and the often ugly technological hacks that were required to make them work. And, once one looks at the problem carefully, the culprit becomes obvious: fragmentation. Each individual project was attempting to implement its own blockchain or meta-layer on top of Bitcoin, and considerable effort was being duplicated and interoperability lost as a result. Eventually, I realized that the key to solving the problem once and for all was a simple insight that the field of computer science first conceived in 1935: there is no need to construct a separate infrastructure for each individual feature and implementation; rather, it is possible to create a Turing-complete programming language, and allow everyone to use that language to implement any feature that can be mathematically defined. This is how our computers work, and this is how our web browsers work; and, with Ethereum, this is how our cryptocurrencies can work.

Since that moment, Ethereum has come very far over the past two months. The Ethereum team has expanded to include dozens of developers including Gavin Wood and Jeffrey Wilcke, lead developers of the C++ and Go implementations, respectively, as well as others including Charles Hoskinson, Anthony Di Iorio and Mihai Alisie, and dozens of other incredibly talented individuals who are unfortunately too many to mention. Many of them have even come to understand the project so deeply as to be better at explaining Ethereum than myself. There are now over fifteen people in our developer chat rooms actively working on the C++ and Go implementations, which are already surprisingly close to having all the functionality needed to run in a testnet. Aside from development effort, there are dozens of people operating around the world in our marketing and community outreach team, developing the non-technical infrastructure needed to make the Ethereum ecosystem the solid and robust community that it deserves to be. And now, at this stage, we have made a collective decision that we are ready to take our organization much more public than we have been before.
<h3>What Is Ethereum</h3>
In short, Ethereum is a next-generation cryptographic ledger that intends to support numerous advanced features, including user-issued currencies, smart contracts, decentralized exchange and even what we think is the first proper implementation of decentralized autonomous organizations (DAOs) or companies (DACs). However, this is not what makes Ethereum special. Rather, what makes Ethereum special is the way that it does this. Instead of attempting to specifically support each individual type of functionality as a feature, Ethereum includes a built-in Turing-complete scripting language, which allows you to code the features yourself through a mechanism known as “contracts”. A contract is like an autonomous agent that runs a certain piece of code every time a transaction is sent to it, and this code can modify the contract’s internal data storage or send transactions. Advanced contracts can even modify their own code.

A simple example of a contract would be a basic name registration system, allowing users to register their name with their address. This contract would not send transactions; its sole purpose is to build up a database which other nodes can then query. The contract, written in our high-level <a href="http://wiki.ethereum.org/index.php/CLL">C-Like Language (CLL)</a> (or perhaps more accurately Python-like language), looks as follows:

if tx.value &lt; block.basefee * 200:
stop
if contract.storage[tx.data[0]] or tx.data[0] &lt; 100:
stop
contract.storage[tx.data[0]] = tx.data[1]

And there you have it. Five lines of code, executed simultaneously by thousands of nodes around the world, and you have the beginnings of a solution to a major problem in cryptography: human-friendly authentication. It is important to point out that when the original version of Ethereum’s scripting code was designed we did not have name registration in mind; rather, the fact that this is possible came about as an emergent property of its Turing-completeness. Hopefully this will give you an insight into exactly what Ethereum makes possible; for more applications, with code, see <a href="http://ethereum.org/ethereum.html">the whitepaper</a>. Just a few examples include:
<ul>
 	<li>User-issued currencies / “colored coins”</li>
 	<li>Decentralized exchange</li>
 	<li>Financial contracts, including leverage trading and hedging</li>
 	<li>Crop insurance</li>
 	<li>Savings wallets with withdrawal limits</li>
 	<li>Peer to peer gambling</li>
 	<li>Decentralized Dropbox-style data storage</li>
 	<li>Decentralized autonomous organizations</li>
</ul>
Perhaps now you see why we are excited.
<h3>Who is Ethereum</h3>
The core Ethereum team includes four members:

&nbsp;
<table>
<tbody>
<tr>
<td>Vitalik Buterin</td>
<td>Vitalik Buterin first joined the Bitcoin community in March 2011, and co-founded Bitcoin Magazine with Mihai Alisie in September 2011. He was admitted to the University of Waterloo to study computer science in 2012, and in 2013 made the decision to leave Waterloo to travel through Bitcoin communities around the world and work on Bitcoin projects full time. Vitalik is responsible for a number of Bitcoin projects, including <a href="http://blog.ethereum.org/2014/01/23/ethereum-now-going-public/github.com/vbuterin/pybitcointools">pybitcointools</a>, <a href="http://blog.ethereum.org/2014/01/23/ethereum-now-going-public/github.com/vbuterin/bitcoinjs-lib">a fork of BitcoinJS</a>and <a href="http://multisig.info/">multisig.info</a>; now, he has returned to Canada and is fully dedicated to working on Ethereum.</td>
</tr>
<tr>
<td>Mihai Alisie</td>
<td>Mihai Alisie’s first foray into the Bitcoin community is Bitcoin Magazine, in September 2011. From Issue #1, which was shipped from his living room in Romania, to today Bitcoin Magazine bears Mihai’s imprint, and has grown as he has grown with the magazine. What started out as a team of people that didn’t have any experience in the publishing industry, is now distributing a physical magazine internationally and in Barnes &amp; Noble bookstores across the US. Mihai is also involved in an innovative online e-commerce startup known as Egora.</td>
</tr>
<tr>
<td>Anthony Di Iorio</td>
<td>Anthony Di Iorio is a Founding Member, Board Member &amp; Executive Director of the <a href="http://www.bitcoinalliance.ca/">Bitcoin Alliance of Canada</a>, Founder of the <a href="http://www.meetup.com/Bitcoin-Toronto/">Toronto Bitcoin Meetup Group</a>, and partner / founder in various Bitcoin start-ups and initiatives including the in-browser Bitcoin wallet <a href="http://kryptokit.com/">KryptoKit</a>, <a href="http://cointalk.ca/">Cointalk</a>, the Toronto-based Bitcoin hub and coworking space<a href="http://bitcoindecentral.ca/">Bitcoin Decentral</a>, <a href="http://bitcoinacrossamerica.com/">Bitcoin Across America</a>, and the Global Bitcoin Alliance.</td>
</tr>
<tr>
<td>Charles Hoskinson</td>
<td>Charles Hoskinson is an entrepreneur and cryptographer actively working on ventures in the Bitcoin ecosystem. He founded both the Bitcoin Education Project and Invictus Innovations prior to accepting his current role as a core developer of the Ethereum Project. He studied at Metropolitan State University of Denver and University of Colorado at Boulder with an emphasis in Analytic Number Theory. Charles is known for his love of economics, horology and MOOCs alongside a passion for chess and games of strategy.</td>
</tr>
</tbody>
</table>
We also have an excellent team of developers, entrepreneurs, marketers and evangelists:
<ul>
 	<li><b>Dr. Gavin Wood</b>: Core C++ Developer</li>
 	<li><b>Geff Obscura</b>: Core Go Developer</li>
 	<li><b>Dr. Emanuele Costa</b>: Quantitative Analyst; SCRUM Master</li>
 	<li><b>Joseph Lubin</b>: Software Engineering, Quantitative Analyst</li>
 	<li><b>Eric Lombrozo</b>: Software Architect</li>
 	<li><b>Max Kaye</b>: Developer</li>
 	<li><b>Jonathan Mohan</b>: Media, Marketing and Evangelism (BitcoinNYC)</li>
 	<li><b>Wendell Davis</b>: Strategic Partner and Branding (Hive Wallet)</li>
 	<li><b>Anthony Donofrio</b>: Logos, branding, Web Development (Hive Wallet)</li>
 	<li><b>Taylor Gerring</b>: Web Development</li>
 	<li><b>Paul Snow</b>: Language Development, Software Development</li>
 	<li><b>Chris Odom</b>: Strategic Partner, Developer (Open Transactions)</li>
 	<li><b>Jerry Liu and Bin Lu</b>: Chinese strategy and translations (<a href="http://www.8btc.com/ethereum">http://www.8btc.com/ethereum</a>)</li>
 	<li><b>Hai Nguyen</b>: Accounting</li>
 	<li><b>Amir Shetrit</b>: Business Development (Colored Coins)</li>
 	<li><b>Steve Dakh</b>: Developer (KryptoKit)</li>
 	<li><b>kyle Kurbegovich</b>: Media (Cointalk)</li>
</ul>
<h3>Looking Forward</h3>
I personally will be presenting at the Bitcoin conference in Miami on Jan 25-26. Soon after that, on February 1, the ether pre-sale will begin, at which point anyone will be able to obtain some of the initial pre-allocated ether (Ethereum’s internal currency) at a rate of 1000-2000 ether for 1 BTC by going to <a href="http://fund.ethereum.org/">http://fund.ethereum.org</a>. The pre-sale will run throughout February and March, and early funders will get higher rewards; anyone who sends money in the first seven days will receive the full 2000 ether, then 1980 ether on the 8th day, 1960 on the 9th day, and so forth until the baseline rate of 1000 ether per BTC is retained for the last three days of the pre-sale.

We will be able to develop fully functional and robust Ethereum clients with as little as 500 BTC funding with current rates; basic implementations in Go, C++ and Python are coming close to testnet quality already. However, we are seeking to go much further than that. Ethereum is not “just another altcoin”; it is a new way forward for cryptocurrency, and ultimately for peer-to-peer protocols as a whole. To that end, we would like to be able to invest a large quantity of funds into securing top-notch talent for improving the security and scalability of the Ethereum network itself, but also supporting a robust Ethereum ecosystem hopefully bringing other cryptocurrency and peer-to-peer projects into our fold. We are already well underway in talks with KryptoKit, Humint and OpenTransactions, and are interested in working with other groups such as Tahoe-LAFS, Bitmessage and Bitcloud as well.

All of these projects can potentially benefit from integrating with the Ethereum blockchain in some fashion, simply because the layer is so universal; because of its Turing-completeness, an Ethereum contract can be constructed to incentivize nearly everything, and even entirely non-financial uses such as public key registration have extremely wide-reaching benefits for any decentralized cryptographic product that intends to include, for example, a social network. All of these projects will add great value to the Ethereum ecosystem, and the Ethereum ecosystem will hopefully add great value to them. We do not wish to compete with any organization; we intend to work together.

Throughout the fundraiser, we will be working hard on development; we will release a centralized testnet, a server to which anyone can push contracts and transactions, very soon, and will then follow up with a decentralized testnet to test networking features and mining algorithms. We also intend to host a contest, similar to those used to decide the algorithms for the Advanced Encryption Standard (AES) in 2005 and SHA3 in 2013, in which we invite researchers from universities around the world to compete to develop the best possible specialized hardware-resistant, centralization-resistant and fair mining algorithms, and will also explore alternatives such as proof of stake, proof of burn and proof of excellence. Details on this will be further released in February.

Finally, to promote local community development, we also intend to create public community hubs and incubators, which we are tentatively calling “holons”, in several cities around the world. The first holon will be based inside of Bitcoin Decentral in Toronto, and a substantial portion of Ethereum development will take place there; anyone who is seriously interested in participating heavily in Ethereum should consider giving us a visit over the next month. Other cities we are looking into include San Francisco, Amsterdam, Tel Aviv and some city in Asia; this part of the project is still in a very early phase of development, and more details will come over the next month.

For now feel free to check out our resources:
<ul>
 	<li><b>Forum: </b><a href="http://forum.ethereum.org/">http://forum.ethereum.org</a></li>
 	<li><b>Blog: </b><a href="http://blog.ethereum.org/">http://blog.ethereum.org</a></li>
 	<li><b>Wiki: </b><a href="http://wiki.ethereum.org/">http://wiki.ethereum.org</a></li>
 	<li><b>Whitepaper: </b><a href="http://ethereum.org/ethereum.html">http://ethereum.org/ethereum.html</a></li>
</ul>
<b>Reddit: </b><a href="http://reddit.com/r/ethereum">http://reddit.com/r/ethereum</a>