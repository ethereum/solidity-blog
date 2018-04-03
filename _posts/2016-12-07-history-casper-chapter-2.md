---
id: 3558
title: 'The History of Casper &#8211; Chapter 2'
date: 2016-12-07T21:58:58+00:00
author: Vlad Zamfir
layout: post
guid: https://blog.ethereum.org/?p=3558
permalink: /2016/12/07/history-casper-chapter-2/
---
This chapter describes the game theory and economic security modelling we were doing in the Fall of 2014. It recounts how the "bribing attacker model" led our research directly to a radical solution to the long range attack problem.
<h2>Chapter 2: The Bribing Attacker, Economic Security, and the Long Range Attack Problem</h2>
Vitalik and I had each been reasoning about incentives as part of our research before we ever met, so the proposition that "getting the incentives right" was crucial in proof-of-stake was never a matter of debate. <strong>We were never willing to take "half of the coins are honest" as a security assumption.</strong> (It's in bold because it's important.) We knew that we needed some kind of "incentive compatibility" between bonded node incentives and protocol security guarantees.

It was always our view that the protocol could be viewed as a game that could easily result in "bad outcomes" if the protocol's incentives encouraged that behaviour. We regarded this as a potential security problem. Security deposits gave us a clear way to punish bad behaviour; slashing conditions, which are basically programs that decide whether to destroy the deposit.

We had long observed that Bitcoin was more secure when the price of bitcoin was higher, and less secure when it was lower. We also now knew that security deposits provided slasher with more economic efficiency than slasher only on rewards. <strong>It was clear to us that economic security existed and we made it a high priority.</strong>
<h3>The Bribing Attacker</h3>
I'm not sure how much background Vitalik had in game theory (though it was clear he had more than I did). My own game theory knowledge at the start of the story was even more minimal than it is at the end. But I knew how to recognize and calculate Nash Equilibriums. If you haven't learned about Nash Equilibriums yet, this next paragraph is for you.

A Nash Equilibrium is a strategy profile (the players' strategy choices) with a corresponding payoff (giving $ETH or taking $ETH away) where no players individually have an incentive to deviate. "Incentive to deviate" means "they get more $ETH if they somehow change what they're doing". If you remember that, and every time you hear "Nash Equilbrium" you thought "no points for individual strategy changes", you'll have it.

Some time in late summer of 2014, I first ran into "the bribing attacker model" when I made an offhand response to an economic security question Vitalik asked me on a Skype call ("I can just bribe them to do it"). I don't know where I got the idea. Vitalik then asked me again about this maybe a week or two later, putting me on the spot to develop it further.

<strong>By bribing game participants you can modify a game's payoffs, and through this operation change its Nash Equilibriums.</strong> Here's how this might look:

<img src="http://i.imgur.com/owE9V4c.png"/>
The bribe attack changes the Nash Equilibrium of the Prisoner's Dilemma game from (Up, Left) to (Down,Right). The bribing attacker in this example has a cost of 6 if (Down, Right) is played.

<strong>The bribing attacker was our first useful model of economic security.</strong>

Before the bribing attack, we usually thought about economic attacks as hostile takeovers by foreign, extra-protocol purchasers of tokens or mining power. A pile of external capital would have to come into the system to attack the blockchain. With the bribe attack, the question became "what is the price of bribing the currently existing nodes to get the desired outcome?".

<strong>We hoped that the bribing attacks of our yet-to-be-defined proof-of-stake protocol would have to spend a lot of money to compensate for lost deposits.</strong>

Debate about "reasonableness" aside, this was our first step in learning to reason about economic security. It was fun and simple to use a bribing attacker. You just see how much you have to pay the players to do what the attacker wants. And we were already confident that we would be able to make sure that an attacker has to pay security-deposit-sized bribes to revert the chain in an attempted double-spend. We knew we could recognize "double-signing". So we were pretty sure that this would give proof-of-stake a quantifiable economic security advantage over a proof-of-work protocol facing a bribing attacker.
<h3>The Bribing Economics of the Long Range Attack</h3>
Vitalik and I applied the bribing attacker to our proof-of-stake research. <strong>We found that PoS protocols without security deposits could be trivially defeated with small bribes. You simply pay coin holders to move their coins to new addresses and give you the key to their now empty addresses.</strong> (I'm not sure who originally thought of this idea.) Our insistence on using the briber model easily ruled out all of the proof-of-stake protocols we knew about. I liked that. (At the time we had not yet heard of Jae Kwon's Tendermint, of Dominic William's now-defunct Pebble, or of Nick Williamson's Credits.)

This bribe attack also posed a challenge to security-deposit based proof-of-stake: The moment after a security deposit was returned to its original owner, the bribing adversary could buy the keys to their bonded stakeholder address at minimal cost.

<strong>This attack is identical to the long range attack.</strong> It is acquiring old keys to take control of the blockchain. It meant that the attacker can create "false histories" at will. But only if they start at a height from which all deposits are expired.

Before working on setting the incentives for our proof-of-stake protocol, therefore, we needed to address the long-range attack problem. <strong>If we didn't address the long range attack problem, then it would be impossible for clients to reliably learn who really had the security deposits.</strong>

We did know that developer checkpoints could be used to deal with the long-range attack problem. We thought this was clearly way too centralized.

In the weeks following my conversion to proof-of-stake, while I was staying at Stephan Tual's house outside of London, I discovered that there was a natural rule for client reasoning about security deposits. <strong>Signed commitments are only meaningful if the sender <em>currently</em> has a deposit.</strong> That is to say, after the deposit is withdrawn, the signatures from these nodes are no longer meaningful. Why would I trust you after you withdraw your deposit?

The bribing attack model demanded it. <strong>It would cost the bribing attacker almost nothing to break the commitments after the deposit is withdrawn. </strong>

This meant that a client would hold a list of bonded nodes, and stop blocks at the door if they were not signed by one of these nodes. <strong>Ignoring consensus messages from nodes who don't <em>currently</em> have security deposits <del>solves</del> circumvents the long-range attack problem.  </strong>Instead of authenticating the current state based on the history starting from the genesis block, we authenticate it based on a list of who currently has deposits.

<strong>This is radically different from proof-of-work. </strong>

In PoW, a block is valid if it is chained to the genesis block, and if the block hash meets the difficulty requirement for its chain. In this security deposit-based model, a block is valid if it was created by a stakeholder with a currently existing deposit. This meant that you would need to have current information in order to authenticate the blockchain. This subjectivity has caused a lot of people a lot of concern, but it is necessary for security-deposit based proof-of-stake to be secure against the bribing attacker.

This realization made it very clear to me that the proof-of-work security model and the proof-of-stake security model are fundamentally not compatible. I therefore abandoned any serious use of "hybrid" PoW/PoS solutions. Trying to authenticate a proof-of-stake blockchain from genesis now seemed very obviously wrong.

Beyond changing the authentication model, however, we did need to provide a way to manage these lists of security deposits. <strong>We had to use signatures from bonded nodes to manage changes to the list of bonded nodes, and we had to do it after the bonded nodes come to consensus on these changes.</strong> Otherwise, clients would have different lists of bonded validators, and they would therefore be unable to agree on the state of Ethereum.<strong> </strong>

<strong>Bond time needed to be made long, so that clients have time to learn about the new, incoming set of bonded stakeholders.</strong> As long as clients were online enough, they could keep up to date. I thought we would use twitter to share the bonded node list, or at least a hash, so that new and hibernating clients could get synchronized after their user enters a hash into the UI.

<strong>If you have the wrong validator list you can get <a href="https://en.wikipedia.org/wiki/Man-in-the-middle_attack">man-in-the-middled</a>.</strong> But it's really not that bad. The argument was (and still is!) that <strong>you only need to be able to trust an external source for this information once</strong>. After that once, you will be able to update your list yourself - at least, if you are able to be online regularly enough to avoid the "long range" of withdrawn deposits.

I know that it might take some getting used to. But we can only rely on fresh security deposits. Vitalik was a bit uncomfortable with this argument at first, trying to hold onto the ability to authenticate from genesis, but eventually was convinced by the necessity of this kind of subjectivity in proof of stake protocols. Vitalik independently came up with his <a href="https://blog.ethereum.org/2014/11/25/proof-stake-learned-love-weak-subjectivity/">weak subjectivity scoring rule</a>, which seemed to me like a perfectly reasonable alternative to my idea at the time, which was basically "have all the deposits sign every Nth block to update the bonded node list".

<strong>With the nails in the nothing-at-stake and long-range attack coffins completely hammered in, we were ready to start choosing our slashing conditions.</strong>

The next chapter will document what we learned from our first struggles to define a consensus protocol by specifying slashing conditions. I'll also tell you about what we learned from talking with fine people from our space about our research. The game theory and economic modelling story presented here will continue developing in Chapter 4.

--------------------------------

NOTE: The views expressed here are solely my own personal views and do not represent those of the Ethereum Foundation. I am solely responsible for what I've written and am not am not acting as a spokesperson for the Foundation.