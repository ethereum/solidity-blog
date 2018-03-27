---
id: 1928
title: 'Security Alert 1 [windows+alethzero]'
date: 2015-08-07T19:08:38+00:00
author: Jutta Steiner
layout: post
guid: https://blog.ethereum.org/?p=1928
permalink: /2015/08/07/security-alert-1-windowsalethzero-2/
dsq_thread_id:
  - "4012306944"
---
This affects users of Alethzero GUI client on Windows. Users of eth CLI client or not on the Windows platform are unlikely to be affected but should take action detailed below. <em>Users of Frontier command line interface geth are unaffected</em>.

<strong>Issue description:</strong> While setting privacy permissions on the keys directory, insufficient error handling can cause the key files to not be written; this may be widespread on the Windows platform. As such, current versions of AlethZero and eth may include identities for which there exists no underlying key. Ether Presale Claim functionality of AlethZero may result in funds automatically being transferred to these lost identities.

<strong>Workaround:</strong> Users of AlethZero version 0.9.39 and earlier should NOT use the “Claim Presale Wallet” function; users of AlethZero and eth versions 0.9.39 and earlier should not attempt to mine or receive funds into their addresses.

Users of eth and AlethZero on all platforms should consider themselves safe once they have confirmed that they do indeed have the underlying key. To check (with your existing setup) run:

<code>ethkey.exe --list</code>

You may assume that all listed addresses do indeed have a key behind them and are not suffering from this issue.

<b>Remedial action taken by Ethereum</b>: New hotfix released with changes:

- Identities for which there are no underlying keys are no longer displayed.

- Key files are written regardless of whether setting directory permissions succeeded.

<strong>Fix:</strong> Versions 0.9.40 and onwards, available from circa 2015.08.07 18:30 CEST.