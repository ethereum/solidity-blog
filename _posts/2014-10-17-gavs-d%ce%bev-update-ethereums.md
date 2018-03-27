---
id: 979
title: 'Gav&#8217;s ÐΞV Update I: Where Ethereum&#8217;s at'
date: 2014-10-17T13:58:46+00:00
author: Gavin Wood
layout: post
guid: https://blog.ethereum.org/?p=979
permalink: '/2014/10/17/gavs-d%ce%bev-update-ethereums/'
dsq_thread_id:
  - "3126493192"
---
<strong>Who are you?</strong>

I'm Gav - together with Jeffrey Wilcke and Vitalik Buterin, I'm one third of the ultimate leadership of Ethereum ÐΞV. ÐΞV is a UK software firm that is under a non-profit-making agreement with the Ethereum Foundation to create version 1.0 of the Web Three software stack. We three directors---who are ultimately responsible that the software is built and works---are the same three developers who designed and implemented the first working versions of the Ethereum clients.

ÐΞV is geographically split between London (where our comms operation is based) and Berlin (which hosts the main hub of ÐΞV). Though I'm based in Zug, Switzerland (being an Ethereum employee), I have been involved most recently in putting together the Berlin side of things.

Since its inception in summer, we have been working to set up the technical side of the project, under which we include our communications, education and adoption team lead by Stephan Tual and helped by Mathias Grønnebæk for the organisation of operations.

<strong>A Berlin Who's Who</strong>

Aeron Buchanan, though originally brought on as a mathematical modeller, has been very successful in coordinating Berlin's various operations including helping set up the arduous process of getting a bank account, recruitment, financial juggling to get people paid, technical interviews and other tedious administration tasks; more recently he has also been helping sort out the UK side of things, too.

I must acknowledge Brian Fabian who helped connect us while in Berlin and made it possible for us to have a legal structure in place quickly. At present, the operation in Berlin is directed by our major PyEthereum contributor, Heiko Hees, with Aeron being the essential point of control for all operations. Over time, we expect Aeron to get back to modelling and to find a suitable candidate for the day-to-day management of the hub.

During our time in Berlin we've been very active in hiring (which as a process is considerably more arduous than you might think): Alex Leverington was our first hire and he flew to Berlin all the way from Texas to join the team. Alex has been engaged helping out with the Mac builds and making volunteer contributions since early in the year, so it's great that he wanted to step forward into a permanent role. Now Alex has been working on some of the internals of the C++ client (specifically the client multiplexing, allowing multiple Web Three applications to coexist on the same physical machine).

Over the past few months we've recruited a few more people: One of our first hires following Alex is Christian, who holds a PhD in Multiobjective Optimization and Language Equations and is now engaged in prototyping and implementing the new domain-specific contract-authoring language that <a href="https://github.com/ethereum/wiki/wiki/Solidity,-Docs-and-ABI" title="Solidity" target="_blank">I proposed a while ago</a>, Solidity. At around the same time, we also hired Christoph, currently finishing his PhD in physics and who utterly loves writing unit tests. It didn't take me long to realise he'd be great for sorting out our clients' interoperability issues. He has been leading our recent surge in getting the protocol in alignment for all clients through a comprehensive code-covering set of unit tests for the virtual machine operation.

Our newest recruit, Marek, journeying to Berlin from Poland, was an early Ethereum volunteer and enjoyed making contracts on some early C++ client prototypes. Coming from a Web-technology background (though being perfectly competent in C++), he has now started working on our C++/Javascript API, aiming towards full node.js integration to facilitate backend integration with existing web sites. Working alongside Marek is Marian, the feathers in whose cap include much of the technical prowess behind the highly successful ether sale. He has been developing out some of <a href="http://poc-6.ethdev.com" title="PoC-6 Blockchain Explorer" target="_blank">the first Web apps</a> to use Ethereum as its backend.

I look forward to welcoming three more hires in the coming weeks, including some personnel with rather impressive and uniquely relevant backgrounds. More news on that next time.

<strong>And More...</strong>

Back in London, we've hired design outfit Proof-of-Work, headed by Louis Chang, to put together our new website and brand. We're ecstatic with how things are coming along there and look forward to unveiling it soon. Once this is in place we'll have a much clearer way of getting our updates and information out regarding what's happening at ÐΞV.

Externally to ÐΞV but supported by it are a number of other individuals and projects: I am very grateful to Tim, who continues to consult on our efforts at an ASIC-resistant proof-of-work algorithm, also implementing it in C++. Similarly, Caktux an early volunteer and maintainer of the Ncurses-based C++ Ethereum front-end neth has been invaluble (alongside Joris and Nick Savers) in getting a <a href="http://build.ethdev.com/console" title="Buildbot Console" target="_blank">continuous integration</a> system up and running. We are pleased to support both of them in their endeavours to make this project a success.

Furthermore the guys at <a href="http://imapp.pl/en/" title="IMAPP" target="_blank">IMAPP</a>, a software firm in Warsaw specialising in advanced languages and compilers deserve a great nod for their on-going efforts at using their considerable expertise in implementing a just-in-time (JIT) compiled version of the Ethereum virtual machine and making computationally-complex contracts a reasonably affordable possibility.

Finally, I must thank the EthereumJ (Java client) volunteer developers Roman and Nick, both of whom have visited us in our prototype hub here in Berlin, and who work tirelessly to find different and innovative new ways of interpreting the formal protocol specification.

<strong>The California Connection</strong>

Over in Silicon Valley, we have made two hires, Joseph and Martin; Joseph will be leading the efforts there and concentrating on developing some of our core Ðapps that will help demonstrate the potential of Ethereum. Martin is leading the effort to create a pure Javascript implementation of Ethereum, a lofty goal, and thus all the more impressive that the project now has a core that is compatible with PoC-6.

We are also looking forward to working with the Agreemint Foundation (ie. Mintchalk), with their effort to create an online contract development environment, to provide a simple and highly accessible interface for the beginner and intermediate level users to learn about contract development and create and deploy Web Three Ðapps.

In the future we hope to expand our operations there, particularly over January and February when Vitalik and I will be staying there, we in particular look forward to spending some time discussing the future of data sharing and online publication with Juan of IPFS and are optimistic about the possibility of finding some synergy between our projects.

<strong>On Go-ing Development</strong>

Though I'm sure Jeff will make his own post on the goings-on over at his Golang-orientated end, I will say that on a personal note I'm very happy that Alex (aka <em>avsa</em>) has joined us on a permanent basis. Alex is well known on the Ethereum forums and his mockups of what Web Three could look like were simply incredible in insight, technical knowledge and polish. As an accomplished UI &amp; UX engineer, he'll be joining Jeff in taking Mist, the Web Three browser, forward and making it into what I am sure will simultaneously be the most revolutionary and pleasing to use piece of new software in a very long time.

<strong>So what's happening in Berlin then?</strong>

When we arrived at first we needed somewhere to be based out of: thanks to Brian, we were invited to the Rainmaking Loft, an excellent space for tech startups that need somewhere to spread their rug prior to world domination. Since August we've had a nice big desk there for our developers to work alongside our inimitable location scout, hub outfitter, project manager and interior designer rolled into one; Sarah.

Sarah has worked tirelessly in finding our perfect location, our perfect contractors and our perfect fixtures and fittings and making it actually work. Right now as I write this at 4am EEST, she's probably up on eBay looking for a decent deal for office chairs or costing a well-placed dry wall. And what a job she has done thus far. We will be based in probably the most perfect place we could hope for. Walking distance to two U-bahn stations, we're located on a quiet street adjacent to Oranienstraße and a central point of Kreuzberg. We're a short cycle ride from the centre of Berlin's mass and, in the opposite direction, from the beautiful canal and Neukölln. We have some lovely quiet bars and cafés on our sexy little street and the bustling new-tech area that is Kreuzberg at the end of it.

Our new hub, designed and outfitted by her will be a 250m² cross of office, homely relaxation environment and (self-service) café---a new (and German-building-law-friendly) twist on the notion of the holon. We'll be able to host meetups and events, have a great area for working and have ample collaboration space for any other Ethereum-aligned operations that would prefer not to pay coffee-tax for their power &amp; wifi.

Not to be forgotten, helping Aeron and me with administration, procurements and organisation, not to mention general German-speaking tasks, Lisa has been invaluable during this period.

<strong>And what have we been doing?</strong>

Since beginning, ÐΞV's time has inevitably been wast^H^H^H^H spent wisely in bureaucracy, administration and red tape. It is impressive how much of a pain doing business in a perfectly well developed nation like Germany can be. Slowly (and thanks in no small part to Aeron) this tediousness is starting to let up. When not engaged in such matters, we've been pressing to get our most recent proof-of-concept releases out, PoC-5 and PoC-6. PoC-5 brought with it a number of important alterations to the Ethereum virtual machine and the core protocol. PoC-6 brought a 4-second block time (this is just for stress-testing; for the mainet we're aiming for a 10 second block time) and wonderfully fast parallel block-chain downloading. Furthermore we've been talking with various potential technology partners concerning the future of Swarm, our data distribution system, including with our good friend Juan Batiz-Benet (Vitalik &amp; I got to know him while staying at his house in Silicon Valley for a week back in March).

Speaking at a few meetings and conferences has taken time also. In my case, the keynote speaker at both Inside Bitcoin and Latin America's popular tech-fest Campus Party was an honour, as was the invitation to address the main hall at the wonderful University San Francisco of Quito. I hesitate to imagine the number of such engagements Vitalik has done during the same time period.

In addition to his impressive public speaking schedule, Vitalik has been putting in considerable efforts into research on potential consensus algorithms. Together with Vlad, a number of potential approaches have been mooted over the past few weeks. Ultimately, we decided to follow the advice of some in our community, like Nick Szabo, who have urged us to focus on getting a working product off the ground and not try to make every last detail perfect before launching. In that regard, we've decided to move many of our more ambitious changes, including native extensions, auto-triggering events and proof of stake, into a planned future upgrade to happen around mid-to-late 2015.

However, during a two-week visit to London Vitalik made major progress working with Vlad on developing stable proof-of-stake consensus algorithms, and we have a few models that we think are likely to work and solve all of the problems inherent in current approaches. The two have also begun more thoroughly laying the plans for our upcoming upgrades in scalability.

More recently, I have been hard at work rewriting much of the networking code and altering the network protocol to truly split off the peer-to-peer portion of the code to make an abstract layer for all peer-to-peer applications, including those external to the Web Three project that wish to piggyback on the Ethereum peer network. I've also been getting PoC-7 up to scratch and more reliable, as well as upgrading my team's development processes which predictably were becoming a little too informal for an increasingly large team. We'll be moving towards a peer-reviewed (rather than Gav-reviewed) commit review process, we have a much more curated GitHub issue tracker, alongside an increasingly scrum-oriented project management framework (a switch to Pivotal Tracker is underway - everything public, of course). Most recently I've been working on the <a href="https://github.com/ethereum/wiki/wiki/Whisper-Overview" title="Whisper Overview" target="_blank">Whisper project</a>, designing, developing, chewing things over and prototyping.

Finally, we've also been making inroads into some well-known and some other not-so-well-known firms that can help us make our final core software as safe and secure as humanly possible. I'm sorry I can't go into anything more specific now, but rest assured, this is one of our priorities.

So there you have it. What's been happening.

<strong>And what's going to happen?</strong>

Aside from the continuing hiring process and our inroads into setting up a solid security audit, we will very soon be instituting a more informal manner for volunteers and contributors to be supported by the project. In the coming days we will be launching a number of ÐΞV schemes to make it possible for dedicated and productive members of the Ethereum and Web Three community to apply for bursaries and expenses for visiting us at one of our hub locations. Watch this space.

In terms of coding, ÐΞV, at present, has one mission: the completion of version 1.0 of the Ethereum client software which will enable the release of the genesis block. This will be done as soon as possible, though we will release the genesis block only when we (and many others in the security world) are happy that it is safe to do so: we are presently aiming to have it out sometime during this winter (i.e. between December 21st and March 21st). This will include at least a basic contract development environment (the focus of the work here in Berlin under myself), an advanced client based around Google's Chromium browser technology and several core Ðapps (the focus of the work under Jeff), and various command-line tools.

In specifics, after we have PoC-7 out, we'll be making at most one more proof-of-concept release before freezing the protocol and moving into our alpha release series. The first alpha will signal the end of our core refactoring &amp; optimisation process and the beginning of our security audit; we aim to have this under way within the next 4-6 weeks. The security audit will involve a number of people and firms, both internal and external, both hired and incentivised, analysing the design and implementations looking for flaws, bugs and potential attack vectors. Once all parties involved have signed off on all aspects of the system will we move to organise a coordinated release of the final block chain. We expect the auditing process to take 2-3 months, with another couple of weeks to coordinate the final release.

During this process we will be developing out the other parts of the project, including the Whisper messaging protocol, the contract development environment and Solidity, the Ethereum browser, Mist and the core Ðapps, all in readiness for the genesis block release.

We will take a very much fluid attitude to software development &amp; release and incrementally roll out updates and improvements to our core suite of software over time. We don't want to keep you waiting with the release of the blockchain and so that is our development priority. So you may be assured, it will be released just as soon as it is ready.

So hold on to your hats! You'll be coding contracts and hacking society into new forms before you know it.

Gav.