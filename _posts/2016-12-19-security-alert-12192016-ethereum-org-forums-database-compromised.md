---
id: 3629
title: 'Security alert [12/19/2016]: Ethereum.org Forums Database Compromised'
date: 2016-12-19T21:54:25+00:00
author: Hudson Jameson
layout: post
guid: https://blog.ethereum.org/?p=3629
permalink: /2016/12/19/security-alert-12192016-ethereum-org-forums-database-compromised/
category: Security
---
On December 16, we were made aware that someone had recently gained unauthorized access to a database from [forum.ethereum.org](http://forum.ethereum.org/). We immediately launched a thorough investigation to determine the origin, nature, and scope of this incident. Here is what we know:
<ul>
 	<li>The information that was recently accessed is a database backup from April 2016 and contained information about 16.5k forum users.</li>
 	<li>The leaked information includes
<ul>
 	<li>Messages, both public and private</li>
 	<li>IP-addresses</li>
 	<li>Username and email addresses</li>
 	<li>Profile information</li>
 	<li>Hashed passwords
<ul>
 	<li>~13k bcrypt hashes (salted)</li>
 	<li>~1.5k Wordpress-hashes (salted)</li>
 	<li>~2k accounts without passwords (used federated login)</li>
</ul>
</li>
</ul>
</li>
 	<li>The attacker self-disclosed that they are the same person/persons who <a href="http://www.coindesk.com/hackers-stole-300k-blockchain-investor/">recently hacked Bo Shen</a>.</li>
 	<li><span style="font-weight: 400;">The attacker used social engineering to gain access to a mobile phone number that allowed them to gain access to other accounts, one of which had access to an old database backup from the forum.</span></li>
</ul>
We are taking the following steps:
<ul>
 	<li>Forum users whose information may have been compromised by the leak will be receiving an email with additional information.</li>
 	<li>We have closed the unauthorized access points involved in the leak.</li>
 	<li>We are enforcing stricter security guidelines internally such as removing the recovery phone numbers from accounts and using encryption for sensitive data.</li>
 	<li>We are providing the email addresses that we believe were leaked to <a href="https://haveibeenpwned.com">https://haveibeenpwned.com</a>, a service that helps communicate with affected users.</li>
 	<li>We are resetting all forum passwords, effective immediately.</li>
</ul>
If you were affected by the attack we recommend you do the following:
<ul>
 	<li>Ensure that your passwords are not reused between services. If you have reused your forum.ethereum.org password elsewhere, change it in those places.</li>
</ul>
Additionally, we recommend <a href="http://blog.kraken.com/post/153209105847/security-advisory-mobile-phones">this excellent blog post by Kraken</a> that provides useful information about how to protect against these types of attacks.

We deeply regret that this incident occurred and are working diligently internally, as well as with external partners to address the incident.

Questions can be directed to security@ethereum.org.