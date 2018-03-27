---
id: 1811
title: 'Frontier is coming &#8211; what to expect, and how to prepare'
date: 2015-07-22T12:49:56+00:00
author: Stephan Tual
layout: post
guid: https://blog.ethereum.org/?p=1811
permalink: /2015/07/22/frontier-is-coming-what-to-expect-and-how-to-prepare/
dsq_thread_id:
  - "3959075989"
tags:
  - frontier
---
We are only days away from launching 'Frontier', the first milestone in the release of the Ethereum project. Frontier will be followed by 'Homestead', 'Metropolis' and 'Serenity' throughout the coming year, each adding new features and improving the user friendliness and security of the platform.


## What is Frontier?

Frontier is a live, but barebone implementation of the Ethereum project. It's intended for technical users, specifically developers. During the Frontier release, we expect early adopters and application developers to establish communities and start forming a live ecosystem. Like their counterparts during the American Frontier, these settlers will be presented with vast opportunities, but will also face many dangers. If building from source and command lines interfaces aren't your cup of tea, we strongly encourage you to wait for a more user-friendly release of the Ethereum software before diving in.

## When is Frontier going live?

Frontier is now feature complete and its codebase has been frozen for two weeks. As a team we are currently reviewing the final steps for the release, not all of them technical. There will be no countdown - Ethereum is not something that's centrally 'launched', but instead emerges from consensus. Users will have to voluntarily download and run a specific version of the software, then generate and load the Genesis block to join the official project's network. Details on this process will be released soon.

&nbsp;

## What can we expect in Frontier?

### Initial 'Thawing': Gas limits during the first few days

The first software release of Frontier will have a hardcoded Gas limit per block of 5,000 Gas. Unlike the normal Gas per block parameter, this special limit will not grow proportionally to the network usage - effectively preventing transacting during the first few days. This 'thawing' period will enable miners to start their operations and early adopters to install their clients without having to 'rush'. After a few days (likely 3-4, but this could change), we'll release a small software update which all clients will have to install. This update will see the Gas limit per block raised to 3 million, an initial number from which it will expand or contract as per the default miner settings.

### Bugs, Issues and Complications

We're very happy with how the 'Olympic' testing phase of the Ethereum Testnet took shape. That said, the work on the Frontier software is far from over. Expect weekly updates which will give you access to better, more stable clients. Many of the planned Frontier gotchas (which included a chain reset at Homestead, limiting mining rewards to 10%, and centralized checkpointing) were deemed unnecessary. However, there are still big differences between Frontier and Homestead. In Frontier, we're going to have issues, we're going to have updates, and there will be bugs - users are taking their chances when using the software. There will be big (BIG) warning messages before developers are able to even install it. In Frontier, documentation is limited, and the tools provided require advanced technical skills.

### The Canary Contracts

The Canary contracts are simple switches holding a value equal to 0 or 1. Each contract is controlled by a different member of the Eth/Dev team and will be updated to ‘1’ if the internal Frontier Disaster Recovery Team flags up a consensus issue, such as a fork.

Within each Frontier client, a check is made after each block against 4 contracts. If two out of four of these contracts have a value switched from 0 to 1, mining stops and a message urging the user to update their client is displayed. This is to prevent "fire and forget" miners from preventing a chain upgrade.

This process is centralized and will only run for the duration of Frontier. It helps preventing the risk of a prolonged period (24h+) of outage.

### Stats, Status and Badblock websites

You probably are already familiar with our network stats monitor, <a href="https://stats.ethdev.com/">https://stats.ethdev.com/</a>. It gives a quick overview of the health of the network, block resolution time and Gas statistics. If you’d like to explore it further, I’ve made a <a href="https://www.youtube.com/watch?v=kRGR_De16GU">brief video</a> explaining the various KPIs. Remember that participation in the stats page is voluntary, and nodes have to <a href="https://forum.ethereum.org/discussion/2112/how-to-add-yourself-to-the-stats-dashboard-its-not-automatic">add themselves</a> before they appear on the panel.

In addition to the stats page, we will have a status page at https://status.ethdev.com/ (no link as the site is not live yet) which will gives a concise overview of any issue that might be affecting Frontier. Use it as your first port of call if you think something might not be right.

Finally, if any of the clients receive an invalid block, they will refuse to process it send it to the bad block website (AKA ‘Sentinel’). This could mean a bug, or something more serious, such as a fork. Either way, this process will alert our developers to potential issues on the network. The website itself is public and available at <a href="https://badblocks.ethdev.com">https://badblocks.ethdev.com</a> (currently operating on the testnet).

### A Clean Testnet

During the last couple of months, the Ethereum test network was pushed to its limits in order to test scalability and block propagation times. As part of this test we encouraged users to spam the network with transactions, contract creation code and call to contracts, at times reaching over 25 transactions per second. This has led the test network chain to grow to a rather unwieldy size, making it difficult for new users to catch up. For this reason, and shortly after the Frontier release, there will be a new test network following the same rules as Frontier.

### Olympic rewards distribution

During the Olympic phase there were a number of rewards for various achievements including mining prowess. A large number of you participated and earned rewards - a special mention goes to PhiStr90, Dino and Samuel Lavery for their help during the stress tests. Note that rewards will not be part of the Frontier Genesis block, but instead will be handed out by a Foundation bot during the weeks following the release.

&nbsp;

## How do I get started with Frontier?

### The Tools

Frontier and all its dependencies will be made available as a single line installer on our website at <a href="https://www.ethereum.org/">https://www.ethereum.org/</a>. A single line installer will be provided for OSX, Linux and Windows. Of course, more advanced users will still be able to install everything <a href="https://github.com/ethereum/go-ethereum">from source</a>, or use a binary build from our <a href="https://build.ethdev.com/waterfall">automated build bots</a>.

Once Frontier has been installed on their machines, users will need to generate the Genesis block themselves, then load it into their Frontier clients. A script and instructions on how to do this will be provided as part of the new Ethereum website, as well as our various wikis.

We’re often asked how existing users will switch from the test network to the live network: it will be done through a switch at the geth console (<code>--networkId</code>). By default the new build will aim to connect to the live network, to switch back to the test network you’ll simply indicate a network id of ‘0’.

### The Documentation

To get started with Ethereum, the best place is our official <a href="http://ethereum.gitbooks.io/frontier-guide/content/">gitbook. </a>After consulting the gitbook, you can dig into the official <a href="https://github.com/ethereum/wiki/wiki/Solidity-Tutorial">Solidity tutorial. </a>For more in-depth information, please consult the <a href="https://github.com/ethereum/wiki/wiki">main wiki</a>, <a href="https://github.com/ethereum/go-ethereum/wiki">go client wiki</a> and<a href="https://github.com/ethereum/cpp-ethereum/wiki"> c++ client wiki</a>. Finally, if it's mining you'd like to learn more about, a <a href="https://forum.ethereum.org/discussion/197/mining-faq-live-updates">mining FAQ and guide</a> are regularly updated on our forums.

### Getting Help

Ethereum is an open source software project and as such, all help is provided through the community channels. If you face issues, the first port of call should be our <a href="http://forum.ethereum.org/">forums</a>, followed by our <a href="https://forum.ethereum.org/discussion/1495/chat-with-the-ethereum-community-on-skype-irc-and-gitter">Ethereum chat</a> channels. If you're on the other hand are experiencing problems specific to your Ether Sale wallet, such as not being able to load your pre sale purchase, the <a href="mailto:helpdesk@ethereum.org?subject:Issue%20with%20Ethereum%20presale%20Wallet">helpdesk address</a> will continue operating throughout Frontier (and probably beyond).

And of course, you can also find help locally at one of our <a href="http://ethereum.meetup.com/">115 meetups</a> around the world - if your city isn’t listed, we encourage you to <a href="https://forum.ethereum.org/discussion/171/how-to-start-an-ethereum-meetup-faq">create one</a>.