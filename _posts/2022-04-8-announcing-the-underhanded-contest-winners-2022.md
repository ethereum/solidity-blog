---
layout: post
published: true
title: 'Announcing the Winners of the Underhanded Solidity Contest 2022'
date: '2022-04-07'
author: Franziska Heintel
category: Announcements
---

The time has come to share this year's winners of the [Underhanded Solidity Contest](https://underhanded.soliditylang.org/)!

Before we dive into the winning submissions, let's revisit the most important features of the USC:

In a nutshell, the USC is about finding loopholes or “hiding spots” in the Solidity language and using those to write seemingly innocent and straightforward-looking Solidity code which contains malicious behavior or backdoors.

The Underhanded Solidity Contest aims to...
+ Raise awareness about smart contract security.
+ Uncover language design faults.
+ Battle-test recently introduced language features and restrictions.
+ Highlight anti-patterns in smart contact development.
+ Establish new best practices for secure smart contract development.

Each contest has a different theme or topic. [This year](https://blog.soliditylang.org/2022/02/09/underhanded-solidity-contest-2022-announcement/), the task was to build a decentralized exchange that looks fair, but can be “manipulated”.

In total, we received 19 submissions, 18 of which are eligible for a “qualified submission” Underhanded Solidity POAP NFT. You can find all 18 qualified submissions in [this repo](https://github.com/ethereum/solidity-underhanded-contest/tree/master/2022/submissions_2022). 

A big thank you and props to all participants for taking part!

As always, the judges were presented with anonymized submissions and only after the judging process was finished, the identities of the participants were disclosed.

Now, without further ado, let's take a look at the winners, starting with the 3rd place!

## Underhanded Solidity Contest 2022 Winners

### 🥉 Third Place: [Michael Zhu](https://github.com/ethereum/solidity-underhanded-contest/tree/master/2022/submissions_2022/submission17_MichaelZhu)

_commentary by [samczsun](https://twitter.com/samczsun)_
 
This submission is a simple but elegant reminder that not everything is as it seems. The contract implements a very simple NFT sale mechanism where buyers can place bids and sellers can accept them. For optimization purposes, the addresses of the two assets (the NFT being purchased and the token used for purchasing) are XOR'ed together to form a single unique key. The comments helpfully remind us that the odds of finding another pair of contract addresses which collide to the same key is statistically impossible.

When a bid is accepted, ``safeTransferFrom`` is used to move the payment from the bidder to the sender, and ``transferFrom`` is used to move the NFT from the sender to the bidder. However, both ERC20 and ERC721 define a ``transferFrom`` function with the same parameters, which leads to them having the same selector. Furthermore, ``safeTransferFrom`` will succeed even if the token does not strictly comply with the ERC20 spec and returns no boolean. Finally, though it may be obvious to some, XOR is a commutative operation. Therefore, it's possible to accept a fake bid by swapping the ERC20 and ERC721 addresses when accepting a bid. This causes the NFT, rather than the payment token, to be transferred from the bidder. Clearly this is an undesirable result.

Although in its simplified form, the underhanded nature of the optimization might be easily spotted (especially in the context of an underhanded competition), we think that it acts as a good proxy for teaching users how to look past what the code and comments might suggest and read between the lines. Similar methods of thinking lead to the discovery of the Anyswap exploit, and to a lesser extent, the various "fake contract" exploits.

### 🥈 Second Place: [Santiago Palladino](https://github.com/ethereum/solidity-underhanded-contest/tree/master/2022/submissions_2022/submission10_SantiagoPalladino)

_commentary by [Alex Beregszaszi](https://twitter.com/alexberegszaszi) and [Hari Mulackal](https://twitter.com/_hrkrshnn)_

The submission demonstrates what could go wrong if a non-standard signature scheme is used in contracts. The contract cleverly designs the `Order` struct in such a way that it can collide with the RLP encoding of an `approve` transaction. This allows an attacker to reuse the signature for the `approve` transaction to execute an order in the exchange!

There are several other issues with the implementation, and we leave it up to the reader to uncover all of them!

### 🥇 First Place: [Tynan Richards](https://github.com/ethereum/solidity-underhanded-contest/tree/master/2022/submissions_2022/submission9_TynanRichards)

_commentary by [Duncan Townsend](https://twitter.com/duncancmt)_



## Honorable Mentions

### 💐  [William Bowling](https://github.com/ethereum/solidity-underhanded-contest/tree/master/2022/submissions_2022/submission4_WilliamBowling)

_commentary by [Anton Permenev](https://twitter.com/a_permenev)_

With Solidity v0.8, developers don't have to think about the overflow in arithmetic operations.

This submission is a good reminder that bit shift << and >> operators are not among the arithmetic ones, and thus don't revert on overflow.

However, such operators are quite rare and their usage is suspicious and raises eyebrows.

## Trick us at the next Underhanded Solidity Contest, anon!

Once again, the Underhanded Solidity Contest showed interesting anti-patterns and intriguing ways to embed malicious exploits into smart contract systems. We can't wait to see what you come up with next time!

Thanks to [ChainSecurity](https://chainsecurity.com/), [ConsenSys Diligence](https://diligence.consensys.net/), [Immunefi](https://immunefi.com/), [Solidified](https://solidified.io/), [Trail of Bits](https://www.trailofbits.com/), [Paradigm](https://www.paradigm.xyz/), the [Ethereum Foundation](https://ethereum.foundation/) for their support!

Last  but not least, we'd like to extend a big thank you to our amazing judges for helping us make this contest happen: Alex Beregszaszi, Anton Permenev, Duncan Townsend, Gonçalo Sá, Hari Mulackal, Josselin Feist, samczsun and Stefan Beyer.

Would you like to propose a topic for the next Underhanded Solidity Contest, provide feedback, or help with judging the next time? Then feel free to reach out to us at ``sol_underhanded@ethereum.org``!

_We will be in touch with all participants shortly with details on the claiming process for the NFTs as well as the main prizes._