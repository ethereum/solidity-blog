---
id: 1438
title: 'Gav&#8217;s Ethereum ÐΞV Update V'
date: 2015-03-02T17:00:35+00:00
author: Gavin Wood
layout: post
guid: https://blog.ethereum.org/?p=1438
permalink: '/2015/03/02/gavs-ethereum-d%ce%bev-update-v/'
dsq_thread_id:
  - "3560819746"
tags:
  - dev
  - ethereum
  - gav
  - update
---
I was woken by Vitalik's call at 5:55 this morning; pitch black outside, nighttime was still upon us. Nonetheless, it was time to leave and this week had best start on the right foot.

The 25-minute walk in darkness from the Zug-based headquarters to the train station was wet. Streetlights reflecting off the puddles on the clean Swiss streets provided a picturesque, if quiet, march into town. I couldn't help but think the rain running down my face was a very liquid reminder of the impending seasonal change, and then, on consideration, how fast the last nine months had gone.

<strong>Solid Foundations</strong>

The last week was spent in Zug by the Ethereum foundation board and ÐΞV leadership: Vitalik, Mihai and Taylor who officially form the founation's board, Anthony and Joseph as the other official advisors and Aeron & Jutta as the ÐΞV executive joined by Jeff and myself wearing multiple hats of ÐΞV and advisory). The chief outcome of this was the dissemination of Vitalik's superb plan to reform the foundation and turn it into a professional entity. The board will be recruited from accomplished professionals with minimal conflicts of interest; the present set of "founders" officially retired from those positions and a professional executive recruited, the latter process lead by Joseph. Anthony will take a greater ambassadorial role for Ethereum in China and North America. Conversely, ÐΞV will function much more as a department of the Foundation's executive rather than a largely independent entity. Finally, I presented the release strategy to the others; an event after which I've never seen quite so many photos taken of a whiteboard. Needless to say, all was well received by the board and advisors. More information will be coming soon.

As I write this, I'm sitting on a crowded early commuter train, Vinay Gupta in tow, who recently took on a much more substantive role this week as release coordinator. He'll be helping with release strategy and to keep you informed of our release process. This week, which might rather dramatically be described as 'pivotal' in the release process, will see Jeff, Vitalk and me sit around a table and develop all the PoC-9 changes, related unit tests, and integrations in three days, joined by our indomitable Master of Testing, Christoph. The outcome of this week will inform our announcement which will come later this week outlining in clear terms what we will be releasing and when.

I'm sorry it has been so long without an update. The last 2 months has been somewhat busy, choked up with travel and meetings, with the remaining time soaked up by coding, team-leading and management. The team is now substantially formed; the formal security audit started four weeks ago; the bounty programme is running smoothly. The latter processes are the exceedingly capable hands of Jutta and Gustav. Aeron, meanwhile will be stepping down as the ÐΞV head of finance and operations and assuming the role he was initially brought aboard for, system modelling. We'll hopefully be able to announce his successors next week (yes, that was plural; he has been doing the jobs of 2.5 people over the last few months).

We are also in the process of forming partnerships with third parties in the industry; George, Jutta and myself managing this process; I'm happy to announce that at least three exchanges will be supporting Ether from day one on their trading platforms (details of which we'll annouce soon), with more exchanges to follow. Marek and Alex are providing technical supprt there with Marek going so far as to make a substantial reference exchange implementation.

I also finished the first draft of ICAP, the Ethereum Inter-exchange Client Address Protocol, an IBAN-compatible system for referencing and transacting to client accounts aimed to streamline the process of transfering funds, worry-free between exchanges and, ultimately, make KYC and AML pains a thing of the past. The IBAN compatibility may even provide possibility of easy integration with existing banking infrastructure in some future.

<strong>Developments</strong>

Proof-of-Concept releases VII and VIII were released. NatSpec, "natural language specification format" and the basis of our transaction security was prototyped and integrated. Under Marek's watch, now helped by Fabian, ethereum.js is truly coming of age with a near source-level compatibility with Solidity on contract interaction and support for the typed ABI with calling and events, the latter providing hassle-free state-change reporting. Mix, our IDE, underwent its first release and after some teethng issues is getting good use thanks to the excellent work done by Arkadiy and Yann. Solidity had numerous features added and is swiftly approaching 1.0 status with Christian, Lefteris and Liana to thank. Marian's work goes ever forward on the network monitoring system while Sven and Heiko have been working diligently on the stress testing infrastructure which analyses and tests the peer network formation and performance. They'll soon be joined by Alex and Lefteris to accellerate this programme.

So one of the major things that needed sorting for the next release is the proof-of-work algorithm that we'll use. This had a number of requirements, two of which were actually pulling in opposite directions, but basically it had to be light-client-friendly algorithm whose speed-of-mining is proportional to the IO-bandwidth and which requires a considerable amount of RAM to do so. There was a vague consensus that we (well.. Vitalik and Matthew) head in the direction of a Hasimoto-like algorithm (a proof-of-work designed for the Bitcoin blockchain that aims to be IO-bound, meaning, roughly, that to make it go any faster, you'd need to add more memory rather than just sponsoring a smaller/faster ASIC). Since our blockchain has a number of important differences with the Bitcoin blockchain (mainly in transaction density), stemming from the extremely short 12s block time we're aiming for, we would have to use not the blockchain data itself like Hashimoto but rather an artifcially created dataset, done with an algorithm known as Dagger (yes, some will remember it as Vitalik's first and flawed attempt at a memory-hard proof-of-work).

While this looked like a good direction to be going in, a swift audit of Vitalik and Matt's initial algorithm by Tim Hughes (ex-Director of Technology at Frontier Developments and expert in low-level CPU and GPU operation and optimisation) showed major flaws. With his help, they were able to work together to devise a substantially more watertight algorithm that, we are confident to say, should make the job of developing an FPGA/ASIC sufficiently difficult, especially given our determination to switch to a proof-of-stake system within the next 6-12 months.

Last, but not least, the new website was launched. Kudos to Ian and Konstantin for mucking down and getting it done. Next stop will be the developer site, which will be loosely based on the excellent resource at qt.io, the aim to provide a one-stop extravaganza of up to date reference documentation, curated tutorials, examples, recipes, downloads, issue tracking, and build status.

<strong>Onwards</strong>

So, as Alex, our networking maestro might say, these are exciting times. When deep in nitty gritty of development you sometimes forget quite how world-altering the technology you're creating is, which is probably just as well since the gravity of the matter at hand would be continually distracting. Nonetheless, when one starts considering the near-term alterations that we can really bring one realises that the wave of change is at once unavoidable and heading straight for you. For what it's worth, I find an excellent accompaniment to this crazy life is the superb music of Pretty Lights.