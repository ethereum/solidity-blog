---
layout: post
published: true
title: Ethereum Constantinople/St. Petersburg Upgrade Announcement
date: '2019-02-22'
author: Hudson Jameson
---

The Ethereum network will be undergoing a scheduled upgrade at **block number 7,280,000**, which is predicted to occur on **Thursday, February 28, 2019**. The exact date is subject to change depending on block times between now and then and could be activated 1-2 days before or after. A countdown timer can be seen at <https://amberdata.io/blocks/7280000>. You can monitor the network upgrade in real time at <http://forkmon.ethdevops.io/>.

# What is Constantinople and St. Petersburg?

Constantinople and St. Petersburg are the names given to this network upgrade. Previous network upgrades have been given other names such as [Spurious Dragon](https://blog.ethereum.org/2016/11/18/hard-fork-no-4-spurious-dragon/) and [Byzantium](https://blog.ethereum.org/2017/10/12/byzantium-hf-announcement/). The reason that this network upgrade has two names is because [the original Constantinople network upgrade was postponed](https://blog.ethereum.org/2019/01/15/security-alert-ethereum-constantinople-postponement/) and two protocol upgrades will need to occur on the same block number in order to fix issues on various Ethereum test networks, such as Ropsten.

# As an Ethereum user or ether holder is there anything I need to do?

If you use an exchange (such as Coinbase, Kraken, or Binance), a web wallet service (such as Metamask, MyCrypto, or MyEtherWallet), a mobile wallet service (such as Coinbase Wallet, Status.im, or Trust Wallet), or a hardware wallet (such as Ledger, Trezor, or KeepKey) you do not need to do anything unless you are informed to take additional steps by your exchange or wallet service.

# As a node operator or miner, what do I need to do?

Download the latest version of your Ethereum client:

-   [Latest geth client (v1.8.23)](https://github.com/ethereum/go-ethereum/releases/tag/v1.8.23)

-   [Latest Parity client (v2.2.10-stable)](https://github.com/paritytech/parity-ethereum/releases/tag/v2.2.10)

-   [Latest Harmony client (v2.3 Build 74)](https://github.com/ether-camp/ethereum-harmony/releases/tag/v2.3b74)

-   [Latest Pantheon client (v0.9.1)](https://pegasys.tech/)

-   [Latest EthereumJS VM client (v2.6.0)](https://github.com/ethereumjs/ethereumjs-vm/releases/tag/v2.6.0)

-   [Latest version of Nethermind client (v0.9.4)](https://github.com/NethermindEth/nethermind/releases/tag/v0.9.4)

-   [Latest version of Trinity client (v0.1.0-alpha.23)](https://github.com/ethereum/trinity/releases/tag/v0.1.0-alpha.23)

-   [Latest version of Ethereum Wallet/Mist (v0.11.1)](https://github.com/ethereum/mist/releases/tag/v0.11.1)

# What happens if I am a miner or node operator and I do not participate in the upgrade?

If you are using an Ethereum client that is not updated to the latest version (listed above), your client will sync to the pre-network upgrade blockchain once the upgrade occurs. You will be stuck on an incompatible chain following the old rules and you will be unable to send ether or operate on the post-upgrade Ethereum network.

# What is a network upgrade in Ethereum-land?

A network upgrade is a change to the underlying Ethereum protocol, creating new rules to improve the system. The decentralized nature of blockchain systems makes a network upgrade more difficult. Network upgrades in a blockchain require cooperation and communication with the community, as well as with the developers of the various Ethereum clients in order for the transition to go smoothly.

# What happens during a network upgrade?

After the community comes to an agreement concerning which changes should be included in the upgrade, changes to the protocol are written into the various Ethereum clients, such as geth, Parity, and Harmony. The protocol changes are activated at a specific block number. Any nodes that have not been upgraded to the new ruleset will be abandoned on the old chain where the previous rules continue to exist.


# What changes are going into Constantinople?

Changes that are implemented in Constantinople are defined using EIPs. Ethereum Improvement Proposals (EIPs) describe standards for the Ethereum platform, including core protocol specifications, client APIs, and contract standards. The following EIPs will be implemented in Constantinople.

### [EIP 145: Bitwise shifting instructions in EVM](https://eips.ethereum.org/EIPS/eip-145)

-   Provides native bitwise shifting with cost on par with other arithmetic operations.

-   EVM is lacking bitwise shifting operators, but supports other logical and arithmetic operators. Shift operations can be implemented via arithmetic operators, but that has a higher cost and requires more processing time. Implementing SHL and SHR using arithmetics cost each 35 gas, while these proposed instructions take 3 gas.

-   In short: This EIP adds native functionality to protocol so that it is cheaper & easier to do certain things on chain.

### [EIP 1014: Skinny CREATE2](https://eips.ethereum.org/EIPS/eip-1014)

-   Adds a new opcode at 0xf5, which takes 4 stack arguments: endowment, memory_start, memory_length, salt. Behaves identically to CREATE, except using keccak256( 0xff ++ address ++ salt ++ keccak256(init_code)))[12:] instead of keccak256(RLP(sender_address, nonce))[12:] as the address where the contract is initialized at.

-   This allows interactions to be made with addresses that do not exist yet on-chain but can be relied on to only possibly contain code eventually that has been created by a particular piece of init code.

-   Important for state-channel use cases that involve counterfactual interactions with contracts.

-   In short: This EIP makes it so you can interact with addresses that have yet to be created.


### [EIP 1052: EXTCODEHASH opcode](https://eips.ethereum.org/EIPS/eip-1052)

-   This EIP specifies a new opcode, which returns the keccak256 hash of a contract's code.

-   Many contracts need to perform checks on a contract's bytecode, but do not necessarily need the bytecode itself. For instance, a contract may want to check if another contract's bytecode is one of a set of permitted implementations, or it may perform analyses on code and whitelist any contract with matching bytecode if the analysis passes.

-   Contracts can presently do this using the EXTCODECOPY opcode, but this is expensive, especially for large contracts, in cases where only the hash is required. As a result, a new opcode is being implemented called EXTCODEHASH which returns the keccak256 hash of a contract's bytecode.

-   In short: This EIP makes it cheaper (less gas is needed) to do certain things on chain.

### [EIP 1234: Constantinople Difficulty Bomb Delay and Block Reward Adjustment](https://eips.ethereum.org/EIPS/eip-1234)

-   The average block times are increasing due to the difficulty bomb (also known as the "ice age") slowly accelerating. This EIP proposes to delay the difficulty bomb for approximately 12 months and to reduce the block rewards to adjust for the ice age delay.

-   In short: This EIP make sure we don’t freeze the blockchain before proof of stake is ready & implemented.

# What changes are going into St. Petersburg?
Before Ethereum performs network upgrades on the main network, test networks, such as Ropsten, are upgraded to test the changes. The original Constantinople changes, listed in this blog post, were applied to test networks before the postponement and require a 2nd network upgrade to reverse the original Constantinople changes. This is called St. Petersburg and it occurs on the same block number as Constantinople.

The following EIP was removed from test networks using the St. Petersburg network upgrade:

###  REMOVING [EIP 1283: Net gas metering for SSTORE without dirty maps](https://eips.ethereum.org/EIPS/eip-1283)

# Thank You!

**A big thanks to the Ethereum community, and to all Ethereum developers across all clients and platforms who came together to provide input, thoughts, and contribution. Special thanks to Reddit user cartercarlson who let us use [his Reddit post](https://old.reddit.com/r/ethereum/comments/abv70c/heres_a_summary_of_the_constantinople_update/) and the [MyCrypto](https://mycrypto.com) team who let us use their "[Ethereum Constantinople: Everything You Need To Know](https://link.medium.com/MLw7MrPknT)" Medium post.**

**DISCLAIMER**: This is an emergent and evolving highly technical space. If you choose to implement the recommendations in this post and continue to participate, you should make sure you understand how it impacts you. You should understand that there are risks involved including but not limited to risks like unexpected bugs. By choosing to implement these recommendations, you alone assume the risks of the consequences. This post and recommendations are not a sale of any kind and do not create any warranties of any kind including but not limited to any relating to the Ethereum network or the Ethereum clients referred to herein.
