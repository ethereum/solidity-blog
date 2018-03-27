---
id: 1851
title: 'A few last minute notes&#8230;'
date: 2015-07-30T12:04:20+00:00
author: Stephan Tual
layout: post
guid: https://blog.ethereum.org/?p=1851
permalink: /2015/07/30/a-few-last-minute-notes/
dsq_thread_id:
  - "3986227102"
---
Genesis is roughly <a href="http://frontier-countdown.meteor.com/">4 hours</a> away, here are a few last minute pointers for those preparing:

* All clients are functional on all platforms and build to completion: <a href="http://ethereum-buildboard.meteor.com/">http://ethereum-buildboard.meteor.com/</a>

* Please note that it will take a little while (10-1000 blocks) before the network stabilises. This is a marathon, not a sprint. For this reason we will not make the Genesis block hash known until a solid consensus has been formed. We will not announce the --extradata immediately either - if we did, we might as well launch centrally. We'll let the community come to a consensus on what it is (as there could be multiple blocks #1028201 on the testnet, while it undergoes a re-org, or multiple re-orgs). 

* Do not reuse keys from Olympic, especially if you are expecting Olympic rewards, as these should be considered completely insecure due to the development state of the various codebases that generated them. They may also be vulnerable to replay attacks. If you are expecting an Olympic reward for a key A, please create new key B with pyethereum, cpp-ethereum-0.9.36+ or go-ethereum-1.0 and place a transaction sent by A to B on the testnet; rewards will be given to the first such transaction following block #1026900. Don't use B on Olympic. If you have 'won' something, it will get sent to B on Frontier.

* Difficulty will start at 2^34, this number was decided on based on what we observed on the testnet

See you at Genesis!