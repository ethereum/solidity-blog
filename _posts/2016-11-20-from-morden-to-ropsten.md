---
id: 3507
title: From Morden to Ropsten
date: 2016-11-20T23:03:42+00:00
author: Martin Swende
layout: post
guid: https://blog.ethereum.org/?p=3507
permalink: /2016/11/20/from-morden-to-ropsten/
---
## Testing a fork
The Spurious Dragon hardfork is scheduled for the coming week; block 2675000 is likely to occur Tuesday evening (CET). The block number for the testnet "Morden" was scheduled at block 1885000. Performing the fork in the test network prior to performing it in the main network was an important measure taken in the testing process to ensure a smooth rollover into the post-fork state.

The Morden fork occurred on Nov-20-2016, 06:12:20 +UTC, at block 1885000 as planned.  A bit later, at block 1885074, there was a consensus issue between Geth and Parity.

## Morden replay protection

The Morden testnet has been running since the launch of the Ethereum blockchain (July 2015). At that time, concerns about replay-attacks between Morden and Mainnet were addressed by using a nonce-offset. All accounts on Morden used a starting nonce of `2^20` instead of `0`, ensuring that any transaction valid on one chain would not be valid on the other.

[EIP 161](https://github.com/ethereum/EIPs/issues/161) specifies new EVM rules regarding nonces. The implementation of those rules, in combination with Morden-specific nonce-rules, resulted in Geth and Parity creating incompatible blocks at block 1885074.

## Consequences for the Main network

All issues found during the rollout of Spurious Dragon on the test network were _Morden-specific_. There are currently no known issues affecting the Mainnet.

## Starting the new “Ropsten” test network

Before the current hard forks, there were already discussions about restarting the test network from a new genesis block in order to make full syncing simpler and less resource intensive. And due to the low difficulty of the testnet, the *difficulty bomb* was already causing noticeable increases in block times, which would continue to grow if unaddressed. So the time is now right to leave Morden behind and start a new test network.

New clients will be released that use Ropsten instead of Morden as the default testnet.

Developers who want to get started with Ropsten right away can download the genesis file [here]( https://dl.dropboxusercontent.com/u/4270001/testnet_genesis.json), and start a client with the Ropsten network id:`3`

* __geth__: `geth --datadir /path/to/testnet/data init genesis.json; geth --datadir /path/to/testnet/data --networkid 3 console`
* __parity__: Download [ropsten.json](https://raw.githubusercontent.com/ethcore/parity/master/ethcore/res/ethereum/ropsten.json
), then <code>parity --chain path/to/ropsten.json</code>