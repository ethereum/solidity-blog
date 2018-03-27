---
id: 2005
title: 'Security Advisory [Insecurely configured geth can make funds remotely accessible]'
date: 2015-08-29T12:25:59+00:00
author: Jutta Steiner
layout: post
guid: https://blog.ethereum.org/?p=2005
permalink: /2015/08/29/security-alert-insecurely-configured-geth-can-make-funds-remotely-accessible/
dsq_thread_id:
  - "4077755476"
---
<strong>Insecurely configured Ethereum clients with no firewall and unlocked accounts can lead to funds being accessed remotely by attackers. </strong>

<b>Affected configurations: </b><span style="font-weight: 400">Issue reported for Geth, though all implementations incl. C++ and Python can in principle display this behavior if used insecurely; only for nodes which leave the JSON-RPC port open to an attacker (this precludes most nodes on internal networks behind NAT), bind the interface to a public IP, and simultaneously leave accounts unlocked at startup.</span>

<b>Likelihood: </b><span style="font-weight: 400">Low</span>

<b>Severity: </b><span style="font-weight: 400">High</span>

<b>Impact: </b><span style="font-weight: 400">Loss of funds related to wallets imported or generated in clients </span>

<b>Details:</b>

<span style="font-weight: 400">It’s come to our attention that some individuals have been bypassing the built-in security that has been placed on the JSON-RPC interface. The RPC interface allows you to send transactions from any account which has been unlocked prior to sending a transaction and will stay unlocked for the entirety of the the session. </span>

<span style="font-weight: 400">By default, RPC is disabled, and by enabling it it is only accessible from the same host on which your Ethereum client is running. By opening the RPC to be accessed by anyone on the internet and not including a firewall rules, you open up your wallet to theft by anybody who knows your address in combination with your IP.</span>

&nbsp;

<b>Effects on expected chain reorganisation depth: </b><span style="font-weight: 400">none</span>

<b>Remedial action taken by Ethereum</b><span style="font-weight: 400">: eth RC1 will be fully secure by requiring explicit user-authorisation for any potentially remote transaction. Later versions of Geth may support this functionality.</span>

<b>Proposed temporary workaround:</b><span style="font-weight: 400"> Only run the default settings for each client and when you do make changes understand how these changes impact your security.</span>

&nbsp;

<b>NOTE: This is not a bug, but a misuse of JSON-RPC.</b>

&nbsp;

<b>ADVISORY: Never enable JSON-RPC interface on an internet-accessible machine without a firewall policy in place to block the JSON-RPC port (default: 8545).</b>

&nbsp;

<b>eth: </b><span style="font-weight: 400">Use RC1 or later.</span>

&nbsp;

<b>geth:</b><span style="font-weight: 400"> Use the safe defaults, and know security implications of the options.</span>

<span style="font-weight: 400">--rpcaddr  "127.0.0.1". This is the default value to only allow connections originating on the local computer; remote RPC connections are disabled</span>

<span style="font-weight: 400">--unlock. This parameter is used to unlock accounts at startup to aid in automation. By default, all accounts are locked</span>