---
id: 1092
title: 'Gav&#8217;s Ethereum ÐΞV Update III'
date: 2014-11-18T01:17:56+00:00
author: Gavin Wood
layout: post
guid: https://blog.ethereum.org/?p=1092
permalink: '/2014/11/18/gavs-d%ce%bev-update-iii/'
dsq_thread_id:
  - "3235631710"
tags:
  - dev
  - ethereum
  - gav
  - update
---
I'm Gavin Wood, a co-founder of Ethereum and, along with Vitalik Buterin and Jeffrey Wilcke, one of the three directors of the Eth Dev, the NFP organisation that is managing the development (under contract from Ethereum Suisse) of the Ethereum blockchain. This is a small update to let you all know what has been going on recently.

I sit here on an immaculate couch that has been zapped forward in time from the 1960s. It is in the room that will become the chillout & wind-down room of the heart of the (C++) Ethereum development operation. Surrounding me is Alex Leverington on a Bond villain's easy chair, and Aeron Buchanan stuck behind a locker that looks as though it was an original prop from <a href="https://en.wikipedia.org/wiki/M*A*S*H_(TV_series)" title="MASH" target="_blank">M\*A\*S\*H</a>. Lighting equipment from a Soviet <a href="https://en.wikipedia.org/wiki/Blade_runner" title="MASH" target="_blank">Blade Runner</a>, now forgotten except in Berlin's coolest districts where renaissance chemistry and 60s luxury breath Frankensteinesque life into it, provides an unyielding glow to the work in progress. There is still much to be done here (I feel a little like I'm on the set of <a href="https://en.wikipedia.org/wiki/Challenge_Anneka" title="Challenge Anneka" target="_blank">Challenge Anneka</a>) but it is undeniably taking shape. This is thanks mostly to our own Anneka Rice, Sarah O'Neill, who is working around the clock to get this place ready for ÐΞVcon-0, our first developer symposium. Helping her, similarly around the clock, is the inimitable Roland, a hardened international interior outfitter whose memoirs I can't wait to read.

On a personal note, I must say these last few months have been some of the busiest of my life. I spent the last couple of weeks between Switzerland and the UK, visiting Stephan, Ian and Louis. Despite the draws of the north of England, it's nice to be back in Berlin; the combination of great burgers and cocktails, beautiful surroundings and nice people makes it difficult to leave. Awesome C++ coders are welcome to take that as a hint: we're still hiring (-:

<strong>Technicals</strong>

During the last couple of weeks we've made a number of important revisions to the protocol, mostly provisions for creating light-client ÐApp nodes. There will be a directors' post in due course detailing these, but suffice it to say we are as committed as ever that the Ethereum blockchain make possible the massively multi-user decentralised applications for all sizes of devices. The seventh in our proof-of-concept series is awaiting imminent release and the final in the series, PoC-8 will be starting development shortly.

<strong>Fresh meat</strong>

As time goes on, our team moves from strength to strength. I'm pleased to announce that Dr. Sven Ehlert has joined us. He will be leading development operations; cleaning up the build process, making the build as robust as possible, assisting Caktux in our CI systems and, most importantly, helping architect a stress-testing harness in which we'll be simulating a series of extreme situations, measuring and analysing. He's also a scrum aficionado and will be helping us streamline some of our development processes as our team grows.

It is with great pleasure I can also announce that Dr. Jutta Steiner will also be working closely with us in the capacity of managing our security audit. As well as being an enthusiastic ÐApp-developer, she comes with an excellent track record of handling projects and a superb understanding of not only this cutting edge technology but also the human processes that must go on behind it.

I must also shout out to Dr. Andreas Lubbe; though a long-time member of the Berlin Ethereum community and having worked on Ethereum-related code (a notable devotee of node.js), we have recently started working much more closely together on the secure Ethereum contract documentation (SECDoc) framework and the associated natural language specification format, NatSpec. I look forward to some great collaboration.

Aside from Lefteris, who began his first official day with us today (working with Christian on Solidity, and more specifically on the SECDoc and NatSpec portions of it), we have two new developers joining us: Yann Levreau and Arkadiy Paronyan. Yann, a recent arrival in Berlin from his native France will be joined by Arkadiy who is travelling all the way from Moscow to become part of the team. Both have substantial experience in C++ and related technologies and will be helping us flesh out the developer tools and in particular pave the way to the IDE vision.

Finally, I'm happy to report that Christoph Jentzsch, though originally joining us for only 2 months (while taking time out from his doctoral studies), will be joining the project full time in the new year and continuing his much appreciated work on our tests and the general C++ health and robustness.
 
<strong>ÐΞVcon-0</strong>

As time rushes by, Sarah, Roland and their team rush even more to finish our hub. For they know that come Monday the 24th, Berlin will have some new arrivals. Developers and collaborators from around the globe will descend on 37a Waldemarstraße, Berlin 10999 for a week of getting everybody on the same page. It is DEVcon-0: ethereum's first developer symposium.

Conceived by myself and Jeff on a sleepy train from Zug to Zurich as a means of getting the Amsterdam/Go guys on the same page as the Berlin/C++ guys it has evolved into a showcase, set of seminars and workshops of all of ÐΞV, our technologies, our personnel and some of our close collaborators. It is a chance for us each to build lasting professional relationships and bond in what will become a project that may if not define, certainly form a hallmark, on our professional lifes.

Our hub will play host to around 40 people, the vast majority of which are accomplished technical minds that Jeff, myself or Vitalik has at one point or another mentioned, and for the period of a week we will be chatting, mingling and sharing our ideas, hopes and dreams for everything blockchain, decentralised and disruption related. It's going to be awesome: look out for the videos!

<strong>Ever closer</strong>

Aside from the continuing work towards starting PoC-8 and the alpha series, I'm glad to report that the Solidity project storms onwards under the stewardship of Christian: the first contracts compiled with Solidity have been delivered and tested working on the testnet, and as I write this I see another Pull Request for state mappings. Great stuff.

Alex has also been working tirelessly on our crypto code and is now beginning work on the p2p layer, the full strategy for which we'll be seeing in his address at ÐΞVcon. Marek and Marian have also been busy on the Javascript API, and I can assure any Javascript ÐApp developers that they will have a lot to look forward to in PoC-8.

<strong>Summing Up</strong>

There are also a few other developments and personnel I'd love to announce, but I fear, once again, it will have to wait until next time. Watch this space for a post-ÐΞVcon update!

Gav.