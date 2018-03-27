---
id: 1022
title: 'Gav&#8217;s Ethereum ÐΞV Update II'
date: 2014-11-01T15:58:57+00:00
author: Gavin Wood
layout: post
guid: https://blog.ethereum.org/?p=1022
permalink: '/2014/11/01/gavs-ethereum-d%ce%bev-update-ii/'
dsq_thread_id:
  - "3179889506"
---
Well... what a busy two weeks. I thought it about time to make another update for any of you who might be interested in how we're doing. If you don't already know, I'm Gavin, a founder of Ethereum and one of the three directors (alongside Vitalik and Jeffrey) of Ethereum ÐΞV, the development entity building Ethereum and all the associated technology.

After doing some recruitment on behalf of DEV in Bucharest with the help of Mihai Alisie and the lovely Roxanna Sureanu I spent the last week at my home (and coincidentally, the Ethereum HQ) in Zug, Switzerland. During this time I was able to get going on the first prototype of Whisper, our secure identity-based communications protocol, finishing with a small IRC-like ÐApp demonstrating how easy it is to use. For those interested, there is more information on Whisper in the <a href="https://github.com/ethereum/cpp-ethereum/wiki/Whisper-Overview" target="_blank">Ethereum Github wiki</a> and a nice little <a href="https://twitter.com/gavofyork/status/526457129352581120" target="_new">screenshot</a> on my twitter feed. In addition to this I've been helping finalise the soon-to-be-announced PoC-7 specification and working towards a PoC-8 (final). Finally, during our brief time together in Zug, Jeffrey, Vitalik and I drafted our strategy concerning identity and key management; this will be developed further during the coming weeks. 

### ÐΞVHUB Berlin

In Berlin, Sarah has been super-busy with the builders getting the hub ready. Here's a <a href="http://opensecrecy.com/berlin1.jpeg">couple</a> <a href="http://opensecrecy.com/berlin2.jpeg">pictures</a> of the work in progress that is the Berlin hub. It might not look like much yet, but we're on target to be moved in by mid-November. I'm particularly happy with Sarah's efforts to find a genuine 70's barrista espresso machine (-:

I'm excited to announce that Christian Vömel is joining the team in Berlin to be the Office Manager of ÐΞVHUB Berlin. Christian has many years experience including having worked in an international environment and has even taught office management! He'll be taking some of the load from our frankly much-overworked company secretary Aeron Buchanan.

### The Team Grows

We've finalised a number of new hires over the past couple of weeks: Network engineer Lefteris Karapetsas will be joining the Berlin team imminently. Having considerable experience with state-of-the-art network traffic analysis and deep-packet inspection systems, he'll be helping audit our network protocols, however (like much of our team) truly multidisciplinary, he'll also be working on NatSpec, the code name for our Natural Language Formal Contract Specification system, a cornerstone of our transaction security model.

I'm happy to announce that Ian Meikle, the accomplished videographer who co-authored the impressive "Koyaanis-glitchy" Ethereum <a href="http://vimeo.com/92338385" target="_new">brand video</a> has been moved to ÐΞV to help with the communications team. He who shall be known only as Texture has also joined the comms side with Stephan to help with the strategy stateside and coordinate the worldwide meetup and hackathon network. Great to see such a capable and passionate designer on the team; I know he has a good few ideas for ÐApps!

Two more hires under Stephan in the comms team include <a href="http://dappsforbeginners.wordpress.com/" target="_new">Ken Kappler</a>, handling the developer education direction, hackathons, ethereum curriculum and university partnerships. George Hallam has also been employed to evangelize ethereum to startups and partners, boost the reach of our formal network and generally help Stephan in the quest of having everybody know what Ethereum is and how it can help them.

Jeff's team has also been expanded recently too; he'll be telling you about his developments in an imminent post.

### Further Developments

Aside from the aforementioned progress with Whisper and PoC-7, Christoph has been continuing his great work with the tests repository. Christian has been making great progress with the Solidity language having recently placed the first Solidity-compiled program onto the testnet block chain only a few days ago.

Marek has studiously been moving C++ over to a JSON-RPC and Javascript front-end fundamentally unified and bound to the Go client. Alex meanwhile has been grappling with the C++ crypto back-end and has done a great job of reducing bloat and extraneous dependencies.

Of late, the comms team has some good news brewing, in particular, it is in contact with some world-class education establishments regarding the possibility of eduction partnership and the formation of a network of chapters both in the UK and internationally. Watch this space (-:

Finally, our Polish partners at IMAPP (Paweł and Artur) have completed their first implementation of the JIT-compiled LLVM-based EVM implementation. They are reporting an average of 30x speedup (as high as 100x!) for non-external EVM instructions over the already best-in-class basic C++-based EVM implementation. Brilliant work and we're looking forward to more improvements and optimisations yet.

### And the rest...

So much to come; there are a couple of announcements (including a slew of imminent hires) I'd love to make but they need to be finalised before I can write about them here. Look out for the next update!

Gav (gavin.nospam.wood@ethdev.com).