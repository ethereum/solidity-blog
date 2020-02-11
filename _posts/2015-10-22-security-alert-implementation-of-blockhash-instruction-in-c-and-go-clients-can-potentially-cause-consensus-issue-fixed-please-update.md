---
id: 2216
title: 'Security alert [Implementation of BLOCKHASH instruction in C++ and Go clients can potentially cause consensus issue – Fixed. Please update.]'
date: 2015-10-22T17:06:53+00:00
author: Gustav Simonsson
layout: post
guid: https://blog.ethereum.org/?p=2216
permalink: /2015/10/22/security-alert-implementation-of-blockhash-instruction-in-c-and-go-clients-can-potentially-cause-consensus-issue-fixed-please-update/
dsq_thread_id:
  - "4249046261"
category: Security
---
<b>Summary: </b>Erroneous implementation of BLOCKHASH can trigger a chain reorganisation leading to consensus problems

<b>Affected configurations: </b>All geth versions up to 1.1.3 and 1.2.2. All eth versions prior to 1.0.0.

<b>Likelihood: </b>Low

<b>Severity: </b>Medium

<b>Impact: </b>Medium

<b>Details:</b> Both C++ (eth) and Go (geth) clients have an erroneous implementation of an edge case in the Ethereum virtual machine, specifically which chain the BLOCKHASH instruction uses for retrieving a block hash. This edge case is very unlikely to happen on a live network as it would only be triggered in certain types of chain reorganisations (a contract executing BLOCKHASH(N - 1) where N is the head of a non-canonical subchain that is not-yet reorganised to become the canonical (best/longest) chain but will be after the block is processed).

pyethereum is unaffected.

<b>Effects on expected chain reorganisation depth: </b>none

<b>Remedial action taken by Ethereum</b>: Provision of hotfixes as below.
<h3><strong>Geth:</strong></h3>
<strong>PPA</strong>: sudo apt-get update then sudo apt-get upgrade

<strong>Brew</strong>: brew update then brew reinstall ethereum

<strong>Windows</strong>: download the updated binary from <a href="https://github.com/ethereum/go-ethereum/releases/tag/v1.2.3">https://github.com/ethereum/go-ethereum/releases/tag/v1.2.3</a>

<strong>Building from source:</strong>
<pre>git fetch origin &amp;&amp; git checkout origin/master</pre>
<pre>make geth

</pre>
<div>Master branch commit: <a href="https://github.com/ethereum/go-ethereum/commit/e55594ca0e131d518944e98701fc067735b11152">e55594ca0e131d518944e98701fc067735b11152</a></div>
<div></div>
<h3><strong>Eth:</strong></h3>
<strong>PPA</strong>: <a href="https://gavofyork.gitbooks.io/turboethereum/content/chapter1.html">https://gavofyork.gitbooks.io/turboethereum/content/chapter1.html</a>
<div>

<strong>Window and Mac OS X:</strong>  <a href="https://build.ethdev.com/cpp-binaries-data/">https://build.ethdev.com/cpp-binaries-data/</a>

<strong>Source:</strong> <a href="https://github.com/ethereum/webthree-umbrella/tree/release">https://github.com/ethereum/webthree-umbrella/tree/release</a>

<strong>Building from source:</strong> <a href="https://github.com/ethereum/webthree-umbrella/wiki">https://github.com/ethereum/webthree-umbrella/wiki</a>

&nbsp;

&nbsp;

</div>