---
layout: post
published: true
title: 'Analysis of Storage Corruption Bug'
date: '2016-11-09'
author: Christian Reitwiessner
category: Security Alerts
---
_This post was originally published on the [Ethereum blog](https://blog.ethereum.org/2016/11/09/analysis-storage-corruption-bug/)._

This blog post provides an update on our findings following the discovery of the [storage corruption bug last week](https://blog.soliditylang.org/2016/11/01/security-alert-solidity-variables-can-overwritten-storage/). 
In summary, the bug was much less severe than we initially thought. The small number of affected contracts we found is either only exploitable by the owner, or the exploit can only cause a disruption in the user interface and not in the actual contract logic. All exploitable contracts/dapps we reviewed can be fixed without having to upgrade the contract itself. Of course, please still check your contracts to be safe.

Following the discovery of the storage corruption bug in the Solidity compiler and the realization that it may have serious effects on already-deployed contracts that cannot be updated, we started analyzing how common the bug is and how exploitable contracts can be addressed.

We focused on contracts with source code published on etherscan because important or popular smart contracts usually have their source code published there in order to gain trust from their users, who can then verify the compilation. Furthermore, if the source code is not available, it is also much harder for an attacker to find a suitable exploit. Finally, contracts that are privately used (and thus do not require publishing their source code) usually check that they are called from a certain address, and thus an attacker has no means to write to their storage.

In order to automate the process of checking all contracts on etherscan, we created a modified version of the Solidity compiler that can automatically detect the conditions for triggering the bug. This technique has already reduced the number of potentially vulnerable contracts to 167. We then manually checked those contracts for potential corruption of storage that would make them vulnerable to attacks.

It turns out that only ten contracts were vulnerable, so we were able to contact most of the contract owners/developers. Seven out of ten of those contracts are only exploitable by the owner in that they are allowed to change certain parameters outside their permitted range, or allowed to unlock a previously locked contract. One contract is exploitable by unprivileged users but have other major flaws in its design. The other two contracts found to be exploitable by unprivileged users either provided no advantages if exploited or only affected the user interface.

## Why are only so few contracts exploitable?

First, let us define what we mean by "exploitable":

The storage corruption bug is exploitable if it can be used to modify a variable in storage in a way that would not be possible without the bug, and this modification has consequences for the behaviour and use of the smart contract. For example, we do not consider a contract exploitable in the following situations:

+ The same account would be able to overwrite the variable in the same state of the contract by regular means.
+ Overwriting can only happen at construction time (note that we did not check whether overwriting occurred at that time).
+ Overwriting is only triggered in unlikely situations where the contract logic was broken anyway (for example, a 32-bit counter that is incremented once per block, oveflows).
+ Variables can be overwritten that are unused in the smart contract and look non-critical, but may be part of the public interface.

**Why is this critical bug only exploitable in so few cases?**

It is a combination of the following factors that together multiply and dramatically reduce the probability of exploitability.

1. Since small types only provide an advantage in very rare cases, they are seldomly used.
2. Small types must be adjacent to each other in storage – a single large type in between them prevents the bug from being triggered.
3. State variables are often assigned one after the other, which removes the corruption at the second assignment.
4. The combination of "address" and "bool" is most common among the cases that are left, but here, the address variable is often an "owner" that is assigned from ``msg.sender`` and thus not exploitable. Even if the owner can be changed, the flag is often a flag that can be still be set by the owner through other means.

## How to fix affected contracts

A large majority of the exploitable contracts are only exploitable by the contract owner, administrator or developer, particularly though a single function that allows the owner to be changed. The exploit allows a further escalation of privileges for the owner. In order to prevent the owner from taking advantage of this exploit, a proxy contract can be installed between the owner and the affected contract. This proxy contract forwards calls from the owner, but disallows calling the exploitable functions. If calling the exploitable functions is still necessary, the proxy contract can prevent malicious data from being forwarded to the contract.

If you have specific questions or concerns regarding your contracts, please contact us on [Gitter](https://gitter.im/ethereum/solidity).

---

**A FRIENDLY IMPORTANT NOTE FROM LEGAL**

The statements in this post are recommendations to address the storage corruption bug in the Solidity compiler. As you know, we are working in an emergent and evolving technical space. The same elements that make this work exciting – the innovation, the impact, the growing understanding of how contracts function – are the same ones that make it risky. If you choose to implement the recommendations in this post and continue to participate, you should make sure you understand how it impacts your specific contract and you should understand that there are risks involved. By choosing to implement these recommendations, you alone assume the risks of the consequences.
