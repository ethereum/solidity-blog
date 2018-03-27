---
id: 4051
title: Ethereum scalability research and development subsidy programs
date: 2018-01-02T17:42:21+00:00
author: Vitalik Buterin
layout: post
guid: https://blog.ethereum.org/?p=4051
permalink: /2018/01/02/ethereum-scalability-research-development-subsidy-programs/
---
The Ethereum community, key developers and researchers and others have always recognized scalability as perhaps the single most important key technical challenge that needs to be solved in order for blockchain applications to reach mass adoption. Blockchain scalability is difficult primarily because a typical blockchain design requires every node in the network to process every transaction, which limits the transaction processing capacity of the entire system to the capacity of a single node.

<span style="font-weight: 400;">There are two main paths to improving blockchain scalability. The first (“<a href="https://github.com/ethereum/wiki/wiki/Sharding-FAQ">sharding</a>”) involves creating better-designed base-layer blockchain protocols, which still maintain most of the desired decentralization and security properties of a blockchain that we see in the simple designs available today but only require a small percentage of nodes to see and process every transaction, allowing many more transactions to be processed in parallel at the same time. The second involves creating “layer 2” protocols that send most transactions off-chain and only interact with the underlying blockchain in order to enter and exit from the layer-2 system and in the case of attacks on the system.</span>

<span style="font-weight: 400;">We view the two strategies as complementary with each other and we believe in supporting a multi-pronged strategy toward Ethereum scalability that engages both strategies and treats them as complementary with each other.</span>

<b>Technical reading materials on Ethereum scalability technologies</b>

<span style="font-weight: 400;">Sharding:</span>
<ul>
 	<li style="font-weight: 400;"><span style="font-weight: 400;">Sharding FAQ: </span><a href="https://github.com/ethereum/wiki/wiki/Sharding-FAQ"><span style="font-weight: 400;">https://github.com/ethereum/wiki/wiki/Sharding-FAQ</span></a></li>
 	<li style="font-weight: 400;"><span style="font-weight: 400;">A note on data availability and erasure coding: </span><a href="https://github.com/ethereum/research/wiki/A-note-on-data-availability-and-erasure-coding"><span style="font-weight: 400;">https://github.com/ethereum/research/wiki/A-note-on-data-availability-and-erasure-coding</span></a></li>
 	<li style="font-weight: 400;"><span style="font-weight: 400;">Sharding preliminary spec: </span><a href="https://github.com/ethereum/sharding/blob/develop/docs/doc.md"><span style="font-weight: 400;">https://github.com/ethereum/sharding/blob/develop/docs/doc.md</span></a></li>
 	<li style="font-weight: 400;"><span style="font-weight: 400;">Video describing the strategy for incorporating sharding into Ethereum over time: </span><span style="font-weight: 400;"><a href="https://www.youtube.com/watch?v=Yo9o5nDTAAQ&amp;amp;feature=youtu.be&amp;amp;t=7h55m33s">https://www.youtube.com/watch?v=Yo9o5nDTAAQ&amp;amp;feature=youtu.be&amp;amp;t=7h55m33s</a></span></li>
</ul>
<span style="font-weight: 400;">Examples of existing layer-2 systems:</span>
<ul>
 	<li style="font-weight: 400;"><span style="font-weight: 400;">Plasma: </span><a href="http://plasma.io/"><span style="font-weight: 400;">http://plasma.io/</span></a></li>
 	<li style="font-weight: 400;"><span style="font-weight: 400;">State channels: </span><a href="http://www.jeffcoleman.ca/state-channels/"><span style="font-weight: 400;">http://www.jeffcoleman.ca/state-channels/</span></a></li>
 	<li style="font-weight: 400;"><span style="font-weight: 400;">Raiden: </span><span style="font-weight: 400;"><a href="https://raiden.network/101.html">https://raiden.network/101.html</a></span></li>
</ul>
<span style="font-weight: 400;">With the Ethereum blockchain reaching 1 million transactions per day, and both Ethereum and other blockchain projects frequently reaching their full transaction capacity, the need for scaling progress is becoming more and more clear and urgent. To that end, in addition to ongoing and upcoming work that is happening on scalability internally, we are starting two experimental subsidy schemes that we hope will empower more independent teams to collaborate with the Ethereum Research team’s base-layer scalability research and development efforts as well as building independent layer-2 projects that can tie into and improve Ethereum’s scalability.</span>

<span style="font-weight: 400;">Independent teams of developers, companies, and university and academic groups are all welcome to apply; we recognize that different types of applicants may require different formats and processes and we are willing to be flexible to accommodate individual teams’ needs.</span>

<b>Sharding client subsidy program</b>

<span style="font-weight: 400;">Over the last few months, development on sharding has picked up quickly. A specification for an initial prototype is </span><a href="https://github.com/ethereum/sharding/blob/develop/docs/doc.md"><span style="font-weight: 400;">close to finalized</span></a><span style="font-weight: 400;">, with a roadmap that allows it to be slowly introduced into Ethereum, first as a “loosely coupled” sidechain anchored into the Ethereum base chain through a “validator manager contract”, later introducing tighter and tighter integration with the Ethereum base chain over time. A reference implementation is being built in python on top of </span><a href="https://github.com/ethereum/py-evm/issues/190"><span style="font-weight: 400;">Py-EVM</span></a><span style="font-weight: 400;">, and a testnet in python is not too far away. </span>

<span style="font-weight: 400;">And in this next step, we want you to be involved. We want the Ethereum sharding testnet, and later sharding mainnet, to be a multi-client ecosystem right from the start, with the Ethereum Foundation not supporting any single privileged production implementation. The Ethereum Foundation-funded research team will continue to build an implementation in python and possibly other languages, but this is intended as a reference and proof of concept first and foremost. While we aim to continue to focus heavily on research and specification, we do not want to ultimately “win” the competition for which client gets the most actual users once the network goes live.</span>

<span style="font-weight: 400;">Instead, the Ethereum Foundation will be making subsidies available to independent groups in the community that want to help build an implementation and participate in the sharding testnets and mainnet. These payments are NOT intended to be sources of substantial profit to recipient organizations; they are rather intended to cover some of the costs involved, with the understanding that anyone who participates in the scheme will have access to a unique opportunity to participate in Ethereum 2.0 development, with close collaboration with core Ethereum researchers, and be part of the development of one of the first clients that will be available when the sharding mainnet goes live.</span>

<span style="font-weight: 400;">This will take the form of a specialized program, which will exist alongside more general grant program that the Foundation will release soon. Subsidy amounts of $50,000 up to $1,000,000 will be available, and possibly more for highly successful projects; the size of the subsidy will take into account the quality of the team, the scope of the proposed implementation, and the progress of the project over time. Participants will interact closely with the core research team, and will have a key role in shaping the final specification that gets developed over the course of implementing the spec and running the test networks.</span>

<b>Layer-2 scalability solution subsidy program</b>

<span style="font-weight: 400;">There has been a large number of independent proposals recently for how blockchains such as Ethereum can be scaled up through second-layer protocols. We recognize and appreciate that developers and researchers are excited about researching and implementing technologies in this area, and that many teams want the freedom to conceive and build out their own design that incorporates their own ideas. We want to offer an opportunity for such teams to exercise their creativity and build out their scalable blockchain proposals, all while staying within the Ethereum family.</span>

<span style="font-weight: 400;">To that end, we are announcing a subsidy scheme for projects that are building scalability and latency-reducing “layer 2” platforms that live on top of Ethereum, benefitting from the Ethereum blockchain’s security as a base layer and interoperability with the greater Ethereum community and platform.</span>

<span style="font-weight: 400;">Like the sharding client scheme, this will take the form of a specialized grant program, and subsidy amounts of $50,000 to $1,000,000 will be available depending on scope, scale and quality. The subsidy may be available even if the project has an independent business model, or funding from potential other sources in the Ethereum community, though we will prioritize funding projects that otherwise lack ability to sustain themselves, and it absolutely must be the case that the work funded is open source from end to end and provides a common good to the Ethereum ecosystem.</span>

Targets for funding include efforts at developing high-quality implementations of existing known layer-2 scaling strategies (eg. state channels, Plasma), as well as researching and developing new ones.

<b>Closing notes</b>

<span style="font-weight: 400;">Both of these programs are in a very early stage, and grants will be initially decided at the discretion of Ethereum core leadership. Details, including the terms, conditions and schedules under which payments will be made may change as the result of our initial experience with program participants, and we expect the program to solidify and expand over the course of the year. </span>

Note also that though the payments from these collaboration programs are much higher than those that we have made from our previous grant programs, these payments come with a much higher expectation of focus and quality. We are targeting skilled teams with either direct experience in the Ethereum or blockchain space, or experience in the broader fields of mechanism design, distributed systems or cryptography, as well as software engineering. The programs are also highly targeted at sharding clients and layer-2 scaling solutions; this is NOT a general-purpose grant program. That is still being developed, and details will be released as soon as they are ready.

<b>How to apply</b>

The first step is to send an email to apply@ethereumresearch.org, with the following information:
<ul>
 	<li class="gmail_extra">Official name of project, applicant and core developers</li>
 	<li class="gmail_extra">Further information on the team, including previous activity if any in the Ethereum or blockchain space or distributed systems, mechanism design or cryptography</li>
 	<li class="gmail_extra">Proposal and impact on scalability</li>
 	<li class="gmail_extra">Estimated timeline for development milestones and completion, request for grant amount and estimated total overall budget</li>
</ul>
<div class="gmail_extra"></div>
<div class="gmail_extra">If we're sufficiently interested, we'll proceed from there with requests for further information.</div>