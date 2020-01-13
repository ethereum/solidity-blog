---
layout: post
published: true
title: 'Validated, staking on eth2: #1 - Incentives'
date: '2020-01-13'
author: Carl Beekhuizen
---

![00-Merkle-4K](https://blog.ethereum.org/img/2019/11/00-Merkle-4K.png)

*Thanks to Joseph Schweitzer and Danny Ryan for review.*

Welcome back! Having discussed eth2's design philosophy [last time](https://blog.ethereum.org/2019/11/27/Validated-Staking-on-eth2-0/), today's focus is on eth2's incentives through the lens of that philosophy. More specifically, we look at the incentives effecting eth2 and how they are realised in the form of rewards, penalties, and slashings. 

We then walk through how and why validators are incentivised to remain online, why you **won't** be slashed for going offline, and more. Let's dig in.

## If not for being offline, when do slashings occur? ⚔️

Slashing has two purposes: (1) to make it prohibitively expensive to attack eth2, and (2) to stop validators from being lazy by checking that they actually perform their duties. Slashing a validator is to destroy (a portion of) the validator's stake if they act in a **provably** destructive manner. The two major ways a validator can behave slashably maliciously within eth2 phase 0 are double voting and surround voting (read the [original paper](https://arxiv.org/abs/1710.09437) for more on how Casper FFG works in detail):

***Double voting*** is when a validator votes for two different blocks during the same epoch, which means they are signalling support for two different versions of reality. The simplest example of why this is forbidden is a validator sending transaction $a$ in block $A$ and $b$ in block $B$ where $a$ and $b$ spend the same ETH. This is the Proof of Stake version of the classic *double-spend attack*.

Slashing of ***surround votes*** also prevents two versions of the chain from becoming finalised by punishing validators who create votes which present multiple different versions of reality which they claim to be true at the same time. More specifically, attestations (votes for blocks) are surround votes when a validator attests to one version of reality and later attests to another version, but in a way that doesn't make clear that they no longer believe in the first.

Double and surround voting are the only way validators can be slashed within phase 0, but additional rules are added in later phases to ensure that validators actually store and make available the shard data that they sign (which prevents validators from being lazy or from withholding information).

**A validator that correctly follows the protocol never emits a slashable vote in normal operations**. If not an intentionally malicious action, forming a slashable message only occurs as a result of some bug or accident. To minimise the pain of such errors, the amount of stake destroyed is proportional to the number of other validators slashed around the same time. If a small number of validators commit some slashable offence, it is unlikely that they are trying to attack eth2 because a successful attack requires many validators. Slashings that occur in small numbers are therefore assumed to be honest mistakes and are punished lightly (a minimum of 1 ETH). On the other hand if many validators commit an offence during a similar time, then a large amount of their stake is burnt (up to their full balance) as it is assumed to be an attack on the network.

Validators that are slashed are prevented from participating in the protocol further and are forcibly exited. In the case of an honest mistake, this prevents offending validators from doing further harm to themselves by being slashed again; whereas in the malign instance, this removes malicious validators from the protocol.

## So what happens to validators who are offline? 🚫👩‍💻

Validators that are offline when they are supposed to be participating in the protocol are penalised, but in the normal case these validators **only stand to lose what they would have made as rewards had they participated correctly in the protocol**. This means that validators that are online > 50% of the time will still see their stake increase over time.

As a result of this mechanism, *validator clients that need to go offline for maintenance etc, are usually best off if they just go offine for a short time instead of exiting and re-joining the protocol (both of which have associated delays)*. 

This means that validators need not go to extreme lengths with backup clients or redundant internet connections as the repercussions of being offline are not so severe. In fact, any such system in which two entities can sign messages can be detrimental as primary and backup clients could end up both being online at the same time and emitting slashable votes (via the double voting mechanism explained earlier) as was the case with [the first Cosmos slashing](https://twitter.com/zmanian/status/1145072296723275776).

This regime of offline penalties holds provided that blocks are being finalised (2/3 of validators (weighted by stake) are online and their votes are being counted). This is the expected state of eth2 during normal operation. If less than 2/3 of nodes are online then something has gone catastrophically wrong in the realm of eth2. The family of consensus protocols that Eth's Casper is a part of can no longer reach agreement under these conditions.

## What does eth2 do if > 1/3 of validators are offline? 💣

This is where the inactivity leak mentioned at the start of the article comes in. The *inactivity leak* reduces the balances of the offline nodes over time so that the ratio of online validators to total validators (weighted by stake) can once again exceed 2/3 so eth2 can continue to make decisions as a protocol.

Inactivity leaks are one of the ways eth2 has been designed to survive a WW3-style event. If such an event were to knock out more than 1/3 of all validators, then the offline validators would find that their balances decreased to the point that their participation was no longer needed for eth2 to continue as a chain.

## Anti-correlation and decentralisation

*Both the slashing mechanism and the inactivity leak encourage validators to make decisions that cause their nodes to fail in manners different to those of others*. That is -- to ensure the smallest possible slashings and to prevent inactivity leaks, a validator should attempt to have their clients fail in ways that are different to others'.

This places pressure on all validators to decentralise every aspect of being a validator as, for example, validators that rely on the same source of truth like Infura or use AWS to host their clients will be worse off if something goes wrong.

## With all the many ways to be punished, why would a someone want to be a validator? 📈

As stated in the first article, "validators will be lazy, take bribes, and they will try to attack the system unless they are otherwise incentivised not to." The punishments discussed so far discourage bad behaviour, but rewards are needed to encourage validators to perform actions that benefit eth2.

There are 3 major classes of rewards:

### Whistleblower rewards 🚓

A validator that raises the alarm on another validator by providing proof that gets them slashed is rewarded for their efforts in cleaning up the eth2 streets.

### Proposer rewards ⬜️⛓⬛️⛓⬜️

Validators are randomly assigned the duty of producing a block; the chosen validator is called the *proposer*. A proposer is rewarded for their efforts in the following ways:

* Including a proof from a wistleblower that gets a validator slashed
* Including new attestations from other validators

These rewards encourage validators to provide helpful information to the chain when they are chosen to produce a block.

### Attester rewards ✔

Attestations are votes that signal that a validator agrees with a decision in eth2. These types of messages form the basis of consensus and are rewarded in 5 different ways:

* Getting your attestation on-chain
* Agreeing with other validators about the history of the chain
* Agreeing with others about the head of the chain
* Getting your attestation on chain quickly
* Pointing to the correct block in the assigned shard

## Scaling validator earnings 💸

There are two common approaches for paying validators in PoS systems: fixed rewards and fixed inflation. In the fixed reward model, validators are paid a fixed amount for doing their jobs, and the inflation rate then depends on how many validators sign up. This has the problem of how to correctly set the reward rate. If the reward rate is set too low then too few validators will participate, while a reward rate that is too high encourages extensive validation beyond the requisite security and wastes money.

The complimentary model is one with a fixed inflation rate where some total reward is divided amongst the active validators. This model has the benefit of allowing market forces to find the right amount to pay validators as they all make individual decisions about whether or not to participate based on current earnings. There are downsides to this model. Validator earnings can be erratic making profitability decisions difficult for individual validators. This model also makes the protocol vulnerable to [discouragement attacks](https://github.com/ethereum/research/raw/master/papers/discouragement/discouragement.pdf) in which validators attempt to prevent each other from participating to increase their own profit (even at their own temporary loss).

eth2 aims to have the best of both worlds by choosing a reward model in which validator rewards are proportional to the square root of the total amount of ETH staked. This hybrid model attempts to suppress variations in inflation and validator return rates while still allowing market forces to determine the correct amount to pay each validator for the security provided.

## Hope for the best, but expect the worst 🛡️

Each of the facets of eth2's incentive scheme is a result of designing a protocol under the philosophy laid out in the last article. Examples of this include the anti-correlation mechanisms encouraging decentralisation and inactivity leaks helping eth2 to survive World War 3, but the main idea underpinning how the incentives work is the assumption that "validators will be lazy, take bribes, and that they will try to attack the system unless they are otherwise incentivised not to". If someone attacks eth2 in one of the ways discussed here, they better be prepared to throw away a lot of ETH because one way or another they are going to lose it all.


