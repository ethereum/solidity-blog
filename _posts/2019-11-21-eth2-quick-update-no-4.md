---
layout: post
published: true
title: 'eth2 quick update no. 4'
date: '2019-11-21'
author: Danny Ryan
---

![Eth wallpaper](https://blog.ethereum.org/img/2019/10/kumiko_background.jpeg)

Welcome to the fourth installment of _eth2 quick update_. There are a lot of moving pieces to talk about this week. Other than the heroic eth2 client development going on, these are the highlights:

tldr;

* Grant to Sigma Prime for [eth2 differential fuzzing](https://github.com/sigp/beacon-fuzz/)
* [Light Client Task Force](https://medium.com/chainsafe-systems/ethereum-light-client-task-force-go-99485011b095) is formed
* Some notes on the uni-directional bridge between eth1 and eth2

## Differential fuzzing grant

Sigma Prime has been awarded a grant to lead the [differential fuzzing effort](https://github.com/sigp/beacon-fuzz/) for eth2 clients. This effort is critical to the success of launching a multi-client network by aiding in catching consensus issues prior to mainnet.

The act of "fuzzing" is the act of throwing many random inputs at a piece of software to see how it reacts. When fuzzing a single piece of software, the goal is often to find inputs that lead to unexpected crashes. When we find such inputs, we then figure out what went wrong and harden the software to this type of input. 

_Differential_ fuzzing is a bit different. Instead of explicitly looking for crashes, we look for instances in which different implementations of a protocol have a different output for the same input. In a blockchain context, we use differential fuzzing to find cases in which a series of blocks leads to a different resulting state on two different clients. Ideally in production there are no such cases.

## Light client task force

Chainsafe/[Lodestar](https://github.com/ChainSafe/lodestar), the recipients of an Ethereum Foundation grant for research and development on eth2 light clients, has formed the [Light Client Task Force](https://medium.com/chainsafe-systems/ethereum-light-client-task-force-go-99485011b095). This group has tasked themselves with ensuring that light clients are first class citizens in eth2. To this end, they are hosting a [monthly call](https://github.com/ChainSafe/lodestar/issues/555) aimed at driving light client research, standards, specs, and education.

The need for a rich ecosystem of light clients and light client servers is only amplified in a sharded protocol like eth2. Even if a client is syncing some subset of the protocol (e.g. just a couple of shards), a user will very often need to get information about accounts, contracts, and the general state of things on another shard. A client could inefficiently sync the entire additional shard, but more often than not, lightly requesting information about specific accounts on the shard with succinct proofs will be the way to go.

Tune in to the next [Light Client Task Force call](https://github.com/ChainSafe/lodestar/issues/555) to stay up-to-date on all things light in eth2.

## eth1 -> eth2

In the early days of eth2, the transfer of ether from the existing ethereum chain (eth1) into the new beacon chain (eth2) will be uni-directional. That is, the ether moved into staking on eth2 will not be transferable (to start) back to eth1. The choice of a single directional transfer into validation is in an effort to minimize the risk profile that eth2 induces upon eth1, and to allow for a quicker development cycle on eth2 without having to fork eth1 in the process. There is some movement around creating a bi-directional bridge, but I'll save discussion of the bridge mechanics and the trade-offs for a later post. Today, I'd like to get more into _how_ this uni-directional transfer works and how it can be safely implemented without changing eth1.

On the existing ethereum PoW chain, we will deploy the [eth2 validator contract](https://github.com/ethereum/eth2.0-specs/blob/master/deposit_contract/contracts/validator_registration.v.py). This contract has a single function called `deposit` which takes in a number of parameters to initialize a new validator (e.g. private key, withdrawal credentials, an ETH deposit, etc). There is no `withdrawal` function on this contract. Barring a fork to add in a bi-directional bridge, this deposited ETH now only exists in eth2 on the beacon chain.

It is then the validators' responsibility on the beacon chain to come to consensus on the state of this contract such that new deposits can be processed. This is done by eth2 block proposers embedding recent eth1 data into a beacon block field called `eth1_data`. When enough block proposers during a voting period agree on recent `eth1_data`, this data is enshrined in the beacon chain state allowing for new deposits to be processed.

An important note about this mechanism is that the `eth1_data` is deep in the eth1 PoW chain -- ~1000 blocks of "follow distance". This follow distance induces a high latency in processing new validator deposits, but provides a high degree of safety in the coupling of these two systems. The eth1 chain would have to re-org deeper than 1000 blocks to break the link, and in such a case would require some manual intervention to overcome.

We are researching and prototyping the utilization the beacon chain to finalize eth1 (i.e. the finality gadget). This would require eth1 to defer its fork choice ultimately to the beacon chain, gaining security from the PoS validators, and allowing for a much quicker eth1 to eth2 deposits. The finality gadget also opens up other fun things such as the bi-directional bridge and exposing the eth2 data-layer to eth1. More on all of this in a later post :rocket:.

