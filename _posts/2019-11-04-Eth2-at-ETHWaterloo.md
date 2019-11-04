---
layout: post
published: true
title: 'Eth2 at ETHWaterloo: Prizes for Eth2 education, tooling, and research'
date: '2019-11-04'
author: Ethereum Foundation Team
---

For the first time ever, the Ethereum Foundation will be sponsoring a range of hacker prizes related to Eth2 at a major hackathon.

With Eth2 development proceeding rapidly, there are now many discrete areas of work that a team of motivated hackers can complete over a weekend. The bounties below include useful educational tools for Eth2 development, necessary tooling, and valuable research.

Members of Eth2 teams will be available remotely during the hackathon to answer questions from hackers and provide advice related to these prizes. 

The EF will offer 5 prizes of $1,000 each, for projects that tackle any (or more than one!) of  the following ideas:


## Education
- **An interactive, educational visualization of the [Beacon Chain](https://media.consensys.net/state-of-ethereum-protocol-2-the-beacon-chain-c6b6a9a69129)**
- **Help document/visualize the architecture of your [favourite eth2 client](https://eth.wiki/en/eth2/clients)**
- **Visualize the cutting edge of [phase 2](https://docs.ethhub.io/ethereum-roadmap/ethereum-2.0/eth-2.0-phases/)** (suggestion: the [phase 2 wiki](https://hackmd.io/UzysWse1Th240HELswKqVA?view) is very helpful)


## Tooling
- **Implement the proposed BLS key standards** (EIPs [2333](https://github.com/ethereum/EIPs/pull/2333), [2334](https://github.com/ethereum/EIPs/pull/2334), [2335](https://github.com/ethereum/EIPs/pull/2335)) in a language of your choice (suggestion: one of the [eth2 client languages](https://eth.wiki/en/eth2/clients))
- **Fast viz:** Given just a single BeaconState, visualize the last events and validator registry state (suggestion: use [lodestar](https://github.com/ChainSafe/lodestar) code to work in the browser)
- **Attestation surround/double vote checker** (suggestion: look at [eth2.0-pm repo issue 63](https://github.com/ethereum/eth2.0-pm/issues/63))
- **Attestation pool:** make a bare bones tool that listens on the attestation aggregation subnets and global net, and show what’s there (suggestion: start with logging received attestations)
- **Beacon node load balancer:** implement a validator API that proxies traffic to any healthy up-to-date beacon node (suggestion: take existing http proxy, and focus on the nodes health status)
- **Netstats2**: log/visualize the health status of a list of eth2 testnet nodes (suggestion: [prysm](https://github.com/prysmaticlabs/prysm) or [lighthouse](https://github.com/sigp/lighthouse) testnet, start with simple api queries like latest block)
- **Merkle multi proof builder:** check out [simpleserialize.com](http://simpleserialize.com/) and the tree visualization. This could be enhanced with checkboxes in each node to interactively create multiproofs with (suggestion: start with a simple proof encoding format - [Cayman Nava](https://twitter.com/caymannan) and [Proto](https://twitter.com/protolambda?lang=en) will be available to help on the ETHWaterloo discord)
- **Validator tracking:** a service that you can point at a validator and it notifies you if the validator is offline.


## Research
- **Take the [EE interaction design from Quilt](https://github.com/lightclient/sheth)**, and extend it with your own experiment. (Curious to know more about this? Ask us [in the Discord](http://ethwaterloo.com/chat) during the hackathon and we can point you in the right direction)
- **Model state size growth on beaconchain and shards** (suggestion: parametrize with constants config, [lodestar](https://github.com/ChainSafe/lodestar) has a [neat TS package](https://github.com/ChainSafe/lodestar/tree/master/packages/eth2.0-config))


### **Interested in other bounties or prizes offered by the Ethereum Foundation?** 

Here are a few other bounties currently live:

**Ethereum.org Gitcoin Bounties**

- We have [several open bounties available on Gitcoin](https://gitcoin.co/hackathon/web3-world), covering a variety of small updates and improvements to [ethereum.org](http://ethereum.org)


**Legendre PRF**

- The Legendre pseudo-random function is an extremely MPC-friendly one bit PRF. This PRF is currently slated to be used in the proof of custody scheme in Phase 1 of Eth 2.0, as it is the only known method to make the proof of custody possible in a shared secret setting. To encourage more research about this PRF, the Ethereum Foundation has set out a series of bounties here: [legendreprf.org/bounties](https://legendreprf.org/bounties)

**STARK-Friendly Hash Challenge**

- The Ethereum Foundation has asked StarkWare to recommend a STARK-Friendly Hash (SFH). The StarkWare hash challenge is a public competition aimed at evaluating the security of current proposed SFH candidates. The challenge is proposed at four security levels: low-security, medium-security, target-security, and high-security in multiple scenarios. See [starkware.co/hash-challenge](https://starkware.co/hash-challenge/) for more details about the competition and how to get started.

