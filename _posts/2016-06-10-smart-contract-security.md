---
layout: post
published: true
title: 'Smart Contract Security'
date: '2016-06-10'
author: Christian Reitwiessner
category: Security Alerts
---
_This post was originally published on the [Ethereum blog](https://blog.ethereum.org/2016/06/10/smart-contract-security/)._

Solidity was started in October 2014 when neither the Ethereum network nor the virtual machine had any real-world testing, the gas costs at that time were even drastically different from what they are now. Furthermore, some of the early design decisions were taken over from Serpent. During the last couple of months, examples and patterns that were initially considered best-practice were exposed to reality and some of them actually turned out to be anti-patterns. Due to that, we recently updated some of the [Solidity documentation](https://solidity.readthedocs.org), but as most people probably do not follow the stream of github commits to that repository, I would like to highlight some of the findings here.

I will not talk about the minor issues here, please read up on them in the [documentation](http://solidity.readthedocs.io/en/latest/miscellaneous.html#pitfalls).

## Sending Ether

Sending Ether is supposed to be one of the simplest things in Solidity, but it turns out to have some subtleties most people do not realise.

It is important that at best, the recipient of the ether initiates the payout. The following is a **BAD** example of an auction contract:

```solidity
// THIS IS A NEGATIVE EXAMPLE! DO NOT USE!
contract auction {
  address highestBidder;
  uint highestBid;
  function bid() {
    if (msg.value < highestBid) throw;
    if (highestBidder != 0)
      highestBidder.send(highestBid); // refund previous bidder
    highestBidder = msg.sender;
    highestBid = msg.value;
  }
}
```

Because of the maximal stack depth of 1024 the new bidder can always increase the stack size to 1023 and then call ``bid()`` which will cause the ``send(highestBid)`` call to silently fail (i.e. the previous bidder will not receive the refund), but the new bidder will still be highest bidder. One way to check whether ``send`` was successful is to check its return value:

```solidity
/// THIS IS STILL A NEGATIVE EXAMPLE! DO NOT USE!
if (highestBidder != 0)
  if (!highestBidder.send(highestBid))
    throw;
```

The ``throw`` statement causes the current call to be reverted. This is a bad idea, because the recipient, e.g. by implementing the fallback function as ``function() { throw; }`` can always force the Ether transfer to fail and this would have the effect that nobody can overbid her.

The only way to prevent both situations is to convert the sending pattern into a withdrawing pattern by giving the recipient control over the transfer:

```solidity
/// THIS IS STILL A NEGATIVE EXAMPLE! DO NOT USE!
contract auction {
  address highestBidder;
  uint highestBid;
  mapping(address => uint) refunds;
  function bid() {
    if (msg.value < highestBid) throw;
    if (highestBidder != 0)
      refunds[highestBidder] += highestBid;
    highestBidder = msg.sender;
    highestBid = msg.value;
  }
  function withdrawRefund() {
    if (msg.sender.send(refunds[msg.sender]))
      refunds[msg.sender] = 0;
  }
}
```

Why does it still say "negative example" above the contract? Because of gas mechanics, the contract is actually fine, but it is still not a good example. The reason is that it is impossible to prevent code execution at the recipient as part of a send. This means that while the send function is still in progress, the recipient can call back into withdrawRefund. At that point, the refund amount is still the same and thus they would get the amount again and so on. In this specific example, it does not work, because the recipient only gets the gas stipend (2100 gas) and it is impossible to perform another send with this amount of gas. The following code, though, is vulnerable to this attack: ``msg.sender.call.value(refunds[msg.sender])()``.

Having considered all this, the following code should be fine (of course it is still not a complete example of an auction contract):

```solidity
contract auction {
  address highestBidder;
  uint highestBid;
  mapping(address => uint) refunds;
  function bid() {
    if (msg.value < highestBid) throw;
    if (highestBidder != 0)
      refunds[highestBidder] += highestBid;
    highestBidder = msg.sender;
    highestBid = msg.value;
  }
  function withdrawRefund() {
    uint refund = refunds[msg.sender];
    refunds[msg.sender] = 0;
    if (!msg.sender.send(refund))
     refunds[msg.sender] = refund;
  }
}
```

Note that we did not use throw on a failed send because we are able to revert all state changes manually and not using throw has a lot less side-effects.

## Using Throw

The throw statement is often quite convenient to revert any changes made to the state as part of the call (or whole transaction depending on how the function is called). You have to be aware, though, that it also causes all gas to be spent and is thus expensive and will potentially stall calls into the current function. Because of that, I would like to recommend to use it **only** in the following situations:

#### 1. Revert Ether transfer to the current function

If a function is not meant to receive Ether or not in the current state or with the current arguments, you should use throw to reject the Ether. Using throw is the only way to reliably send back Ether because of gas and stack depth issues: The recipient might have an error in the fallback function that takes too much gas and thus cannot receive the Ether or the function might have been called in a malicious context with too high stack depth (perhaps even preceding the calling function).

Note that accidentally sending Ether to a contract is not always a UX failure: You can never predict in which order or at which time transactions are added to a block. If the contract is written to only accept the first transaction, the Ether included in the other transactions has to be rejected.

#### 2. Revert effects of called functions

If you call functions on other contracts, you can never know how they are implemented. This means that the effects of these calls are also not know and thus the only way to revert these effects is to use throw. Of course you should always write your contract to not call these functions in the first place, if you know you will have to revert the effects, but there are some use-cases where you only know that after the fact.

## Loops and the Block Gas Limit

There is a limit of how much gas can be spent in a single block. This limit is flexible, but it is quite hard to increase it. This means that every single function in your contract should stay below a certain amount of gas in all (reasonable) situations. The following is a BAD example of a voting contract:

```solidity
/// THIS IS STILL A NEGATIVE EXAMPLE! DO NOT USE!
contract Voting {
  mapping(address => uint) voteWeight;
  address[] yesVotes;
  uint requiredWeight;
  address beneficiary;
  uint amount;
  function voteYes() { yesVotes.push(msg.sender); }
  function tallyVotes() {
    uint yesVotes;
    for (uint i = 0; i < yesVotes.length; ++i)
      yesVotes += voteWeight[yesVotes[i]];
    if (yesVotes > requiredWeight)
      beneficiary.send(amount);
  }
}
```

The contract actually has several issues, but the one I would like to highlight here is the problem of the loop: Assume that vote weights are transferrable and splittable like tokens (think of the DAO tokens as an example). This means that you can create an arbitrary number of clones of yourself. Creating such clones will increase the length of the loop in the tallyVotes function until it takes more gas than is available inside a single block.

This applies to anything that uses loops, also where loops are not explicitly visible in the contract, for example when you copy arrays or strings inside storage. Again, it is fine to have arbitrary-length loops if the length of the loop is controlled by the caller, for example if you iterate over an array that was passed as a function argument. But **never** create a situation where the loop length is controlled by a party that would not be the only one suffering from its failure.

As a side note, this was one reason why we now have the concept of blocked accounts inside the DAO contract: Vote weight is counted at the point where the vote is cast, to prevent the fact that the loop gets stuck, and if the vote weight would not be fixed until the end of the voting period, you could cast a second vote by just transferring your tokens and then voting again.

## Receiving Ether / the fallback function

If you want your contract to receive Ether via the regular send() call, you have to make its fallback function cheap. It can only use 2300, gas which neither allows any storage write nor function calls that send along Ether. Basically the only thing you should do inside the fallback function is log an event so that external processes can react on the fact. Of course any function of a contract can receive ether and is not tied to that gas restriction. Functions actually have to reject Ether sent to them if they do not want to receive any, but we are thinking about potentially inverting this behaviour in some future release.
