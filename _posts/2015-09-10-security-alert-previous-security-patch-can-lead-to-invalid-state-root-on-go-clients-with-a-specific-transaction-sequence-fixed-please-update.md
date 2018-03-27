---
id: 2052
title: 'Security Alert – [Previous security patch can lead to invalid state root on Go clients with a specific transaction sequence – Fixed. Please update.]'
date: 2015-09-10T13:19:27+00:00
author: Jutta Steiner
layout: post
guid: https://blog.ethereum.org/?p=2052
permalink: /2015/09/10/security-alert-previous-security-patch-can-lead-to-invalid-state-root-on-go-clients-with-a-specific-transaction-sequence-fixed-please-update/
dsq_thread_id:
  - "4115787746"
---
&nbsp;

<b>Summary: </b><span style="font-weight: 400">Implementation bug in the go client may lead to invalid state</span>

<b>Affected client versions: </b>Latest (unpatched) versions of Go client; v1.1.2, v1.0.4 tags and develop, master branches before September 9.

<b>Likelihood: Low</b>

<b>Severity: High</b>

<b>Impact: High</b>

<b>Details:</b><span style="font-weight: 400"> Go ethereum client does not correctly restore state of execution environment when a transaction goes out-of-gas if - within the same block - a contract was suicided. This would result in an invalid copy operation of the state object; flagging the contract as </span><b>not deleted</b><span style="font-weight: 400">. This operation would cause a consensus issue between the other implementations.</span>

&nbsp;

<b>Effects on expected chain reorganisation depth: </b><span style="font-weight: 400">none</span>

<b>Remedial action taken by Ethereum</b><span style="font-weight: 400">: Provision of hotfixes as below. </span>

<b>Proposed temporary workaround:</b><span style="font-weight: 400"> Use Python or C++ client</span>

&nbsp;

<span style="font-weight: 400">If using the PPA: </span><code><span style="font-weight: 400">sudo apt-get update</span></code><span style="font-weight: 400"> then </span><code><span style="font-weight: 400">sudo apt-get upgrade</span></code>

<span style="font-weight: 400">If using brew: </span><code><span style="font-weight: 400">brew update</span></code><span style="font-weight: 400"> then </span><code><span style="font-weight: 400">brew reinstall ethereum</span></code>

<span style="font-weight: 400">If using a windows binary: download the updated binary from <code>https://github.com/ethereum/go-ethereum/releases/tag/v1.1.3</code></span>

&nbsp;

<span style="font-weight: 400">Master branch commit: <code>https://github.com/ethereum/go-ethereum/commit/9ebe787d3afe35902a639bf7c1fd68d1e591622a</code></span>

&nbsp;

<span style="font-weight: 400">If you’re building from source: <code> git fetch origin &amp;&amp; git checkout origin/master </code> followed by a <code> make geth </code></span>