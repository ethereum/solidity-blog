---
layout: post
published: true
title: 'Announcing the Winners of the Solidity Underhanded Contest 👨‍💻🏅'
date: '2020-12-03'
author: Franziska Heintel
category: Announcements
---

After thorough assessment of all submissions, we are happy to share the winners of this year's [Solidity Underhanded Contest](https://underhanded.soliditylang.org/)!

If you are not familiar with it, please read [the announcement](https://blog.soliditylang.org/2020/09/21/solidity-underhanded-contest/) from September.

Before we dive into the winning submissions, we'd like to thank all participants for taking part. In total, we received 16 qualifying submissions which you can find in [this repo](https://github.com/ethereum/solidity-underhanded-contest/tree/master/submissions_2020). All 16 submissions are eligible for a "qualified submission" Solidity Underhanded POAP NFT - winners will receive an additional "Winners" POAP NFT.

We will be in touch with all participants shortly with details on the claiming process for the NFTs as well as the main prizes.

Now, let's have a look at this year's winners, starting from the 5th place!

## Solidity Underhanded Contest 2020 Winners

### 5️⃣ th Place: [Marius van der Wijden](https://github.com/ethereum/solidity-underhanded-contest/tree/master/submissions_2020/submission14_MariusVanDerWijden) 

_commentary by [Alex Beregszaszi](https://twitter.com/alexberegszaszi)_

This submission highlights the need for clearly distinguishing error conditions from return values. The `ecrecover` precompile has a single output, the address, which is set to zero in case of errors. While [some](https://gist.github.com/axic/5b33912c6f61ae6fd96d6c4a47afde6d) high-level wrappers for this precompile made a distinction, many introduce new error conditions, but still reuse the same mechanic of returning a “zero address” on error. This was the case with OpenZeppelin’s version [prior](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/v2.5.0/contracts/cryptography/ECDSA.sol) to the 3.0 series.

Using this validation shortcoming, the submission allows anyone to create unlimited "vote accepted" transactions.

Solidity has always supported multiple return values which allows returning a status/error code and data separately, but more importantly the established best practice is to use `require` statements for validation, which most `ecrecover` use cases follow today.

While the submission is written clearly and the issue is not obvious without dissecting both the `closeVote` and the *trusted* `ECDSA.recover` functions, in our opinion there are two shortcomings raising suspicion:
1. Append only data structures are not commonly used.
2. Awareness about `ECDSA.recover` was raised recently, and thus one would expect a more thorough review of its applications. The submission used the version from OpenZeppelin 2.5.0.

### 4️⃣ th Place: [Richard Moore](https://github.com/ethereum/solidity-underhanded-contest/tree/master/submissions_2020/submission8_RichardMoore)

_commentary by [Stefan Beyer](https://twitter.com/beyer_st)_

The submission exploits `CREATE2` to hijack a multisig contract that controls upgradability. Whilst this is a known issue, the scenario demonstrates the impact of `CREATE2` in combination with self-destruct and shows the importance of assessing the contracts deployment procedure in a security analysis. 

### 3️⃣ rd Place: [Cory Dickson](https://github.com/ethereum/solidity-underhanded-contest/tree/master/submissions_2020/submission1_CoreyDickson)

_commentary by [Goncalo Sá](https://twitter.com/GNSPS)_
 
This submission highlights the perils of validating non-EOA (Externally Owned Accounts) addresses within the EVM. Even though assessing whether an account interacting with a certain system is not a contract has been the source of many production hacks on mainnet and therefore is a well-studied, bad-practice pattern, the one where a user "DoS-es" a system by self-destructing their contract is not.

In a nutshell, this attack demonstrates the possibility of a lock-up scenario in a governance system that depends on composability and expects the existence of a smart contract at a certain address to be a time-independent function.
Since the OpenZeppelin library used to check if an account is a contract or not returns true even though a contract might self-destruct at the end of a transaction, the attacker is able to lock-up following governance votes to change the delegated contract.

**Any drawbacks?**

As a result of its extreme simplicity, the submission is a bit lacking in framing an actual real-life scenario where this might happen.
Improvements in either the storytelling or the smart contract system would have been welcomed.

**Why did we chose this submission to be one of the winners?**

Its simplicity is really good. The attack could not have been more straight-forward than it was and still relevant in today’s security landscape of the EVM.

Reiterating the answer to the previous question, this extreme simplicity is also a reason for it to not be the strongest of submissions. Some more context would’ve been welcomed.

### 2️⃣ nd Place: [Jaime Iglesias](https://github.com/ethereum/solidity-underhanded-contest/tree/master/submissions_2020/submission4_JaimeIglesias)

_commentary by [Austin Williams](https://twitter.com/onewayfunction)_

In this submission, an [EIP-1967](https://eips.ethereum.org/EIPS/eip-1967) proxy is used for upgradability of an [ERC-20](https://eips.ethereum.org/EIPS/eip-20) token contract. There are two key roles here: an admin and an owner. There is an `_optIn` storage variable that indicates whether or not the owner has consented to an upgrade. The admin should not be able to upgrade unless the owner has set the `_optIn` value to `true`.

The `_optIn` storage variable is stored in storage slot `0x7b191067458f5b5c0c36f2be8ceaf27679e7ea94b6964093ce9e5c7db2aff82a`, which the code comments claim is derived via "the keccak-256 hash of 'eip1967.proxy.optIn' subtracted by 1". However, the storage location actually points to the location in storage where the token balance of an admin-known address is stored. This means the admin can set `_optIn` to `true` without the owner's consent simply by sending some tokens to the admin-known address -- thereby changing the value at the `_optIn` storage location from `0` to a truthy value.

In this way, the back door can be unlocked and locked simply by sending tokens to and from some address that is controlled by the admin.


### 1️⃣ st Place: [Robert M C Forster](https://github.com/ethereum/solidity-underhanded-contest/tree/master/submissions_2020/submission11_RobertMCForster)

_commentary by [Chris Reitwiessner](https://twitter.com/ethchris)_

This submission implements a nicely structured and very simple timelock upgrade: The owner proposes a new implementation of the delegate which will get active one month after it has been proposed.

The only suspicious part about it is that the point in time where the upgrade is performed is stored as month and day instead of a timestamp.
The exploit is very well hidden and makes use of special unicode symbols that change the text flow direction. Such symbols are of course invalid in code, but they are accepted in comments.

Since multi-line comments work in the same way when read from left to right as well as from right to left, the submission is able to terminate the comment "from right to left" and switches into code mode which is then displayed in the "wrong" order. This causes the day and month variables to be swapped unnoticed and the upgrade is performed much earlier than expected.

You can see (*or not, depending on your editor 😬*) the direction change in action by selecting [part of line 65](https://github.com/ethereum/solidity-underhanded-contest/blob/master/submissions_2020/submission11_RobertMCForster/contracts/TimelockUpgrade.sol#L65) in the submission.

This is a flaw that can be prevented at the level of Solidity by disallowing text direction changes to flow across comments ([issue tracker](https://github.com/ethereum/solidity/issues/10254)), but I would also call this to the attention of IDE, highlighter and linter developers. I am not sure how a linter would handle such a situation, but if a linter marks superfluous whitespace at the end of a line, it should certainly mark a text direction change flowing out of a comment.

## Honorable Mentions

### 🦇 [Chris Whinfrey](https://github.com/ethereum/solidity-underhanded-contest/tree/master/submissions_2020/submission15_ChrisWhinfrey)'s VampireSwap

Props for the witty and detailed storytelling and documentation!

This submission shows the social aspect of trusting “audited” upgradable contracts: unless someone verifies every single on-chain executed upgrade (plus the initialization), none of the audits matter. *Not even the one from Open Trail of Diligence! ;)*

### 🐛 [Luiz Soares & Boris Breslav](https://github.com/ethereum/solidity-underhanded-contest/tree/master/submissions_2020/submission9_LuizSoares)'s Superior Proxy

This submissions exploits a Solidity compiler bug, namely the [Solidity Dynamic Array Cleanup Bug](https://blog.soliditylang.org/2020/10/07/solidity-dynamic-array-cleanup-bug/), which was fixed with Solidity v0.7.3. Using this bug is a great idea in theory, however, the implementation is suspicious and would raise eyebrows.

## See you at the next Solidity Underhanded Contest!

We would like to thank ConsenSys Diligence, Solidified, OpenZeppelin, and the Ethereum Foundation for contributing prizes and our judges Alex Beregszaszi, Austin Williams, Christian Reitwiessner, Gonçalo Sá, and Stefan Beyer for their assessments and support!

Would you like to propose a topic for the next Solidity Underhanded Contest, provide feedback, sponsor a prize, or help with judging the next time? Then feel free to reach out to us at sol_underhanded@ethereum.org!
