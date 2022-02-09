---
layout: post
published: true
title: 'Underhanded Solidity Contest 2022'
date: '2022-02-09'
author: Franziska Heintel
category: Announcements
---

The long wait is over: The [Underhanded Solidity Contest](https://underhanded.soliditylang.org/) is back with a 2022 edition!

After a successful [revival](https://blog.soliditylang.org/2020/09/21/solidity-underhanded-contest/) in 2020, we believe it's time for the great Solidity minds to get together again and compete over the next big underhanded hack!

In case you're new to this, let's get you up to speed with a quick recap on what the Underhanded Solidity Contest (USC) is all about:

In a nutshell, the USC is about finding loopholes or "hiding spots" in the Solidity language and using those to write seemingly innocent and straightforward-looking Solidity code which contains malicious behavior or backdoors.

With that, the Underhanded Solidity Contest aims to:

+ Raise awareness about smart contract security.
+ Uncover language design faults.
+ Battle-test recently introduced language features and restrictions.
+ Highlight anti-patterns in smart contact development.
+ Establish new best practices for secure smart contract development.

So, let's have a look at the 2022 contest specifics, the theme, deadlines, judges, prizes and more!

## The Theme: Decentralized Exchanges 🧪

Build a decentralized exchange that looks fair, but can be "manipulated".

In this year, we would like you to build a simple decentralized exchange where people can trade their hard-earned NFTs, DAO governance tokens, or dog coins of their choice.

The rules of decentralized exchanges (at least the simple ones) are very easy and nothing can go wrong there... right?

Build a decentralized exchange either with an automated market maker or a match-making mechanism where trades do not really work as expected. This can be either by it leaking money, a specific account being able to withdraw all money or something else you can think of. You can also add a flaw to a token implementation you provide instead of to the exchange itself. The only hard requirement is that the flaw is hidden.

## The Judges ⚖️

Judges are presented with anonymised submissions. This year, the submissions will be assessed by:

+ [Alex Beregszaszi](https://twitter.com/alexberegszaszi), Solidity Co-Lead at Ethereum Foundation.
+ [Anton Permenev](https://twitter.com/a_permenev), Senior Engineer at ChainSecurity.
+ [Duncan Townsend](https://twitter.com/duncancmt), CTO at Immunefi.
+ [Gonçalo Sá](https://twitter.com/GNSPS), Security Engineer at ConsenSys Diligence.
+ [Harikrishnan Mulackal](https://twitter.com/_hrkrshnn), C++ Engineer Solidity at Ethereum Foundation.
+ [Josselin Feist](https://twitter.com/Montyly), Principal Security Engineer at Trail of Bits.
+ [samczsun](https://twitter.com/samczsun), Research Partner at Paradigm.
+ [Stefan Beyer](https://twitter.com/beyer_st), Lead Auditor at Solidified.

### Judging Criteria ☑️

Rember to consider the following aspects and properties for your submissions:

+ **Simplicity & Conciseness**: Since it's much easier to hide a vulnerability in complex and poorly written code, short and clean submissions will be scored higher than those that are lengthy and complicated.
+ **Plausibility**: Code that drops down to inline assembly without any clear reason why will look immediately suspicious, no matter how cleverly written the assembly-level flaw is.
+ **Originality**: Truly original ideas will receive more points than making use of already well known exploit/backdoor mechanisms.

## The Prizes 🏆

This year, the first place will receive a ticket to [Devcon VII Bogota](https://devcon.org/).

The top 3 submissions will receive a ticket to the [Devconnect Coworking Space](https://devconnect.org/) and the opportunity to present their contest submission at the [Solidity Summit 2022](https://summit.soliditylang.org/) in Amsterdam.

The top 3 submissions of the Underhanded Solidity Contest will also be awarded points for the upcoming [Paradigm CTF 2022](https://ctf.paradigm.xyz/).
Those points will be awarded as though the winners solved a bonus challenge ("pseudo-challenge").

Furthermore, the three winners will be added to the Board of Fame.

The winners and all qualified submissions will receive a custom Underhanded Solidity Contest NFT.

## The Submission 📨

Before submitting your code ensure that:
+ The entirety of your submission is licensed under an open-source license.
+ In your submission you use Solidity v0.8.0 or newer.
+ You have carefully read the [Coding Brief](https://underhanded.soliditylang.org/#coding-brief).

Please email your submissions before the deadline [2022-03-16, 11:59PM UTC] to ``sol_underhanded@ethereum.org``. Entries should consist of a ZIP file containing a README describing your submission and how it works, a separate file for spoilers, and one or more Solidity files.

Each person can only enter one submission. If you want to make a team submission, nominate a single person to submit on your team's behalf. Since entries will be forwarded to the judges and assessed anonymously, please do not include identifying information in the ZIP file.

## The Deadline ⏳

+ Submissions open: 2022-02-09.
+ Submissions close: 2022-03-16.

Winners will be announced in time before Devconnect Amsterdam in April.

## The Board of Fame ⭐

To honor all winners of previous Underhanded Solidity Contests, we have introduced the [Board of Fame](https://underhanded.soliditylang.org/#board-of-fame). Consider checking out the hacks of previous winners for inspiration and to make sure you are not using an already known exploit by accident.

## The Supporters 💖

Last but not least, we'd like to extend our thanks to [ChainSecurity](https://chainsecurity.com/), [ConsenSys Diligence](https://consensys.net/diligence/), [Immunefi](https://immunefi.com/), [Solidified](https://solidified.io/), [Trail of Bits](https://www.trailofbits.com/), [Paradigm](https://www.paradigm.xyz/), the [Ethereum Foundation](https://ethereum.foundation/) and all the judges for their support in organizing this contest!

Should you have any questions or encounter problems please feel free to reach out to us via ``sol_underhanded@ethereum.org`` or join the Solidity [Matrix](https://matrix.to/#/#ethereum_solidity:gitter.im) / [Gitter](https://gitter.im/ethereum/solidity) channel.

**We’re looking forward to receiving many innocent submissions. 😇 Have fun!**
