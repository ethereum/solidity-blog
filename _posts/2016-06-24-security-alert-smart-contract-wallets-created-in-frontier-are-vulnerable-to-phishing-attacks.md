---
id: 2750
title: Security Alert – Smart Contract Wallets created in frontier are vulnerable to phishing attacks
date: 2016-06-24T13:58:05+00:00
author: Fabian Vogelsteller
layout: post
guid: https://blog.ethereum.org/?p=2750
permalink: /2016/06/24/security-alert-smart-contract-wallets-created-in-frontier-are-vulnerable-to-phishing-attacks/
dsq_thread_id:
  - "4935387322"
category: Security
---
<h1></h1>
<b>Affected configurations:</b><span style="font-weight: 400"> All smart contract wallets created using <em>Ethereum Wallet  Frontier, version 0.4.0 (Beta 7) or earlier</em>. Wallets created with Ethereum Wallet 0.5.0 and all later versions released after March 3, 2016, are not affected.</span>

<b>Likelihood</b><span style="font-weight: 400">: Low</span>

<b>Severity</b><span style="font-weight: 400">: High</span>
<h2>Summary:</h2>
<span style="font-weight: 400">Do not use wallet contracts or owner accounts of those wallets that were created by the Ethereum Wallet 0.4.0 or earlier. If you send to (or interact with) a malicious contract it could take ownership of your wallet contract. Create a new wallet and move your funds.</span>
<h3>How to be super safe??</h3>
<strong>Don't use the vulnerable wallet contracts, AND the owner accounts of these wallets to send ether and interact with contracts you don't know!
If you don't use these accounts and wallets, and upgrade your wallet as </strong><b>described <a href="https://github.com/ethereum/mist/releases/tag/0.7.6">here</a>, you are safe!</b>
<h2>Details:</h2>
<span style="font-weight: 400">An attack vector was discovered that affects the smart contract wallets created before the Homestead release (Frontier phase). The attack can happen if an affected wallet interacts with a malicious contract OR if the owner account of an affected wallet interacts with a malicious contract that knows the address of his wallet. An attacker can then impersonate the owner and thus can steal funds or tokens and change the owner of the wallet.</span>

<strong>If you do not use your wallet and owner accounts with contracts you don't know, you are safe!</strong>

<span style="font-weight: 400">Receiving Ether and sending Ether to non-contract accounts is fine.</span>

<span style="font-weight: 400">Also if you configured your wallet with multisig, you are safer, as the attacker would need to make you send with all owners to malicious contract(s).</span>

&nbsp;
<h2>Proposed solution:</h2>
<span style="font-weight: 400">We recommend that if you created a wallet using the affected versions, you take one of these steps:</span>
<ul>
 	<li style="font-weight: 400"><b>Create a new wallet</b><span style="font-weight: 400"> with the latest version of Ethereum Wallet (any version from 0.5.0 or newer) and </span><b>move your funds</b><span style="font-weight: 400"> there. <a href="https://github.com/ethereum/mist/releases/tag/0.7.6">You can follow these steps</a>.</span></li>
 	<li style="font-weight: 400"><i><span style="font-weight: 400">Until you do the above</span></i><span style="font-weight: 400">, </span><b>do not use any account</b><span style="font-weight: 400"> which is an </span><b><i>owner </i></b><b>of an affected wallet, or the affected wallet itself</b><span style="font-weight: 400"> to interact with closed source or otherwise unknown contracts that might trigger arbitrary actions (including forwarding Ether). </span><b>Send/interact only to addresses you own, or know!</b></li>
 	<li style="font-weight: 400">Create a secondary account for your every day usage. This one should not be connected to your contract wallets</li>
</ul>
&nbsp;
<h2>Remedial action taken by Ethereum Foundation:</h2>
<span style="font-weight: 400">We created a new Ethereum Wallet release 0.7.6, which will detect your vulnerable wallets.</span>

<a href="https://github.com/ethereum/mist/releases/tag/0.7.6">Download the latest release and follow the steps described in the release notes to update your vulnerable wallets!</a>

<a href="https://github.com/ethereum/mist/releases/tag/0.7.6">https://github.com/ethereum/mist/releases/tag/0.7.6</a>

&nbsp;