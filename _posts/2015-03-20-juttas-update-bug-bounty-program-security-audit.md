---
id: 1525
title: 'Jutta&#8217;s update on bug bounty program and security audit'
date: 2015-03-20T11:18:25+00:00
author: Jutta Steiner
layout: post
guid: https://blog.ethereum.org/?p=1525
permalink: /2015/03/20/juttas-update-bug-bounty-program-security-audit/
dsq_thread_id:
  - "3610417839"
tags:
  - ethereum project
  - Security audit
  - update
---
Hi, <a href="https://twitter.com/tajukahe">Jutta</a> writing again – I initially introduced myself when we started the bounty program earlier this year and I’m happy to provide you with an update on what’s happening on the security side prior and throughout launch.

We have had some high quality submissions by bounty hunters – kudos for the creative exploits sent along that the bugs made possible. The number of submissions recently rose. Hence we decided and are announcing that we will continue the bounty program at least throughout the Frontier phase of Ethereum’s launch plan – see <a href="https://blog.ethereum.org/2015/03/02/gavs-ethereum-d%CE%BEv-update-v/">Gav's</a> and <a href="https://blog.ethereum.org/2015/03/03/ethereum-launch-process/">Vinay's</a> respective blog posts.

Please go to our <a href="https://bounty.ethdev.com/">bounty website</a> for more information on the bounty program and make sure check out our lead hunter’s repository <a href="https://github.com/jonasnick/eth-neg-value-tx">here</a> for helpful testing scripts before starting the hunt.

Not only do we rely on individual bug hunters and the community: together with EthDev’s  <a href="https://github.com/Gustav-Simonsson/">Gustav Simonsson</a>, I had started the process of selecting professional security experts, academics and blockchain experts for our external security audits late last year. Gustav is now working with auditors and the Ethereum Go dev team to track all security issues tagged <a href="https://github.com/ethereum/go-ethereum/labels/vuln">here</a> and work out fixes for them. We want to keep good track of all issues and only close them once fully resolved and solutions sufficiently tested. Every bug we find is taken care of and will be fixed before Frontier release. Feel free to follow us on github if you want to keep an eye on the progress.

The first round of work from security auditors is finishing in a couple of weeks, and bug fixing is already well underway. Working through all issues will take the time it takes. It’s a security-driven not schedule-driven process, after all.