---
id: 2696
title: 'Security Alert – Geth suffers from a very low probable DoS attack vector &#8211; Update immediately'
date: 2016-05-17T18:39:54+00:00
author: Jeffrey Wilcke
layout: post
guid: https://blog.ethereum.org/?p=2696
permalink: /2016/05/17/security-alert-geth-suffers-from-a-very-low-probable-dos-attack-vector-update-immediately/
dsq_thread_id:
  - "4834819226"
category: Security
---
<b>Affected configurations: </b><span style="font-weight: 400;">All</span> <span style="font-weight: 400;">Go client versions </span>

<b>Likelihood: </b><span style="font-weight: 400;">Very low</span>

<b>Severity: </b><span style="font-weight: 400;">High</span>

<b>Details:</b><span style="font-weight: 400;"> A bug in Geth (and potentially other clients) may suffer from a DoS attack and allows remote attackers to stall synchronisation process almost indefinitely by supplying a valid, lighter chain. More information will be given out a later time including the report that was submitted through the bug bounty program.</span>

<b>Effects on expected chain reorganisation depth: </b><span style="font-weight: 400;">None</span>

<b>Proposed temporary workaround:</b><span style="font-weight: 400;"> None</span>

<b>Remedial action taken by Ethereum</b><span style="font-weight: 400;">: Provision of hotfixes as below:</span>

If you're using Mist: download the updated binary from the <a href="https://github.com/ethereum/mist/releases/tag/0.7.4">release page</a>

<span style="font-weight: 400;">If using the PPA: </span><code><span style="font-weight: 400;">sudo apt-get update</span></code><span style="font-weight: 400;"> then </span><code><span style="font-weight: 400;">sudo apt-get upgrade</span></code>

<span style="font-weight: 400;">If using brew: </span><code><span style="font-weight: 400;">brew update</span></code><span style="font-weight: 400;"> then </span><code><span style="font-weight: 400;">brew reinstall ethereum</span></code>

<span style="font-weight: 400;">If using a windows binary: download the updated binary from the </span><a href="https://github.com/ethereum/go-ethereum/releases/tag/v1.4.4"><span style="font-weight: 400;">release page</span></a>

<span style="font-weight: 400;">If you are building from source: </span><span style="font-weight: 400;">git pull</span><span style="font-weight: 400;"> followed by </span><span style="font-weight: 400;">make geth</span><span style="font-weight: 400;"> (please use the Master branch <code>94ad694a26ca3f7776ec8240802596755e5d5c0a</code></span><span style="font-weight: 400;">)</span>