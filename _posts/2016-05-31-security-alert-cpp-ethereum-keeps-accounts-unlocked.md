---
id: 2707
title: 'Security Alert &#8211; cpp-ethereum keeps accounts unlocked'
date: 2016-05-31T00:13:36+00:00
author: Christian Reitwiessner
layout: post
guid: https://blog.ethereum.org/?p=2707
permalink: /2016/05/31/security-alert-cpp-ethereum-keeps-accounts-unlocked/
dsq_thread_id:
  - "4870091266"
category: Security
---
<section class="postbody"><b>Affected configurations:</b> cpp-ethereum (eth, AlethZero, ...) version 1.2.0 up to 1.2.5 (fixed in 1.2.6)
Note: Neither "geth" nor "Mist" nor the "Ethereum Wallet" (unless explicitly used together with cpp-ethereum) are affected by this, they lock accounts correctly again.</section><section class="postbody"></section><section class="postbody"><b>Severity: </b>High</section><section class="postbody"></section><section class="postbody"><strong>Possible Attacks:</strong> Attackers can spend funds from previously used accounts if they have access to the local machine or to an exposed json-rpc interface.</section><section class="postbody"></section><section class="postbody"><b>Details:</b> Due to a bug in cpp-ethereum, accounts stay unlocked once their password has been entered until cpp-ethereum is closed. This includes accounts encrypted with the "master password" entered upon startup or any password entered through Mist. This means that an attacker can spend funds from the account as soon as they have access to the RPC interface. For that to happen, they either need access to the local filesystem or the exposed http-json-rpc interface (not the default setting). Using Mist in "Mist mode" (not the default setting) and navigating to a malicious website also provides that website access to the RPC interface.</section><section class="postbody"></section><section class="postbody"><b>Proposed temporary workaround:</b> Restart eth after each transaction, do not expose the json-rpc interface via http and upgrade to version 1.2.6 as soon as binaries are released. A fix has already been merged to the develop branch.</section>