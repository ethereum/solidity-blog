---
id: 2021
title: 'Security Advisory [Implementation bugs in Go and Python clients can cause DoS – Fixed – Please update clients]'
date: 2015-09-02T18:24:05+00:00
author: Jutta Steiner
layout: post
guid: https://blog.ethereum.org/?p=2021
permalink: /2015/09/02/security-advisory-implementations-bugs-in-go-and-python-clients-can-cause-dos/
dsq_thread_id:
  - "4090797185"
---
<strong>State transition and consensus issue in geth client causes panic (crash) when processing a (valid) block with a specific combination of transactions, which may cause overall network instability if block is accepted and relayed by unaffected clients thus causing a DoS. This may happen in a block that contains transactions which suicide to the block reward address. </strong>

<b>Affected configurations: </b><span style="font-weight: 400">Issue reported for Geth.While investigating the issue, related issues were discovered and corrected in pyethereum, hence pyethapp is also affected. C++ clients are unaffected.</span>

<b>Likelihood: </b><span style="font-weight: 400">Low</span>

<b>Severity: </b><span style="font-weight: 400">High</span>

<b>Complexity: </b><span style="font-weight: 400">High</span>

<b>Impact: </b><span style="font-weight: 400">Network Instability and DoS</span>

<b>Details: <span style="font-weight: 400">A block containing a specific combination of transactions which include one or more SUICIDE calls, while valid, causes panic crash in go-ethereum client and crash in pyethereum. Additional details may be posted when available.</span></b>

<b>Effects on expected chain reorganisation depth: </b>None.

<b>Remedial action taken by Ethereum</b><span style="font-weight: 400">: Provision of fixes as below.</span>

<b>Proposed temporary workaround:</b><span style="font-weight: 400"> Switch to unaffected client such as eth (C++).</span>

<b>Fix:</b><span style="font-weight: 400">Upgrade geth and pyethereum client software.</span>

<b>go-ethereum (geth):</b>

<span style="font-weight: 400">Please note that the current stable version of geth is now 1.1.1; if you are running 1.0 and using a package manager such as apt-get or homebrew the client will be upgraded.</span>

<span style="font-weight: 400">If using the PPA: </span><span style="font-weight: 400">sudo apt-get update</span><span style="font-weight: 400"> then </span><span style="font-weight: 400">sudo apt-get upgrade</span>

<span style="font-weight: 400">If using brew: </span><span style="font-weight: 400">brew update</span><span style="font-weight: 400"> then </span><span style="font-weight: 400">brew reinstall ethereum</span>

<span style="font-weight: 400">If using a windows binary: download the </span><a href="https://build.ethdev.com/builds/Windows%20Go%20master%20branch/Geth-Win64-20150902012504-1.1.1-8f09242.zip"><span style="font-weight: 400">updated binary</span></a><span style="font-weight: 400">.</span>

<span style="font-weight: 400">If you are building from source: </span><span style="font-weight: 400">git pull</span><span style="font-weight: 400"> followed by </span><span style="font-weight: 400">make geth</span><span style="font-weight: 400"> (please use the Master branch commit </span><code><span style="font-weight: 400">8f09242d7f527972acb1a8b2a61c9f55000e955d)</span></code>

&nbsp;

<span style="font-weight: 400">The correct version for this update on Ubuntu AND OSX is Geth/v1.1.1-</span><span style="font-weight: 400">8f09242d</span>

<b>pyethereum:</b>

<span style="font-weight: 400">Users of pyethapp should reinstall </span>

<code><span style="font-weight: 400">&gt; pip install pyethapp --force-reinstall</span></code>