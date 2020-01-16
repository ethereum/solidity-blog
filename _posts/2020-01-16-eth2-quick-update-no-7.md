---
layout: post
published: true
title: 'eth2 quick update no. 7'
date: '2020-01-16'
author: Danny Ryan
---

![Eth wallpaper](https://blog.ethereum.org/img/2019/10/kumiko_background.jpeg)

Welcome to the first eth2 quick update of 2020! This is going to be an exciting year.

tldr;

* Release of [`v0.10.0` spec](https://github.com/ethereum/eth2.0-specs/releases/tag/v0.10.0) as stable target for multi-client testnets and security reviews
* @paulhauner and @sigp_io team [hard at work](https://twitter.com/paulhauner/status/1217349576278999041) building Lighthouse
* [Relaunch](https://medium.com/prysmatic-labs/eth-2-0-dev-update-42-mainnet-capable-testnet-now-hiring-53d4d08d3901) of [Prysm testnet](https://prylabs.net/), now with aggregators and mainnet configuration
* A new proposal for an expedited merging of eth1+eth2 (aka [Phase 1.5](https://ethresear.ch/t/alternative-proposal-for-early-eth1-eth2-merge/6666))

## Release of `v0.10.0` for security reviews and multi-client testnets

[`v0.10.0` -- 404 Not Found](https://github.com/ethereum/eth2.0-specs/releases/tag/v0.10.0) was released last week. Read the [release notes](https://github.com/ethereum/eth2.0-specs/releases/tag/v0.10.0) for the technical details (integration of IETF BLS, simpler eth1 caching, etc), but what does it actually mean for eth2?

`v0.10.0` is the culmination of changes introduced to the Phase 0 spec after the unfreezing following deVcon as well as the integration of the new IETF BLS standards. For some time, these new spec changes disrupted the smooth development cycle of the eth2 client teams and postponed the release of testnets that were imminent. With the release of `v0.10.0`, we are now entering back into a smooth rhythm. 

### New and updated resources

With the release of `v0.10.0`, we once again have a very stable target both for client testnets as well as third party audits and security reviews.

To this end, we recently updated a number of onboarding resources (and created a few new ones). If you are looking to better understand the eth2 Phase 0 specs, you should definitely give these a look!

* [Phase 0 for Humans](https://notes.ethereum.org/@djrtwo/Bkn3zpwxB)
* [Eth2 Design Rational](https://notes.ethereum.org/@vbuterin/rkhCgQteN)
* [Phase 0 Design Notes](https://notes.ethereum.org/EbCbXn_BTuuUYqOaLE3iGA?view)
* [Validator Lifecycle doc](https://notes.ethereum.org/@hww/lifecycle)
* [State transition and SSZ diagrams for optimized spec](https://github.com/protolambda/eth2-docs#timeline-concept)
* [Discussion of attestation gossip and aggregation strategies](https://notes.ethereum.org/@hww/aggregation)

### Audits underway

Least Authority kicked off their security review this Monday and are currently digging deep. We have high expectations and are enthusiastic to be working with a team of such caliber!

Another exciting piece of news is in the form of an audit and formalization of the Phase 0 cryptoeconomics led by a new EF team -- Robust Incentives Group (RIG).

RIG has constructed a [cadCAD](https://github.com/BlockScience/cadCAD) environment to simulate the eth2 economic models and explore a diverse range of attacks: including timing attacks, the potential effects of different sized cartels, and more!

Check out [the core of their work](https://github.com/ethereum/rig/blob/master/eth2economics/code/beaconrunner/beacon_runner.ipynb) in the form of an easy to understand python notebook.

### What's next?

While I'll just give you a glimpse here, I promise to dive into more detail in subsequent posts.

In parallel to ongoing audits, clients will code up to `v0.10.0`, integrate new BLS standards, and get some steady testnets running. Once initial stability in `v0.10.0` is achieved, multi-client activity will begin. 

With respect to this activity, I first expect a small number of multi-client tests against largely single-client testnets. By this I mean I expect to see some clients joining what were previously single-client testnets -- as minority nodes -- to test initial interopability.

Once we have success here, we'll orchestrate a shared genesis with 2+ clients from the start. I expect some level of chaos and a lot of learning in this phase. Ambiguities in the spec might become apparent with minor clarifying changes integrated. We might even find out something was broken that we previously thought was fine -- large testnets are their own form of audit.

Once audit results begin to roll in throughout February, we expect to make a wave of changes to the Phase 0 spec. The extent of these changes and the implications they might have on client development and timelines is still to be determined. Regardless, a post-audit `v0.11.0` release will drop near the start of March.

If the changes are small, clients will integrate the changes, patch testnets, and continue forward. If deeper, the integrations might take longer, require additional testing, and full restarts to existing nets. After sufficient time at a stable `v0.11.x`, a final `v1.0.0` will be cut for mainnet release: the exact length of time will depend on the depth of the changes.

This is a high level look at the coming months. I'll keep you updated and fill in more details as we go.

## Lighthouse on an optimization tear

Although Sigma Prime has been relatively quiet since they [took down their first public testnet](https://lighthouse.sigmaprime.io/update-20.html), the team has been hard at work! Lighthouse lead, Paul Hauner, just dropped a [exciting tweetstorm](https://twitter.com/paulhauner/status/1217349576278999041) filled with juicy details of recent optimizations and gains on their eth2 client.

Apparently they've been running a [not-so-publicized](https://twitter.com/paulhauner/status/1217349579579916288) public testnet for the past 4 weeks making impressive gains on everything from BLS to block processing to DB reads/writes. [Pull it down and try it yourself](https://lighthouse-book.sigmaprime.io/become-a-validator.html)!

Oh and if you have some frontend chops and want to get involved, Sigma Prime just put out an [RfP for a Lighthouse User Interface](https://lighthouse.sigmaprime.io/ui-rfp.html). This kind of work is important to ensure that validating is readily accessible to all types of users.

## Prysmatic's testnet relaunches with mainnet config and aggregators

Prysmatic Labs recently [restarted their public testnet](https://medium.com/prysmatic-labs/eth-2-0-dev-update-42-mainnet-capable-testnet-now-hiring-53d4d08d3901) now with the mainnet configuration and the attestation aggregation strategy! As discussed in prior posts, the mainnet configuration has larger caches, longer epochs, and is generally a bit more hefty than the initially used minimal config. Operating a stable testnet with this configuration and 10s of thousands of validators is an important milestone that Prysmatic is currently working through.

If you want to get your hands dirty, [become a validator](https://prylabs.net/) or [join the conversation](https://discord.gg/upbrAU7) on discord.

Oh, and [they're hiring](https://twitter.com/raulitojordan/status/1215689700150775808)! Working remotely with Prysm is a great opportunity for an experienced dev. Help build the future!

## Phase 1.5

Over the holidays, Vitalik posted a [new proposal](https://ethresear.ch/t/alternative-proposal-for-early-eth1-eth2-merge/6666) on how we might expedite the eth1+eth2 merger and begin reaping the benefits of the new eth2 infrastructure much sooner. This proposal suggests migrating eth1 into a shard of eth2 after the Phase 1 infrastruture (shard data chains) is added but before a full Phase 2 (featuring extensible EEs and all the cross-shard bells and whistles) -- thus Phase 1.5!

A phase 1.5 has many potential benefits to the ethereum protocol and to its developers and users.

1. Eth1 living inside of eth2 allows for native access to the scalable sharded data layer. Some of the most exciting constructions being built on ethereum today are layer 2 protocols that scale with the amount of data available to layer 1. These will pair beautifully with eth2 even with just a single chain with native computation (i.e. eth1 as shard). Broadly, these constructions are called "rollups". They come in many flavors and I expect this general design space to continue to expand and be fruitful.
2. Eth1 migrating to a shard of eth2 eliminates PoW from the protocol, greatly reducing issuance and halting energy intensive mining on ethereum once and for all.
3. Finally, integrating eth1 into eth2 on an earlier schedule reduces the amount of moving parts -- unifying the system, the community, and the development of the core protocol. Although the eth2 infrastructure is being developed first in parallel to the existing ethereum chain, an earlier integration of eth1 into eth2 (beyond just the technical wins) helps ensure the community of protocol developers, application developers, random contributors, and end-users remains united within a single, cohesive Ethereum.

Based upon initial discussions and reactions, developers and community members are excited by this proposal. Phase 1.5 as it currently stands largely relies upon the success of two independent components -- Phase 1 of eth2 and Stateless Ethereum on eth1. The relative timelines of each of these components will inform as to how and when this proposal might come to fruition. Over the coming months, we will do due diligence on better spec'ing and understanding the technical challenges at hand so that we are prepared to move on this if/when the time is right :rocket:.
