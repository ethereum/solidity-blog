---
layout: post
published: true
title: 'The Solidity Underhanded Contest is back!'
date: '2020-09-21'
author: Franziska Heintel
category: Announcements
---

We're excited to share that the [Solidity Underhanded Contest](https://underhanded.solidity.rocks/) is finally back!

Inspired by the [Underhanded C Contest](http://www.underhanded-c.org/) and the first [Underhanded Solidity Contest](https://u.solidity.cc/), organized in 2017 by Nick Johnson, we decided it is time for a much needed revival.

![Solidity Underhanded Contest](/img/2020/09/Underhanded_Solidity.gif)

The goal of this contest is to write innocent-looking Solidity code, which pretends to be clear and straightforward, but actually contains malicious behavior or backdoors.

By hosting such a contest we aim to:

+ Raise awareness about smart contract security.
+ Uncover language design flaws.
+ Battle-test recently introduced language features and restrictions.
+ Highlight anti-patterns in smart contact development.
+ Establish new best practices for secure smart contract development.

## The Theme 🚪🔑

**This year's theme is upgradability of contracts, or, more specifically, upgrade mechanisms.**

In order to fix bugs in smart contracts, it is often necessary to perform upgrades. The problem with upgrades is that there is no feasible automatic mechanism to control that the new code still does what the old code intended to do. Because of that, upgrade mechanisms are often designed such that a single account has the ability to change the code arbitrarily.

Since a smart contract in which a single account is authorized to arbitrarily change its implementation defeats the idea of decentralization, we would like to use this contest to find mechanisms that are better suited, be it via an opt-out mechanism, a way to split the contract or whatever else you can come up with.

At the same time, this mechanism has to have a flaw or backdoor that is not easy to discover so that in the end, there is still a single account in control, even if it does not seem like it.

Note that **the flaw should be in the upgrade mechanism**, not in the main contract. Hence, the contract that is actually to be upgraded should be very small, e.g. an ERC20 contract or a simple registry. You do not need to come up with a reason to actually upgrade the contract. Of course a little story around your submission is a nice-to-have, but it's not mandatory.

## The Contest Details 🧑‍💻

Submissions are open from 1st of October until the 31st of October 2020. Anybody, with the exception of judges, can participate (for details about submissions and participation please see the [contest website](https://underhanded.solidity.rocks/)).

Judges will be presented with anonymised submissions. This year's submissions will be assessed by:

+ Alex Beregszaszi, Solidity Co-Lead at Ethereum Foundation.
+ Austin Williams, Security Researcher at OpenZeppelin.
+ Christian Reitwiessner, Solidity Co-Lead at Ethereum Foundation.
+ Gonçalo Sá, Security Engineer at ConsenSys Diligence.
+ Stefan Beyer, Lead Auditor at Solidified.

Winners will be able to choose a prize of their choice from the prize pool. 

The prize pool contains: 

+ 1x Ticket to Devcon6.
+ 1x 1-year MythX Pro Plan.
+ 1x 1-year MythX Dev Plan.
+ 1x Automatic acceptance into Solidified auditor pool & a paid audit with Solidified.
+ 1x 1-year subscription to OpenZeppelin's forthcoming security product.

Furthermore, the winners and all qualified submissions will receive a custom NFT.

## Judging Criteria ⚖️

Consider the following aspects and properties for your submissions:

+ **Simplicity & Briefness**: Since it's much easier to hide a vulnerability in complex and poorly written code, short and clean submissions will be scored higher than those that are lengthy and complicated.
+ **Plausibility**: Code that drops down to inline assembly without any clear reason why will look immediately suspicious, no matter how cleverly written the assembly-level flaw is.
+ **Originality**: Truly original ideas will receive more points than making use of already well known exploit/backdoor mechanisms.

## Submissions 📩

**License**: The entirety of your submission must be licensed under an open-source license. 

**Solidity Version**: Please use Solidity v0.7.0 or newer.

Submissions open on 1st of October 2020. Email your submissions before the deadline (2020-10-31) to `sol_underhanded@ethereum.org`. Entries should consist of a ZIP file containing a README describing your submission and how it works (spoilers included), and one or more Solidity files. For details of the submission process, please refer to the [contest website](https://underhanded.solidity.rocks/).


## Thank you to our supporters! 🥰

We would like to thank ConsenSys Diligence, Solidified, OpenZeppelin, and the Ethereum Foundation for contributing prizes, supporting with judging and all their input and Nick Johnson for initially launching this contest in 2017!

Should you have any questions or encounter problems please feel free to reach out to us via `sol_underhanded@ethereum.org` or join the [Solidity Gitter channel](https://gitter.im/ethereum/solidity).

**We're looking forward to receiving many interesting submissions. Have fun!**
