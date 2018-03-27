---
id: 3791
title: 'Geth 1.6 &#8211; Puppeth Master'
date: 2017-04-14T13:19:30+00:00
author: Péter Szilágyi
layout: post
guid: https://blog.ethereum.org/?p=3791
permalink: /2017/04/14/geth-1-6-puppeth-master/
---
<a href="https://blog.ethereum.org/wp-content/uploads/2017/04/puppeth.png"><img class="alignright wp-image-3793" src="https://blog.ethereum.org/wp-content/uploads/2017/04/puppeth.png" alt="puppeth" width="130" height="180" /></a>Cramming in a lot more goodies than originally anticipated, the Go Ethereum team is proud to finally deliver the first incarnation of the 1.6 Geth release series! Glimpsing through the [commit list](https://github.com/ethereum/go-ethereum/milestone/28?closed=1), this is going to be a monster post!

# Toml configuration files

For a very long time now, people were requesting that we support configuration files to aid in switching between different public and pirate chains. We didn't focus much on private networks, so we've always pointed people towards using shell scripts in those cases. Although they are suitable for many scenarios, shell scripts aren't portable across different operating systems, especially Unix and Windows.

By popular demand, we've reworked the entire configuration and flag handling for Geth 1.6, finally landing support for this much requested feature. The configuration file uses the [TOML](https://github.com/toml-lang/toml) format and the fields that can be set map one-to-one to the config structures `go-ethereum` uses internally. Certain fields have been omitted to prevent sensitive data circulating in configuration files.

Figuring out how to create a "proper" config file for your node with all the correct fields set up just the way you'd like it - whatever the field names even are - can be daunting. Instead, Geth 1.6 ships with a command that you can invoke to print the exact config file you'll need based on your custom invocation flags (`geth --your-favorite-flags dumpconfig`). This dump can then be loaded up via `geth --config yourfile.toml`. We're also planning to allow embedding the genesis configurations, but that needs a bit more thought to finalize.

# Pure Go ethash

Since the very beginning, Ethereum had a [C ethash](https://github.com/ethereum/ethash) implementation, shared between the various clients. This helped reduce maintenance efforts while the algorithm was evolving, at the expense of adding a big chunk of non trivial C code to `go-ethereum`.

This began to hurt more and more as time progressed. Without C knowledge on the Go Ethereum team, we couldn't properly handle performance issues and inherently couldn't implement optimizations essential for mobile devices. The design of the C ethash prevents it from running on big endian processors that would require invasive redesign. Lastly, depending on C code significantly hurts our portability and maintainability.

Starting with Geth 1.6, we've deprecated our use of the original C ethash and reimplemented it fully in Go. This permits us to generate mining DAGs concurrently on all available CPUs, rotate old mining DAGs out instead of storing them indefinitely, use memory mapped verification caches essential for fast mobile startup times, and run on any platform Go supports. For details, performance numbers and CLI flags, please consult the [pure Go ethash PR](https://github.com/ethereum/go-ethereum/pull/3750).

## Router CPU compatibility

We've always taken pride in supporting the `go-ethereum` codebase on a variety of exotic platforms, which were severely limited by the weight of a full node. However, with the light client slowly maturing, it makes more and more sense to expand our platforms towards previously unfeasible directions.

With the 1.6 release family of Geth, we are further pushing the envelope by introducing support for MIPS CPUs, most commonly found in consumer router devices. Geth 1.6 runs on both 64 bit and 32 bit MIPS CPUs in both little endian and big endian mode (courtesy of our new ethash code). You can find pre-built stable and develop binaries for these MIPs CPUs on our [downloads page](https://geth.ethereum.org/downloads/) from now on.

Although we've successfully put a [consumer router on the mainnet](https://twitter.com/peter_szilagyi/status/839489351461531649), there's still much to do make the experience smooth. The most notable bottleneck is the memory hardness of ethash, requiring non-negligible amounts of storage and memory capacity... one more reason to switch to PoS! ;)

# Plugable consensus engines

Over the last few releases we've been slowly working towards abstracting consensus related concepts in our codebase, with the long term goal of preparing for Casper and proof-of-stake. It was a lower priority feature as there were so many other tasks hanging around. Priorities changed when the Ropsten testnet was spammed to an unusable state.

The root cause of the attack's success was that a proof-of-work network is only as secure as the computing capacity placed behind it. Restarting a new testnet from zero wouldn't solve much, since an attacker could mount the same attack over and over again. The time seemed ripe to put some effort into the consensus model within `go-ethereum` and allow using alternatives to proof-of-work for smaller networks, where a different security model is more suitable.

The result is that Geth 1.6 features a plugable consensus model where developers, wanting to roll their own fork of Ethereum with wildly different ways of agreeing on block validity, can now do so by implementing a simple [Go consensus engine interface](https://godoc.org/github.com/ethereum/go-ethereum/consensus#Engine). The current ethash backed proof-of-work consensus model is also ["just" another implementation](https://godoc.org/github.com/ethereum/go-ethereum/consensus/ethash) of this interface.

## Clique PoA (proof-of authority) protocol

With the old Ropsten testnet down the drain, different groups scrambled to create a new testnet that would suit them, but may or may not suit others much. These efforts<strong> </strong>gave birth to a reverted Ropsten testnet from the Parity team (relying on custom soft-fork code, bootnodes and blacklists) and the Kovan testnet from a Parity conglomerate (relying on a then undocumented Parity consensus engine). Both solutions lock out other clients.

Our solution was to propose a cross client proof-of-authority consensus engine called [Clique (EIP 255)](https://github.com/ethereum/EIPs/issues/225). The main design considerations behind Clique were simplicity (i.e. minimize the burden of adding to different clients) and compatibility (i.e. support existing sync techs in **all** clients). The proposal was accepted by all client developers on the [17th of March, 2017](https://github.com/ethereum/pm/blob/master/All%20Core%20Devs%20Meetings/Meeting%2012.md).

With our 1.6.0 release, `go-ethereum` also features a [full implementation](https://godoc.org/github.com/ethereum/go-ethereum/consensus/clique) of the Clique proof-of-authority consensus engine. In addition to all the consensus related functionality, the engine also supports customizable block times for private network scenarios. As to how you can create your own Clique network... ;)

# Puppeth network manager

Do you *like* setting up a private network? Don't answer that! Truth be told, if you've ever tried to set up your own private Ethereum network - whether for friendly fun, corporate work, or hackathon aid - you'll certainly know the pain it takes to do so. Configuring a genesis block is one thing, but when you get to bootnodes, full nodes, miners and light clients, things start to wear thin fast... and we haven't even talked about monitoring, explorers, faucets, wallets. It's a mess.

Geth 1.6 ships a new tool called `puppeth`, which aims to solve this particular pain point. Puppeth is a CLI wizard that aids in creating a new Ethereum network down to the genesis, bootnodes, signers, ethstats, faucet, dashboard and more, without the hassle that it would normally take to configure all these services one by one. Puppeth uses ssh to dial into remote servers, and builds its network components out of docker containers using docker-compose. The user is guided through the process via a command line wizard that does the heavy lifting and topology configuration automatically behind the scenes.

Puppeth is not a magic bullet. If you have large in-house Ethereum deployments based on your own orchestration tools, it's always better to use existing infrastructure. However, if you need to create your own Ethereum network without the fuss, Puppeth might actually help you do that... fast. Everything is deployed into containers, so it will not litter your system with weird packages. That said, it's Puppeth's first release, so tread with caution and try not to deploy onto critical systems.

## Rinkeby test network

As mentioned in this post already, the Ropsten testnet fell apart a few months back. There are [ongoing efforts](https://github.com/ethereum/ropsten/blob/master/revival.md) to revive it as it's a valuable component of the Ethereum ecosystem. That said, we find it essential to provide developers with a network they can rely on, one that cannot be attacked so easily. One such network is the second half of the [EIP 255]((https://github.com/ethereum/EIPs/issues/225)) proposal, the Rinkeby testnet.

On the 10th of April we launched the *alpha* version of Rinkeby, a new proof-of-authority testnet based on the Clique protocol. Rinkeby is currently upheld by three (3) Foundation signing nodes, but we eagerly look forward to promote external entities too, so that the network's resiliency may be furthered. The network also features a public GitHub authenticated faucet that is accessible to everyone under the same conditions. As to why it's an *alpha* version, Rinkeby is the first live incarnation of Clique and we have yet to see how it fares under global load.

So, how can you access it? Being alpha, we didn't yet add a flag for it into Geth 1.6, however we went out of our way to make it beyond trivial do connect and use it... via it's own website at [https://www.rinkeby.io/](https://www.rinkeby.io/)! Dashboard, ethstats, faucet and connectivity tutorials for Geth, Wallet/Mist and Android/iOS! If you're wondering how we made all this, Puppeth of course! This **is** what Puppeth was born for, and you can have the same for your own private networks too!

# Mounting swarm data

If you've used our experimental swarm implementation before, you'll know that operating with files is easy enough, but when it comes to working with entire folders - possibly nested - it can become cumbersome. Fine for a program, but less so for manual user interaction.

To try and address this shortcoming, the Swarm implementation shipped with our current release features a few milestone features, notably the ability to upload and download entire directories via tarball streams in the HTTP interface, as well as mounting an entire folder into your local filesystem via FUSE! This should make Swarm a lot more viable both for file backup purposes as well as for easily exploring complex directory structures.

Apart from these neat features, Swarm saw various stability improvements and bugfixes, gained the ability to manipulate manifests via the command line, as well as to create listings/sitemaps out of them.

# 64 bit gas calculations

The Ethereum Yellow Paper specifies that gas included with transactions can be an arbitrary value up to the ludicrous amount of 2^256 (which could cover almost as many transactions as the number of atoms in the known universe). As such, the `go-ethereum` EVM was implemented to work with these insanely large numbers for gas calculations, causing<strong> </strong>equally large performance penalties while running every transaction.

As there is simply no meaningful reason to use big-number arithmetic for gas calculations, Geth 1.6 switched over to working with 64-bit values, which is a native hardware-supported type on any modern CPU. If you are wondering whether this would pose any limitation, a single block with a gas limit of 64-bits could fit in 44 times more transactions than the number of red blood cells in the human body. I think we're safe for the foreseeable future.

Regarding performance improvement, we don't have an exact number, but it is safe to say that shaving off hundreds of memory allocations per every single transactions can't be a bad thing.

# Closing remarks

Besides all the highlighted features mentioned above, a<strong> </strong>numerous number of bug fixes have also been merged in, ranging from ethstats reporting, to singleton miner networks, star topology propagation fixes and more. Please check the [Geth 1.6.0 release notes](https://github.com/ethereum/go-ethereum/releases/tag/v1.6.0) for a condensed rundown.

Other smaller features include constant improvements to the light protocol, heavy development in the Whisper protocol (getting very close to a public v5 release), and we've even swapped out our entire logging system to a much nicer and more robust version.

As always, you can install Geth via your [favorite package manager](https://geth.ethereum.org/install/), or download a pre-built binary for a [variety of supported platforms](https://geth.ethereum.org/downloads/).

Happy puppetheering! The go-ethereum Authors.

---

# IMPORTANT NOTICE
Given the nature of this release, which includes heavy rework around gas calculations in the Ethereum virtual machine and heavy refactors around plugable consensus engine split, we ask <em>production</em> users to exercise caution and care when upgrading. We consider Geth 1.6 as a pre-release until large scale stability is confirmed.*

It is important for all <em>production</em> users to understand the risks involved in new releases such as this one. There may be undetected bugs and unexpected consequences that could lead to loss or other unwanted results. This release is for sophisticated production users who understand the Geth platform, the impact it may have on the users and the risks that new releases of this nature involve.