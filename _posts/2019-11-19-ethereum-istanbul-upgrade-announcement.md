---
layout: post
published: true
title: Ethereum Istanbul Upgrade Announcement
date: '2019-11-19'
author: Hudson Jameson
---

The Ethereum network will be undergoing a scheduled upgrade at **block number 9,069,000**, which was predicted to occur on **Wednesday, December 4, 2019** and is now predicted to occur on **Friday, December 6, 2019**. The exact date is subject to change due to variable block times. 

### **Please upgrade your node before Sunday, December 1, 2019 to account for the variable block times.**

[ethernodes.org](https://ethernodes.org) has kindly provided Istanbul node statistics and a countdown timer located at <https://ethernodes.org/istanbul>.

# What is Istanbul?

Istanbul is the name given to this network upgrade. Previous network upgrades have been given other names such as [Costantinople](https://blog.ethereum.org/2019/02/22/ethereum-constantinople-st-petersburg-upgrade-announcement/), [Spurious Dragon](https://blog.ethereum.org/2016/11/18/hard-fork-no-4-spurious-dragon/), and [Byzantium](https://blog.ethereum.org/2017/10/12/byzantium-hf-announcement/).

# As an Ethereum user or ether holder is there anything I need to do?

If you use an exchange (such as Coinbase, Kraken, or Binance), a web wallet service (such as Metamask, MyCrypto, or MyEtherWallet), a mobile wallet service (such as Coinbase Wallet, Status.im, or Trust Wallet), or a hardware wallet (such as Ledger, Trezor, or KeepKey) you do not need to do anything unless you are informed to take additional steps by your exchange or wallet service.

# As a node operator or miner, what do I need to do?

Download the latest version of your Ethereum client:

-   [Latest geth client (v1.9.7)](https://github.com/ethereum/go-ethereum/releases/tag/v1.9.7)

-   [Latest Parity client (v2.5.10-stable)](https://github.com/paritytech/parity-ethereum/releases/tag/v2.5.10)

-   [Latest Besu client (v1.3.4)](https://github.com/hyperledger/besu/releases/tag/1.3.4)

-   [Latest Nethermind client (v1.1.8)](https://github.com/NethermindEth/nethermind/releases/tag/1.1.8)

-   [Latest Trinity client (v0.1.0-alpha.30)](https://github.com/ethereum/trinity/releases/tag/v0.1.0-alpha.30)

-   [Twitter thread on the C++ ecosystem Istanbul updates: Aleth/EVMC/evmone](https://twitter.com/chfast/status/1196353053886210048?s=20)

NOTE: Harmony (ethereumJ) is now a deprecated client and will not be supporting Istanbul. Besu is a mainnet compatible Ethereum client written in Java.

# What happens if I am a miner or node operator and I do not participate in the upgrade?

If you are using an Ethereum client that is not updated to the latest version (listed above), your client will sync to the pre-fork blockchain once the upgrade occurs. You will be stuck on an incompatible chain following the old rules and you will be unable to send ether or operate on the post-upgrade Ethereum network.

# What is a network upgrade in Ethereum-land?

A network upgrade is a change to the underlying Ethereum protocol, creating new rules to improve the system. The decentralized nature of blockchain systems makes a network upgrade more difficult. Network upgrades in a blockchain require cooperation and communication with the community, as well as with the developers of the various Ethereum clients in order for the transition to go smoothly.

# What happens during a network upgrade?

After the community comes to an agreement concerning which changes should be included in the upgrade, changes to the protocol are written into the various Ethereum clients, such as geth, Parity, and Nethermind. The protocol changes are activated at a specific block number. Any nodes that have not been upgraded to the new ruleset will be abandoned on the old chain where the previous rules continue to exist.

# What changes are going into Istanbul?

Changes that are implemented in Istanbul are defined using EIPs. Ethereum Improvement Proposals (EIPs) describe standards for the Ethereum platform, including core protocol specifications, client APIs, and contract standards. Please [read this article from the Ethereum Cat Herders](https://medium.com/ethereum-cat-herders/istanbul-testnets-are-coming-53973bcea7df) that explains the EIPs going into Istanbul and the Ethereum testnets that are being upgraded to Istanbul.

# Thank You!

**A big thanks to the Ethereum community and to all Ethereum developers across all clients and platforms who came together to provide input, thoughts, and contribution.**

**DISCLAIMER**: This is an emergent and evolving highly technical space. If you choose to implement the recommendations in this post and continue to participate, you should make sure you understand how it impacts you. You should understand that there are risks involved including but not limited to risks like unexpected bugs. By choosing to implement these recommendations, you alone assume the risks of the consequences. This post and recommendations are not a sale of any kind and do not create any warranties of any kind including but not limited to any relating to the Ethereum network or the Ethereum clients referred to herein.
