---
id: 2599
title: Homestead Release
date: 2016-02-29T16:24:08+00:00
author: Jeffrey Wilcke
layout: post
guid: https://blog.ethereum.org/?p=2599
permalink: /2016/02/29/homestead-release/
dsq_thread_id:
  - "4621704539"
---
<strong>Update: We've released <a href="https://github.com/ethereum/go-ethereum/releases/tag/v1.3.5">version 1.3.5</a> including networking hotfix for homestead 1.3.4. </strong>

<span style="font-weight: 400;">Development of Ethereum started in December 2013 when two developers and a college dropout researcher decided to put their heads together and develop this amazing piece of technology. We were later</span><a href="https://www.youtube.com/watch?v=GA8z7f7a2Pk"><span style="font-weight: 400;"> joined</span></a><span style="font-weight: 400;"> by many like-minded individuals. Our first Proof of Concept (PoC) came on February 1, 2014. By the time we started our crowdsale on July 23, we were up to PoC 5 with a protocol that was almost finished, and had achieved compatibility between multiple clients. The team was psyched when we got our first million and our second and our third and so on. It was crazy! Believing that we could deliver the Ethereum platform was one thing, but seeing others believe and want to participate it was incredibly inspiring.</span>

<span style="font-weight: 400;">Somewhat later in October of 2014, Gav and I were sitting on the train to the Zurich airport when we figured that an Ethereum conference would be absolutely awesome, and settled on doing it in November that same year. It was a bit hectic for those who organised it (thanks Aeron, Christian &amp; Jason!), but we managed to pull it off as we were determined to host our first internal developers conference, </span><a href="https://www.youtube.com/playlist?list=PLJqWcTqh_zKEjpSej3ddtDOKPRGl_7MhS"><b>DEV</b><span style="font-weight: 400;">con0</span></a><span style="font-weight: 400;">.</span>

<a href="https://blog.ethereum.org/wp-content/uploads/2016/02/CRUGRsgWoAAAgSQ.jpg-large.jpeg" rel="attachment wp-att-2601"><img class="alignnone size-full wp-image-2601" src="https://blog.ethereum.org/wp-content/uploads/2016/02/CRUGRsgWoAAAgSQ.jpg-large.jpeg" alt="CRUGRsgWoAAAgSQ.jpg-large" width="1024" height="768" /></a>

<span style="font-weight: 400;">When January 2015 came around, we all knew this would be the year we'd have to launch our beta version of Ethereum. Our PoC series was closed off with the PoC 9 hackathon on March 5th and on July 30, 2015 we successfully launched Frontier. Miners from all over the globe booted up their mining rigs, users fired up their nodes and the network came to life. It worked; no hiccups, no issues and from a developer's perspective the release couldn't have gone smoother. In the following months, our team and community demonstrated its potential and true awesomeness in dealing with issues on the network and together solving two consensus bugs.</span>

<span style="font-weight: 400;">We felt it was extremely important to clearly communicate to our users how we felt about the security of the network. Today, we’re incredibly proud to announce that we are finally ready to remove the scratched out word “safe” from our website as move into a new phase: Homestead. The Homestead block will be <strong>1.150.000</strong> for the main network which means the Homestead transition will be roughly around midday on Pi day and the Homestead block for the Morden network will be <strong>494.000</strong></span><span style="font-weight: 400;">.</span>
<h2><span style="font-weight: 400;">What is Homestead?</span></h2>
<span style="font-weight: 400;">Homestead is the second major version release of the Ethereum platform, which includes several protocol changes and a networking change that gives us the ability to do further network upgrades:</span>
<ul>
	<li style="font-weight: 400;"><a href="https://github.com/ethereum/EIPs/blob/master/EIPS/eip-2.mediawiki"><span style="font-weight: 400;">EIP-2</span></a><span style="font-weight: 400;"> Main homestead hardfork changes</span></li>
	<li style="font-weight: 400;"><a href="https://github.com/ethereum/EIPs/blob/master/EIPS/eip-7.md"><span style="font-weight: 400;">EIP-7</span></a><span style="font-weight: 400;"> Hardfork EVM update: DELEGATECALL</span></li>
	<li style="font-weight: 400;"><a href="https://github.com/ethereum/EIPs/pull/49"><span style="font-weight: 400;">EIP-8</span></a><span style="font-weight: 400;"> devp2p forward compatibility</span></li>
</ul>
<h2><span style="font-weight: 400;">Client releases</span></h2>
<span style="font-weight: 400;">The </span><b>Go</b><span style="font-weight: 400;"> version of Ethereum (geth) for Homestead will be </span><del><span style="font-weight: 400;"><a href="https://github.com/ethereum/go-ethereum/releases/tag/v1.3.4">release 1.3.4</a></span></del><span style="font-weight: 400;"> <a href="https://github.com/ethereum/go-ethereum/releases/tag/v1.3.5">release 1.3.5</a> and includes the above mentioned changes to the protocol and network. Following the release of Homestead the Go team will also shortly come with the </span><a href="https://github.com/ethereum/go-ethereum/milestones/1.4.0"><span style="font-weight: 400;">1.4 release</span></a><span style="font-weight: 400;">, which is our big feature release and includes months of work. The reason for keeping Homestead and our feature release separate is to keep the Homestead release changes to an absolute minimum, making debugging easier when required.</span>

<span style="font-weight: 400;">The </span><b>C++</b><span style="font-weight: 400;"> version of Ethereum (eth) for Homestead will be </span><a href="https://github.com/ethereum/webthree-umbrella/releases/tag/v1.2.0"><span style="font-weight: 400;">version 1.2.0</span></a><span style="font-weight: 400;">. Apart from the protocol and network changes, it also tries to be as compatible to geth as possible. You can already use Mist and “geth attach” with an eth node running in the background. In the next weeks, we will align key management with geth. The new DELEGATECALL feature of the EVM will be usable in Solidity for library calls (this is not yet released). For further details, please see the </span><a href="https://github.com/ethereum/webthree-umbrella/releases/tag/v1.2.0"><span style="font-weight: 400;">release notes</span></a><span style="font-weight: 400;">.</span>
<h2><span style="font-weight: 400;">What's next</span></h2>
<span style="font-weight: 400;">Over the next couple of weeks, the teams will come together to pave the road for future development on both the Ethereum protocol as well as a roadmap for the clients and sub-protocols such as Swarm and Whisper. Once we have a clear picture of the work ahead we'll provide details in another blog post.</span>

<span style="font-weight: 400;">For more information about Ethereum, Homestead and how to operate one of the Ethereum clients, please head over to the </span><a href="http://ethereum-homestead.readthedocs.org/"><span style="font-weight: 400;">Homestead Documentation</span></a><span style="font-weight: 400;"> project and let us together prepare for the next phase of Ethereum.</span>

<a href="https://blog.ethereum.org/wp-content/uploads/2016/02/devcon1.png" rel="attachment wp-att-2600"><img class="alignnone size-full wp-image-2600" src="https://blog.ethereum.org/wp-content/uploads/2016/02/devcon1.png" alt="devcon1" width="1024" height="468" /></a>