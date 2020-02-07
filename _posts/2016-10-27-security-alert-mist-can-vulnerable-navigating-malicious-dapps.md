---
id: 3303
title: 'Security Alert &#8211; Mist can be vulnerable when navigating to malicious DApps'
date: 2016-10-27T11:12:05+00:00
author: Fabian Vogelsteller
layout: post
guid: https://blog.ethereum.org/?p=3303
permalink: /2016/10/27/security-alert-mist-can-vulnerable-navigating-malicious-dapps/
dsq_thread_id:
  - "5257011974"
category: Security
---
Mist leaks some low level APIs, which Dapps could use to gain access to the computer's file system and read/delete files. This would only affect you if you navigate to an untrusted Dapp that knows about these vulnerabilities and specifically tries to attack users. Upgrading Mist is highly recommended to prevent exposure to attacks.

<strong>Affected configurations</strong>: All versions of Mist from 0.8.6 and lower. This vulnerability doesn't affect the Ethereum Wallet since it can’t load external DApps.
<strong>Likelihood</strong>: Medium
<strong>Severity</strong>: High
<h2>Summary</h2>
Some Mist API methods were exposed, making it possible for malicious webpages to gain access to a privileged interface that could delete files on the local filesystem or launch registered protocol handlers and obtain sensitive information, such as the user directory or the user's "coinbase".
Vulnerable exposed mist APIs:
<code>mist.shell</code>
<code>mist.dirname</code>
<code>mist.syncMinimongo</code>
<code>web3.eth.coinbase</code> is now <code>null</code>, if the account is not allowed for the dapp
<h2>Solution</h2>
Upgrade to the<a href="https://github.com/ethereum/mist/releases/tag/v0.8.7"> latest version of the Mist Browser</a>. Do not use any previous Mist versions to navigate to any untrusted webpage, or local webpages from unknown origins. The Ethereum Wallet is not affected as it doesn't allow navigation to external pages.
This is a good reminder that Mist is currently only considered for Ethereum App Development and should not be used for end users to navigate on the open web until it has reached at least version 1.0. An external audit of Mist is scheduled for December.

A big thanks goes to <a href="https://github.com/tintinweb">@tintinweb</a> for his very useful reproduction app to test the vulnerabilities!

We are also thinking of adding Mist to the bounty program, if you find vulnerabilities or severe bugs please contract us at <a href="mailto:bounty@ethereum.org">bounty@ethereum.org</a>

<div id="disqus_link_container"></div>
<script>jQuery(document).ready(function() { EthBlogUtils.display_disqus_link();});</script>