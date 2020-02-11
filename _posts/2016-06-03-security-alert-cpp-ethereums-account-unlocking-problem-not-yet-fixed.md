---
id: 2706
title: 'Security Alert &#8211; cpp-ethereum&#8217;s account unlocking problem not yet fixed [Now fixed]'
date: 2016-06-03T18:05:58+00:00
author: Christian Reitwiessner
layout: post
guid: https://blog.ethereum.org/?p=2706
permalink: /2016/06/03/security-alert-cpp-ethereums-account-unlocking-problem-not-yet-fixed/
dsq_thread_id:
  - "4881044956"
category: Security
---
<b>Affected configurations:</b> cpp-ethereum (eth, AlethZero, …) version 1.2.0 up to 1.2.<strong>6</strong>
Note: Neither “geth” nor “Mist” nor the “Ethereum Wallet” (unless explicitly used together with cpp-ethereum) are affected by this, they lock accounts correctly again.

This is just a quick head's up that <a href="https://blog.ethereum.org/2016/05/31/security-alert-cpp-ethereum-keeps-accounts-unlocked/">cpp-ethereum's security issue around account security</a> is not yet properly fixed. The fix that is part of cpp-ethereum version 1.2.6 did not decrease the security, but it also did not fix the bug completely. We will work towards another bugfix release and announce it once the fix is properly verified.

A fix is now available: Release 1.2.7 - https://github.com/ethereum/webthree-umbrella/releases/tag/v1.2.7