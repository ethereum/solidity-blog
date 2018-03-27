---
id: 1802
title: How do you know Ethereum is secure?
date: 2015-07-07T02:07:17+00:00
author: Jutta Steiner
layout: post
guid: https://blog.ethereum.org/?p=1802
permalink: /2015/07/07/know-ethereum-secure/
dsq_thread_id:
  - "3910814418"
tags:
  - ethereum
  - secure
---
As I'm writing this, I’m sitting in the London office and pondering how to give you a good overview about the work we’ve been doing to secure Ethereum’s protocols, clients and p2p-network. As you might remember, I joined the Ethereum team at the end of last year to manage the security audit. As spring has passed and summer arrived and meanwhile several audits finished, it’s now a good time for me to share some results from the inspection of the world computer’s machine room. ;-)

This much is clear, as much as the delivery of the clients is an elaborate product development process, it is an exciting yet heavily complex research effort. The latter is the reason why even the best planned development schedule is subject to change as we discover more about our problem domain.

The security audit started at the end of last year with the development of a general strategy for ensuring maximum security for Ethereum. As you know, we have a security driven, rather than a schedule driven development process. With this in mind, we put together a multi-tiered audit approach consisting of:
<ul>
	<li>Analyses of the new protocols and algorithms by established blockchain researchers and specialised software security companies</li>
	<li>End-to-end audit of protocols and implementation by a world-class expert security consultancy (Go followed by C++ and a basic audit for the educational Python client), as well as</li>
	<li>The <a href="http://bounty.ethdev.com">bug bounty program</a>.</li>
</ul>
The analyses of the new protocols and algorithms covered topics like the security of:
<ul>
	<li>The gas economics</li>
	<li>The newly devised ASIC-resistant proof of work puzzle as well as</li>
	<li>The economic incentivisation of mining nodes.</li>
</ul>
The “crowd-sourced” audit component started around Christmas along with our bug bounty program. We had set aside an 11-digit satoshi amount to reward people who found bugs in our code. We’ve seen very high quality <a href="http://bounty.ethdev.com">submissions</a> to our bug bounty program and hunters received corresponding rewards. The bug bounty program is is still running and we need further submissions to use up the allocated budget...

The first major security audit (covering the gas economics and PoW puzzle) by security consultancy Least Authority was started in January and continued until the end of winter. We are very glad that we agreed with most of our external auditors that those audit reports will be publicly available once the audit work and fixing of the findings is completed. So along with this blog post, we are delighted to present the Least Authority <a href="https://github.com/LeastAuthority/ethereum-analyses/">audit report</a> and accompanying <a href="https://leastauthority.com/blog/least_authority_performs_incentive_analysis_for_ethereum.html">blog post</a>.  In addition, the report contains helpful recommendations for ÐApp developers to ensure secure design and deployment of contracts. We expect to publish further reports as they become available.

We have also engaged another software security firm at the beginning of the year to provide audit coverage on the Go implementation. Given the increased security that comes with multiple clients and as Gav mentioned in his previous post, we have also decided to give the Python and C++ audit a lightweight security audit starting early July. The C++ code will receive a full audit right after – <b>our goal with this approach is to ensure several available audited clients as early as possible during the release process.</b>

We kicked off this most encompassing audit for the Go client, aka the “end to end audit”, in February with a one-week workshop that would be followed by weeks of regular check-in calls and weekly audit reports. The audit was embedded in a comprehensive process for bug tracking and fixing, managed and thoroughly <a href="https://github.com/ethereum/go-ethereum/issues?page=1&amp;q=is%3Aissue+SEC&amp;utf8=%E2%9C%93">tracked on Github</a> by Gustav with Christoph and Dimitry coding up the corresponding required tests.

As the name implies, the end-to-end audit was scoped to cover “everything” (from networking to the Ethereum VM to syncing layer to PoW) so that at least one auditor would have cross checked the various core layers of Ethereum. One of the consultants recently summarized the situation pretty succinctly: “To be honest, the testing needs of Ethereum are more complex than anything I’ve looked at before”. As Gav reported in his <a href="https://blog.ethereum.org/2015/06/15/another-ethereum-d%CE%BEv-update/">last blog post</a>, because of the significant changes in the networking and syncing strategy we eventually decided to commission further audit work for Go – which we are about to finish this week. The kick-off for the end-to-end C++ and basic Python audits is taking place now.

The audit work with subsequent bug fixing and regression testing as well as related refactoring and redesign (of networking and syncing layer) make up the majority of work that’s keeping the developers busy right now. Likewise, fixing of findings, redesign and regression testing are the reason for the delay in the delivery. In addition, the Olympic testing phase has taught us a great deal about resiliency under various scenarios, such as slow connections, bad peers, odd behaving peers and outdated peers. The greatest challenge so far has been fighting off and recovering from forks. We learnt a lot from the recovery attempts in terms of required processes when it comes to dealing with these type of scenarios and incidents.

It might not come as a surprise that the various audits represent a significant expenditure – and we think money that could not be better invested.

As we draw closer to release, security and reliability is increasingly uppermost in our minds, particularly given the handful of critical issues found in the Olympic test release. We are very grateful for the enthusiasm and thorough work that all auditors have done so far. Their work helped us sharpen the specification in the Yellow Paper and to weed out ambiguity and fix several subtle issues, and they helped with identifying a number of implementation bugs.