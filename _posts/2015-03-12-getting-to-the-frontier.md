---
id: 1465
title: Getting to the Frontier
date: 2015-03-12T18:36:27+00:00
author: Vinay Gupta
layout: post
guid: https://blog.ethereum.org/?p=1465
permalink: /2015/03/12/getting-to-the-frontier/
dsq_thread_id:
  - "3589629011"
---
<a href="https://blog.ethereum.org/wp-content/uploads/2015/03/Frontier_BLOG-Image_2.jpg"><img class="alignleft size-full wp-image-1474" src="https://blog.ethereum.org/wp-content/uploads/2015/03/Frontier_BLOG-Image_2.jpg" alt="Frontier_BLOG-Image_2" width="1920" height="1152" /></a>

So first some background. We’ve already covered a little about what to expect from Frontier in the <a href="https://blog.ethereum.org/2015/03/03/ethereum-launch-process/">Ethereum Launch Process</a> blog post and Gav has given us a <a href="https://blog.ethereum.org/2015/03/02/gavs-ethereum-d%CE%BEv-update-v/">comprehensive overview of the state of the development effort</a>. You can also read about some of the ongoing security work, <a href="https://bounty.ethdev.com/">including the bug bounty system</a>. The bug bounty program has found my favorite bug so far: <a href="https://jonasnick.github.io/">Jonas Nick</a>'s discovery that <a href="https://www.reddit.com/r/ethereum/comments/2xujcn/ethereum_bug_sending_negative_value_transactions/">you could send a negative payment to somebody which moves value from their account to yours!</a>

As bug bounty finds go, an absolute gem. Well done!

Gav has a new blog post describing a little more about what the thorny end of the development process looks like: <a href="https://blog.ethereum.org/2015/03/05/ethereum-development-process/">creating a precise specification for Ethereum by keeping three different implementations in sync</a>, rather than having a canonical implementation which implicitly defines the protocol spec and which other versions then become bug-for-bug compatible with. This is a pretty tough-minded and thorough approach to putting together a system, and it’s worth reading to get a flavor of how the engineering work is going.

By now you should have a pretty good sense of what the development process looks like, and the disparate threads of work which are being woven together into the release. So let’s talk about what actually has to get done between now and Frontier.

There are <b>four specific challenges</b> for us to meet on Frontier.
<ul>
	<li>Security</li>
	<li>Network Stability</li>
	<li>Mining</li>
	<li>Exchange</li>
</ul>
Two of those, Mining and Exchange are partnership efforts: people have to take our software and run it, in concert with their own tooling in some cases, to provide services as part of the ecosystem. Remember, we’re launching an ecosystem, not just a product: everything is part of that ecology. Each one of these aspects of the release process has to work, both on its own, and with the others for the ecosystem to actually get a proper test under load.

In addition:
<ul>
	<li>All software has to be reasonably easy to install, mining included</li>
	<li>We need to support partners, particularly exchanges, to provide services</li>
	<li>We need to build our checkpointing and blockchain integrity testing services</li>
	<li>All within the context of the Frontier environment in which we make strong guarantees about network integrity (more on this later) at the cost of the risk of network rollbacks if/when we discover unexpected network behaviors or other problems.</li>
</ul>
Reasonably good progress is being made on all fronts. Rather than giving you a release date, and then risking that under pressure to meet that date, something will slip through the gaps that another day of work would have caught, we are going to tick off items in the release process as we go. This will let you get have a sense of how far out from the launch we are on any given day, without us basically making a guess at how long it will all take, publishing our guess, and then hoping we can make reality to the calendar: as we said before, this is a security-driven not a schedule-driven process. We can make that process fairly transparent to you, and provide summary information so you don’t have to watch the repositories - but we would rather bring you into the process so you understand what is happening and how the work is going than give you an aggregate date and keep you guessing as to the actual state of the work. We feel this approach is more transparent, better suited to a crowdfunded development process, and overall simply more realistic.

If you would like to see the current state of play, Jeffrey Wilcke (<a href="https://blog.ethereum.org/2015/01/06/jeffs-ethereum-dev-update-2/">lead on the Go client</a> <a href="https://blog.ethereum.org/2015/01/06/jeffs-ethereum-dev-update-2/"><img class="alignleft  wp-image-1467" src="https://blog.ethereum.org/wp-content/uploads/2015/03/Jeffrey.jpg" alt="Jeffrey" width="139" height="139" /></a>and the Amsterdam hub) has a <strong><a href="https://github.com/ethereum/go-ethereum/milestones/Frontier">Go client issue tracker for Frontier</a>.</strong>

This <a href="https://github.com/ethereum/go-ethereum/milestones/Frontier">issue list</a> is the best reference for the Frontier release: right now, 10 issues closed, 26 open. New issues will get raised, and of course issue count is only a very loose proxy for “ship time” but the work done is the best indicator, so look there if you would like to prognosticate.

<a href="https://github.com/ethereum/go-ethereum/milestones/Frontier"><img class="alignleft size-full wp-image-1469" src="https://blog.ethereum.org/wp-content/uploads/2015/03/screen-shot-for-downloads.png" alt="screen shot for downloads" width="997" height="400" /></a>

Please note: this is not to say that we are running on “<a href="https://developer.valvesoftware.com/wiki/Valve_Time%20">Valve Time</a>” (even though Valve Time does produce some spectacular products!) We have open development processes, you can see the code being written, download the latest branches, participate in testing. It’s not all hidden behind a company PR wall. But we are running on “releasing innovative software with huge associated security processes” time, and schedule estimation for things which have never been done before is not going to help anybody: part of the benefit of the way Ethereum was funded over conventional funding models is that we can do what is right in the medium- and long-term, rather than being under constant market pressure for next quarter’s numbers.

We will say this: there will be at least two weeks notice before anything goes live. You’ll get plenty of news from blog posts, you’ll see the issue count go down, and we’ll start saying positive things about security audit feedback and so on. We will keep you in the loop!

What about the feature list? We’ve largely gone over this in the Launch Processes post, but to reiterate Frontier will...
<ul>
	<li>Be command-line client only</li>
	<li>Mine at 10% of the normal rate, but this will be real Ether</li>
	<li>Include the full blockchain feature set including smart contracts and logs, although everything except account balances will be erased when Homestead is launched</li>
</ul>
<ul>
	<li>We are working with exchanges to make ETH convertible during Frontier</li>
	<li>For security, the block chain will be checkpointed manually every 24 hours, and any reported anomalies will be investigated</li>
	<li>Official exchanges will use this checkpointing service to protect traded assets from potential blockchain rollbacks</li>
</ul>
<ul>
	<li>In particularly severe cases of failure, the Foundation may stop checkpointing Frontier completely and release a client upgrade</li>
</ul>
In short, we’re doing everything possible to make Frontier a safe place to test Ethereum with real value, but we are also strongly discouraging people from using Ether on the Frontier network which they are unwilling to lose.

Frontier is intended largely for people who are writing and testing tooling for mining and exchanges, and perhaps a few of the more hardy dApp developers. It is not a general release that we expect ordinary users to interact with at all, although you might download a client and mine a little Ether just because you can. The show really starts at Homestead, and we’ll have more news about Homestead’s features later

More news as I have it, and keep watching the skies!