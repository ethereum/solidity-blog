---
id: 1196
title: Gav’s Ethereum ÐΞV Update IV
date: 2014-12-15T23:25:29+00:00
author: Gavin Wood
layout: post
guid: https://blog.ethereum.org/?p=1196
permalink: '/2014/12/15/gavs-ethereum-d%ce%bev-update-iv/'
dsq_thread_id:
  - "3327494939"
image: /wp-content/uploads/2014/12/IMG_7465.jpg
tags:
  - dev
  - ethereum
  - gav
  - update
---
Time for another update! So quite a bit has happened following ÐΞVcon-0, our internal developer's conference. The conference itself was a great time to get all the developers together and really get to know each other, dissipate a *lot* of information (back to back presentations for 5 days!) and chat over a lot of ideas. The comms team will be releasing each of the presentations as fast as Ian can get them nicely polished.

During the time since the last update, much has happened including, finally, the release of the Ethereum ÐΞV website, ethdev.com. Though relatively simple as present, there are great plans to extend this into a developer's portal in which you'll be able to browse the bug bounty programme, look at and, ultimately follow tutorials, look up documentation, find the latest binaries for each platform and see the progress of builds.

As usual I have been mostly between Switzerland, the UK and Berlin, during this time. Now that ÐΞV-Berlin is settled in the hub, we have a great collaboration space in which volunteers can work, collaborate, bond and socialise alongside our more formal hires. Of late, I have been working to finish up the formal specification of Ethereum, the Yellow Paper, and make it up to date with the latest protocol changes in order that the security audit get underway. Together we have been putting the finishing touches on seventh, and likely final, proof-of-concept code, delayed largely due to a desire to make it the final PoC release for protocol changes. I've also been doing some nice core refactoring and documentation, specifically removing two long standing dislikes of mine, the State::create and State::call methods and making the State class nicer for creating custom states useful when developing contracts. You can expect to see the fruits of this work in Milestone II of Mix, Ethereum's official IDE.

<strong>Ongoing Recruitment</strong>

On that note, I'm happy to announce that we have hired Arkadiy Paronyan, a talented developer originally from Russia who will be working with Yann on the Mix IDE. He's got off to a great start on his first week helping on the front-end with the second milestone. I'm also very pleased to announce that we hired Gustav Simonsson. Being an expert Erlang with Go experience with considerable expertise in network programming and security reviewing, he will initially be working with Jutta on the Go code base security audit before joining the Go team.

We also have another two recruits: Dimitri Khoklov and Jason Colby. I first met Jason in the fateful week back last January when the early Ethereum collaborators got together for a week before the North American Bitcoin conference where Vitalik gave the first public talk about Ethereum. Jason, who has moved to Berlin from his home in New Hampshire, is mostly working alongside Aeron and Christian to help to look after the hub and looking after various bits of administration that need to be done. Dimitri, who works from Tver in Russia is helping flesh out our unit tests with Christoph, ultimately aiming towards full code coverage.

We have several more recruits that I'd love to mention but can't announce quite yet - watch this space... (:

<strong>Ongoing Projects</strong>

I'm happy to say that after a busy weekend, Marek, Caktux, Nick and Sven have managed to get the Build Bot, our CI system, building on all three platforms cleanly again. A special shout goes out to Marek who tirelessly fought with CMake and MSVC to bend the Windows platform to his will. Well done to all involved.

Christian continues to power through on the Solidity project, aided now by Lefteris who is focusing on parsing and packaging the NatSpec documentation. The latest feature to be added allows for the creation of new contracts in a beautiful manner with the <code>new</code> keyword. Alex and Sven are beginning to work on the project of introducing network well-formedness into the p2p subsystem using the salient parts of the well-proven Kademlia DHT design. We should begin seeing some of this stuff in the code base within before the year-end.

I'm also happy to announce that the first successful message was sent between Go & C++ clients on our messaging/hash-table hybrid system, codenamed Whisper. Though only at an early proof-of-concept stage, the API is reasonably robust and fixed, so largely ready to prototype applications on.

<strong>New Projects</strong>

Marian is the lucky guy who has been tasked with developing out what will be our awesome web-based C&C deck. This will provide a public website whose back-end connects to a bunch of nodes around the world and displays real-time information on network status including chain length and a chain-fork early warning system. Though accessible by anyone, we will of course have a dedicated monitor on at all times for this page at the hub.

Sven, Jutta and Heiko have also begun a most interesting and important project: the Ethereum stress-testing project. Designed to study and test the network in a range of real-life adverse situations prior to release, they will construct infrastructure allowing the setup of many (10s, 100s, even 1000s of) nodes each individually remote-controllable and able to simulate circumstances such as ISP attacks, net splits, rogue clients, arrival and departure of large amounts of hash-power and measure attributes like block & transaction propagation times and patterns, uncle rates and fork lengths. A project to watch out for.

<strong>Conclusions</strong>

The next time I write this I hope to have released PoC-7 and be on the way to the alpha release (not to mention have the Yellow Paper out). I expect Jeff will be doing an update concerning the Go side of things soon enough. Until then, watch out for the PoC-7 release and mine some testnet Ether!