---
id: 1202
title: 'Ethereum ÐΞV: What are we doing?'
date: 2014-12-17T16:29:20+00:00
author: Gavin Wood
layout: post
guid: https://blog.ethereum.org/?p=1202
permalink: '/2014/12/17/ethereum-d%ce%bev/'
dsq_thread_id:
  - "3333399857"
tags:
  - dev
  - ethereum
---
OK so a minor update about what we are (and are not) doing here at Ethereum DEV.

We are, first and foremost, developing a robust quasi-Turing-complete blockchain. This is known as Ethereum. Aside from having quasi-Turing-completeness, it delivers on a number of other important considerations, stemming from the fact we are developing entirely new blockchain technology including:
<ul>
<li>speedy, through a 12 second blocktime;
<li>light-client-friendly through the use of Merkle roots in headers for compact inclusion/state proofs and DHT integration to allow light clients to host & share small parts of the full chain;
<li>ÐApp-friendly, even for light-clients, through the use of multi-level Bloom filters and transaction receipt Merkle tries to allow for lightweight log-indexing and proofs;
<li>finite-blockchain-friendly - we designed the core protocol to facilitate upgrading to this technology, further reducing light-client footprint and helping guarantee mid-term scalability;
<li>ASIC-unfriendly - through the (as yet unconfirmed) choice of PoW algo and the threat we'll be upgrading to PoS in the Not-Too-Distant future.
</ul>

It is robust because:
<ul>
<li>it is unambiguously formally defined, allowing a highly tractable analysis, saturation tests and formal auditing of implementations;
<li>it has an extensive, and ultimately complete, set of tests for providing an exceptionally high degree of likelihood a particular implementation is conformant;
<li>modern software development practices are observed including a CI system, internal unit tests, strict peer-reviewing, a strict no-warnings policy and automated code analysers;
<li>its mesh/p2p backend (aka libp2p) is built on well-tested secure foundations (technology stemming from the Kademlia project);
<li>official implementations undergo a full industry-standard security audit;
<li>a large-scale stress test network will be instituted for profiling and testing against likely adverse conditions and attacks prior to final release.
</ul>

Secondly (and at an accordingly lower priority), we are developing materials and tools to make use of this unprecedented technology possible. This includes:
<ul>
<li>developing a single custom-designed CO (contract-orientated) language;
<li>developing a secure natural language contract specification format and infrastructure;
<li>formal documentation for help coding contracts;
<li>tutorials for help coding contracts;
<li>sponsoring web-based projects in order to get people into development;
<li>developing a block chain integrated development environment.
</ul>

Thirdly, to facilitate adoption of this technology, gain testers and spur further development we are developing, collaborating over and sponsoring a number of force-multiplying technologies that leverage pre-existing technology including:
<ul>
<li>a graphical client "browser" (leveraging drop-in browser components from the Chromium project and Qt 5 technology);
<li>a set of basic contracts and ÐApps, including for registration, reputation, web-of-trust and accounting (leveraging the pre-existing compilers and development tech);
<li>a hybrid multi-DHT/messaging system, codenamed Whisper (leveraging the pre-existing p2p back end & protocols);
<li>a simple reverse-hash lookup DHT, codenamed Swarm (also leveraging the pre-existing p2p back end & protocols), for which there is an ongoing internal implementation, but which could end up merging or being a collaboration with the IPFS project.
</ul>

We are no longer actively targeting multiple languages (LLL and Mutan are mothballed, Serpent is continued as a side project). We are not developing any server technology. And, until there is a working, robust, secure and effective block chain alongside basic development tools, other parts of this overall project have substantially lower priority.

Following on from the release of the Ethereum block chain, expect the other components to get increasingly higher amounts of time dedicated to them.