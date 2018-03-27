---
id: 2034
title: 'Security Alert – [Implementation bug in Go clients causing increase in difficulty – Fixed – Miners check and update Go clients]'
date: 2015-09-03T01:44:08+00:00
author: Jutta Steiner
layout: post
guid: https://blog.ethereum.org/?p=2034
permalink: /2015/09/03/security-alert-implementation-bug-in-go-clients-causing-increase-in-difficulty-fixed-miners-check-and-update-go-clients-if-necessary/
dsq_thread_id:
  - "4091864948"
---
<strong>Implementation bug in the go client leads to steady increase of difficulty independent of hashing power.</strong>

<b>Affected configurations: </b><span style="font-weight: 400">All</span> <span style="font-weight: 400">Go client versions v1.0.x, v1.1.x, release and develop branches. The bug was introduced in a recent update and release through commit </span><a href="https://github.com/ethereum/go-ethereum/commit/7324176f702a77fc331bf16a968d2eb4bccce021"><span style="font-weight: 400">https://github.com/ethereum/go-ethereum/commit/7324176f702a77fc331bf16a968d2eb4bccce021</span></a><span style="font-weight: 400"> which went into the affected client versions. All miners running earlier mentioned versions are affected and are advised to update as soon as possible. </span>

<b>Likelihood: </b><span style="font-weight: 400">High</span>

<b>Severity: </b><span style="font-weight: 400">Medium</span>

<b>Impact: </b><span style="font-weight: 400">Increase in block time will lead to an exponential increase in difficulty </span>

<b>Details:</b><span style="font-weight: 400"> A bug in the go client leads to steady increase in difficulty in the following block, as timestamp in new block = timestamp + 1 of old block, regardless of the actual time, when mining. This leads to an increase in the difficulty independently of the hashing power</span>

<b>Effects on expected chain reorganisation depth: </b><span style="font-weight: 400">None</span>

<b>Proposed temporary workaround:</b><span style="font-weight: 400"> None</span>

<b>Remedial action taken by Ethereum</b><span style="font-weight: 400">: Provision of hotfixes as below:</span>

<span style="font-weight: 400">If using the PPA: </span><code><span style="font-weight: 400">sudo apt-get update</span></code><span style="font-weight: 400"> then </span><code><span style="font-weight: 400">sudo apt-get upgrade</span></code>

<span style="font-weight: 400">If using brew: </span><code><span style="font-weight: 400">brew update</span></code><span style="font-weight: 400"> then </span><code><span style="font-weight: 400">brew reinstall ethereum</span></code>

<span style="font-weight: 400">If using a windows binary: download the updated binary from the </span><a href="https://github.com/ethereum/go-ethereum/releases/tag/v1.1.2"><span style="font-weight: 400">release page</span></a>

<span style="font-weight: 400">If you are building from source: </span><span style="font-weight: 400">git pull</span><span style="font-weight: 400"> followed by </span><span style="font-weight: 400">make geth</span><span style="font-weight: 400"> (please use the Master branch </span><span style="font-weight: 400"><code>587669215b878566c4a7b91fbf88a6fd2ec4f46a</code>)</span>