---
id: 1450
title: The Ethereum Launch Process
date: 2015-03-03T17:09:45+00:00
author: Vinay Gupta
layout: post
guid: https://blog.ethereum.org/?p=1450
permalink: /2015/03/03/ethereum-launch-process/
dsq_thread_id:
  - "3563746442"
---
<h1>I’m <a href="http://re.silience.com/">Vinay Gupta</a>, the newly minted release coordinator for Ethereum. I’ve been working with the comms team on strategy, and have now come aboard to help smooth the release process (some of the content in this blog is out of date, please see <a href="https://ethereum.gitbooks.io/frontier-guide/content/ethereum.html">this link</a> for the most up to date information on Ethereum).</h1>
I’ll be about 50/50 on comms and on release coordination. A lot of that is going to be about keeping you updated on progress: new features, new documentation, and hopefully writing about great new services you can use, so it’s in the hinterland between comms and project management. In theory, once I’m up to speed, I should be providing you with the answers to the question: “what’s going on?” But give me some time, because getting up to speed on all of this is nontrivial. We have a very large development team working with very advanced and often quite complex new technology, and keeping everybody up to date on that simultaneously is going to be tricky. To do that well, I have to actually understand what’s going on at quite a technical level first. I have a lot to wrap my head around. I was a 3D graphics programmer through the 1990s, and have a reasonably strong grounding in financial cryptography (I was, and I am not ashamed to admit it, a cypherpunk in those days). But we have a 25-30 person team working in parallel on several different aspects of Ethereum, so... patience please while I master the current state of play, so that I can communicate about what’s changing as we move forwards. It’s a lot of context to acquire, as I’m sure you all know - if there’s an occasional gaffe as I get oriented, forgive me!

I’ve just come back from Switzerland, where I got to meet a lot of the team, my “orientation week” being three days during the release planning meetings. Gav <a href="%20https://blog.Ethereum.org/2015/03/02/gavs-Ethereum-d%CE%BEv-update-v/">writes in some detail about that week here</a>, so rather than repeat Gav, read his post, and I’ll press on to tell you what was on that release white board.

There is good news, there is bad news, but above all, there is a release schedule.

There will be another blog post with much more detail about the release schedule for the first live Ethereum network shortly - likely by the end of this week, as the developer meeting that Gav mentions in his post winds up and the conclusions are communicated. That’s the post which will give you timelines you can start firing up your mining rigs to, feature lists, and so on. Until then, let me lay out roughly what the four major steps in the release process will look like and we can get into detail soon.

Let’s lay out where we are first: Ethereum is a sprawling project with many teams in many countries implementing the same protocol in several different language versions so it can be integrated into the widest possible range of other systems/ecologies, and to provide long term resilience and future-proofing. In addition to that broad effort, there are several specific applications/toolchains to help people view, build and interact with Ethereum: Mist, Mix, Alethzero and so on. Starting quite soon, and over the next few months, a series of these tools will be stood up as late alpha, beta, ready for general use and shipped. Because the network is valuable, and the network is only as secure as the software we provide, this is going to be a security-led not schedule-led process. You want it done right, we want it done right, and this is one of the most revolutionary software projects ever shipped.<strong><strong> </strong></strong>

While you’re waiting for the all singing, all dancing <a href="https://en.wikipedia.org/wiki/CERN_httpd">CERN httpd</a> + <a href="https://en.wikipedia.org/wiki/Mosaic_%28web_browser%29">NCSA Mosaic</a> combo, the “we have just launched the Future of the Internet” breakthrough system, we will be actually be releasing the code and the tools in layers. We are standing up the infrastructure for a whole new web a piece at a time: server first, plus tool chain, and then the full user experience rich client. This makes sense: a client needs something to connect to, so the server infrastructure has to come first. An internet based on this metacomputer model is going to be a very different place, and getting a good interface to that is going to present a whole new set of challenges. There’s no way to simply put all the pieces together and hope it clips into place like forming an arch by throwing bricks in the air: we need scaffolding, and precise fit. We get that by concentrating on the underlying technical aspects for a while, including mining, the underlying network and so on, and then as that is widely deployed, stable and trusted, we will be moving up the stack towards the graphical user interface via Mist in the next few months. None of these pieces stand alone, either: the network needs miners and exchanges, and it takes people time to get organized to do that work properly. The Mist client needs applications, or it’s a bare browser with nothing to connect to, and it takes people time to write those applications. Each change, each step forwards, involves a lot of conversations and support as we get people set up with the new software and help them get their projects off the ground: the whole thing together is an ecology. Each piece needs its own time, its own attention. We have to do this in phases for all of these reasons, and more.<strong><strong> </strong></strong>

It took bitcoin, a much less complex project, several years to cover that terrain: we have a larger team, but a more complex project. On the other hand, if you’re following the github repositories, you can see how much progress is being made, week by week, day by day, so... verify for yourself where we are.

So, now we’ve all got on the same page on real world software engineering, let’s actually look at phases of this release process!

<b>Release Step One: Frontier</b>

Frontier takes a model familiar to Bitcoiners, and stands it up for our initial release. Frontier is the Ethereum network in its barest form: an interface to mine Ether, and a way to upload and execute contracts. The main use of Frontier on the launch trajectory is to get mining operations and Ether exchanges running, so the community can get their mining rigs started, and to start to establish a “live” environment where people can test DApps and acquire Ether to upload their own software into Ethereum.

This is “no user interface to speak of” command line country, and you will be expected to be quite expert in the whole Ethereum world model, as well as to have substantial mastery of the tools at your disposal.

However, this is not a test net: this is a <i>frontier</i> release. If you are equipped, come along! Do not <a href="https://www.google.co.uk/search?q=you+have+died+of+dysentery%22&amp;source=lnms&amp;tbm=isch">die of dysentery</a> on the way.

Frontier showcases three areas of real utility:
<ul>
	<li><del>you can mine real Ether, at 10% of the normal Ether issuance rate, 0.59 Ether per block reward, which can be spent to run programs or exchange for other things, as normal - this real Ether</del> (This was not the case at launch - Frontier block reward is 5 Ether per block, and will remain that amount until Casper).</li>
	<li>you can exchange Ether for Bitcoin, or with other users, if you need Ether to run code etc.</li>
	<li>if you already bought Ether during the crowd sale, and you are fully conversant with the frontier environment, you can use it on the frontier network.</li>
	<li>we do not recommend this, but have a very substantial security-and-recovery process in place to make it safer - see below<strong><strong> </strong></strong></li>
</ul>
We will migrate from Frontier to Homestead once Frontier is fully stable in the eyes of the core devs and the auditors:
<ul>
	<li>when we are ready to move to Homestead, the release after Frontier, the Frontier network will be shut down; Ether values in wallets will be transferred, but state in contracts is will likely be erased (more information to follow on this in later blog posts)</li>
	<li>switchover to  the new network will be enforced by “TheBomb”</li>
</ul>
This is very early release software: feature complete within these boundaries, but with a substantial risk of unexpected behaviours unseen in either the test net or the security review. And it’s not just us that will be putting new code into production: contracts, exchanges, miners, everybody else in the ecosystem will be shipping new services. Any one of those components getting seriously screwed up could impact a lot of users, and we want to shake bugs out of the ecosystem as a whole, not simply our own infrastructure: we are all in this together.

However, to help you safeguard your Ether, we have the following mechanisms planned (more details from the developers will follow soon as the security model is finalised):
<ul>
	<li>if you do not perform any transactions, we guarantee 100% your Ether will not be touched and will be waiting for you once we move beyond Frontier</li>
	<li>if you perform transactions, we guarantee 100% that any Ether you did not spend will will be available to you once we move beyond Frontier not be touched</li>
	<li>Ether you spend will not fall through cracks into other people’s pockets or vanish without a trace: in the unlikely event that this happens, you have 24 hours to inform us, and we will freeze the network, return to the last good state, and start again with the bug patched</li>
	<li>yes, this implies a real risk of network instability: everything possible has been done to prevent this, but this is a brand new aeroplane - <i>take your parachute!</i></li>
	<li>we will periodically checkpoint the network to show that neither user report nor automated testing has reported any problems. We expect the checkpoints will be around once daily, with a mean of around 12 hours of latency</li>
	<li>exchanges etc. will be strongly encouraged to wait for checkpoints to be validated before sending out payments in fiat or bitcoin. Ethereum will provide explicit support to aid exchanges in determining what Ether transactions have fully cleared</li>
</ul>
Over the course of the next few weeks several pieces of software have to be integrated to maintain this basket of security features so we can allow genesis block Ether on to this platform without unacceptable risks. Building that infrastructure is a new process, and while it looks like a safe, sane and conservative schedule, there is always a chance of a delay as the unknown unknown is discovered either by us, the bug bounty hunters or by the security auditors. There will be a post shortly which goes through this release plan in real technical detail, and I’ll have a lot of direct input from the devs on that post, so for now take this with a pinch of salt and we will have hard details and expected dates as soon as possible.<strong><strong> </strong></strong>

<b>Release Step Two: Homestead</b>

Homestead is where we move after Frontier. We expect the following three major changes.
<ul>
	<li><del>Ether mining will be at 100% rather than 10% of the usual reward rate </del>(Frontier/Homestead block reward will remain 5 Ether)</li>
	<li>checkpointing and manual network halts should never be necessary, although it is likely that checkpointing will continue if there is a general demand for it</li>
	<li>we will remove the severe risk warning from putting your Ether on the network, although we will not consider the software to be out of beta until Metropolis</li>
</ul>
Still command line, so much the same feature set as Frontier, but this one we tell you is ready to go, within the relevant parameters.

How long will there be between Frontier and Homestead? Depends entirely on how Frontier performs: best case is not less than a month. We will have a pretty good idea of whether things are going smoothly or not from network review, so we will keep you in the loop through this process.

<b>Release Step Three: Metropolis</b>

Metropolis is when we finally officially release a relatively full-featured user interface for non-technical users of Ethereum, and throw the doors open: Mist launches, and we expect this launch to include a DApp store and several anchor tenant projects with full-featured, well-designed programs to showcase the full power of the network. This is what we are all waiting for, and working towards.

In practice, I suspect there will be at least one, and probably two as-yet-unnamed steps between Homestead and Metropolis: I’m open to suggestions for names (write to vinay[at]ethdev.com). Features will be sensible checkpoints on the way: specific feature sets inside of Mist would be my guess, but I’m still getting my head around that, so I expect we will cross those bridges after Homestead is stood up.

<b>Release Step Four: Serenity</b>

There’s just one thing left to discuss: mining. Proof of Work implies the inefficient conversion of electricity into heat, Ether and network stability, and we would quite like to not warm the atmosphere with our software more than is absolutely necessary. Short of buying carbon offsets for every unit of Ether mined (is that such a bad idea?), we need an algorithmic fix: the infamous Proof of Stake.<strong><strong> </strong></strong>

Switching the network from Proof of Work to Proof of Stake is going to require a substantial switch, a transition process potentially much like the one between Frontier and Homestead. Similar rollback measures may be required, although in all probability more sophisticated mechanisms will be deployed (e.g. running both mechanisms together, with Proof of Work dominant, and flagging any cases where Proof of Stake gives a different output.)

This seems a long way out, but it’s not as far away as all that: the work is ongoing.

Proof of Work is a brutal waste of computing power - like democracy*, the worst system except all the others (*voluntarism etc. have yet to be tried at scale). Freed from that constraint, the network should be faster, more efficient, easier for newcomers to get into, and more resistant to cartelization of mining capacity etc. This is probably going to be almost as big a step forwards as putting smart contracts into a block chain in the first place, by the time all is said and done. It is a ways out. It will be worth it.<strong><strong> </strong></strong>

<b>Timelines</b>

As you have seen since the Ether Sale, progress has been rapid and stable. Code on the critical path is getting written, teams are effective and efficient, and over-all the organization is getting things done. Reinventing the digital age is not easy, but somebody has to do it. Right now that is us.

We anticipate roughly one major announcement a month for the next few months, and then a delay while Metropolis is prepared. There will also be DEVcon One, an opportunity to come, learn the practical business of building and shipping DApps, meet fellow developers, potential investors, and understand the likely shape of things to come.

We will give you information about each release in more detail as each release approaches, but I want to give you the big overview of how this works and where we are going, fill in some of the gaps, highlight what is changing, both technically and in our communications and business partnership, and present you with an overview of what the summer is going to be like as we move down the path towards Serenity, another world changing technology.

I’m very glad to be part of this process. I’m a little at sea right now trying to wrap my head around the sheer scope of the project, and I’m hoping to actually visit a lot of the development teams over the summer to get the stories and put faces to names. This is a big, diverse project and, beyond the project itself, the launch of a new sociotechnical ecosystem. We are, after all, a platform effort: what’s really going to turn this into magic is you, and the things you build on top of the tools we’re all working so hard to ship. We are making tools for tool-makers.

Vinay signing off for now. More news soon!

&nbsp;