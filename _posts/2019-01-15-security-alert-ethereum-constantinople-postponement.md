---
layout: post
published: true
title: Security Alert: Ethereum Constantinople Postponement
date: '2019-01-15'
author: Hudson Jameson
---

The Ethereum Core Developers and the Ethereum Security Community were made aware of the potential Constantinople-related issues identified by [ChainSecurity](https://medium.com/chainsecurity/constantinople-enables-new-reentrancy-attack-ace4088297d9) on January 15, 2019. We are investigating any potential vulnerabilities and will follow with updates in this blog post and across social media channels.

Out of an abundance of caution, key stakeholders around the Ethereum community have determined that the best course of action will be to delay the planned Constantinople fork that would have occurred at block 7,080,000 on January 16, 2019.

This will require anyone running a node (node operators, exchanges, miners, wallet services, etc...) to update to a new version of Geth or Parity before block 7,080,000. Block 7,080,000 will occur in approximately 32 hours from the time of this publishing or at approximately January 16, 8:00pm PT / January 16, 11:00pm ET / January 17, 4:00am GMT.


## What You Need To Do
If you are a person who simply interacts with Ethereum (you do not run a node), **you do not need to do anything**.

### Miners, Exchanges, Node Operators:

-   Update your Geth and/or Parity instances when they are released.

-   These releases are not released yet. We will update this post when they are available.

-   Links and version numbers and instructions will be provided here when they are available.

-   We expect to have updated releases in 3-4 hours from the time this blog is published.

#### Geth

  -   Upgrade to [1.8.21](https://github.com/ethereum/go-ethereum/releases/tag/v1.8.21) , OR

  -   Downgrade to Geth [1.8.19](https://github.com/ethereum/go-ethereum/releases/tag/v1.8.19), OR

  -   Remain on 1.8.20, but use the switch '--override.constantinople=9999999' to postpone the Constantinople fork indefinitely.

#### Parity

  -   Upgrade to Parity Ethereum [2.2.7-stable](https://github.com/paritytech/parity-ethereum/releases/tag/v2.2.7) (recommended)

  -   Upgrade to Parity Ethereum [2.3.0-beta](https://github.com/paritytech/parity-ethereum/releases/tag/v2.3.0)

  -   Downgrade to Parity Ethereum [2.2.4-beta](https://github.com/paritytech/parity-ethereum/releases/tag/v2.2.4) (not recommended)

### Everyone Else:

#### Ledger, Trezor, Safe-T, Parity Signer, WallEth, Paper Wallets, MyCrypto, MyEtherWallet and other users or token holders that do not participate in the network by syncing and running a node.

-   You do not have to do anything.

#### Contract owners

-   You do not have to do anything.

-   You may choose to examine the analysis of the potential vulnerability and check your contracts.

-   However, you do not have to do anything as the change that would introduce this potential vulnerability will not be enabled.

## Background

The article by [ChainSecurity](https://medium.com/chainsecurity/constantinople-enables-new-reentrancy-attack-ace4088297d9) dives deep into the potential vulnerability and how smart contracts can be checked for the vulnerability. Very briefly:

-   [EIP-1283](https://eips.ethereum.org/EIPS/eip-1283) introduces cheaper gas cost for SSTORE operations

-   Some smart contracts (that are already on chain) may utilize code patterns that would make them vulnerable to a re-entrancy attack after the Constantinople upgrade took place

-   These smart contracts would not have been vulnerable before the Constantinople upgrade

Contracts that increase their probability to being vulnerable are contracts that utilize a transfer() or send() function followed by a state-changing operation. An example of such a contract would be one where two parties jointly receive funds, decide on how to split said funds, and initiate a payout of those funds.

## How was the decision to postpone the Constantinople fork was made

Security researchers like ChainSecurity and TrailOfBits ran (and are still running) analysis across the entire blockchain. They did not find any cases of this vulnerability in the wild. However, there is still a non-zero risk that some contracts could be affected.

Because the risk is non-zero and the amount of time required to determine the risk with confidence is longer the amount of time available before the planned Constantinople upgrade, a decision was reached to postpone the fork out of an abundance of caution.

Parties involved in the discussions included, but were not limited to:

-   Security researchers

-   Ethereum stakeholders

-   Ethereum client developer

-   Smart contract owners / developers

-   Wallet providers

-   Node operators

-   Dapp developers

-   Media

### Response Time

**8:04am PT**

-   Original article by ChainSecurity was published around

**8:52am PT**

-   Martin Holst Swende posts in ethsecurity and AllCoreDevs Gitter channel: "Please read: https://medium.com/chainsecurity/constantinople-enables-new-reentrancy-attack-ace4088297d9 @/all We need a quick decision on potential consequences and how to move forward. We have about 37 hours left until the fork happens"

**8:52am PT - 10:15am PT**

-   Discussion occurs across various channels regarding potential risks, on-chain analysis, and what steps need to be taken

**10:15am PT - 12:40pm PT**

-   Discussion via Zoom audio call with key stakeholders. Discussion continues in gitter and other channels as well

**12:08pm PT**

-   Decision made to delay Constantinople upgrade

**1:30pm PT**

-   Public blog post released across various channels and social media

_This article was put together in a collaborative effort by EvanVanNess, Infura, MyCrypto, Parity, Status, The Ethereum Foundation, and the Ethereum Cat Herders._
