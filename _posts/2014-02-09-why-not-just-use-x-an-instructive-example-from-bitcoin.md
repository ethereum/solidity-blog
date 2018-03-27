---
id: 57
title: Why Not Just Use X? An Instructive Example from Bitcoin
date: 2014-02-09T10:08:53+00:00
author: Vitalik Buterin
layout: post
guid: http://blog.ethereum.org/?p=57
permalink: /2014/02/09/why-not-just-use-x-an-instructive-example-from-bitcoin/
dsq_thread_id:
  - "2790050015"
---
Bitcoin developer Gregory Maxwell writes the following <a href="http://www.reddit.com/r/Bitcoin/comments/1x93tf/some_irc_chatter_about_what_is_going_on_at_mtgox/cf99yac">on Reddit:</a>

There is a design flaw in the Bitcoin protocol where its possible for a third party to take a valid transaction of yours and mutate it in a way which leaves it valid and functionally identical but with a different transaction ID. This greatly complicates writing correct wallet software, and it can be used abusively to invalidate long chains of unconfirmed transactions that depend on the non-mutant transaction (since transactions refer to each other by txid).

This issue arises from several sources, one of them being OpenSSL’s willingness to accept and make sense of signatures with invalid encodings. A normal ECDSA signature encodes two large integers, the encoding isn’t constant length— if there are leading zeros you are supposed to drop them.

It’s easy to write software that assumes the signature will be a constant length and then leave extra leading zeros in them.

This is a very interesting cautionary tale, and is particularly important because situations like these are part of the reason why we have made certain design decisions in our development philosophy. Specifically, the issue is this: many people continue to bring up the point that we are in many places unnecessarily reinventing the wheel, creating our own serialization format, <a href="https://wiki.ethereum.org/index.php/RLP">RLP</a>, instead of using the existing <a href="https://code.google.com/p/protobuf/">protobuf</a> and we’re building an application-specific scripting language instead of “just using Lua”. This is a very valid concern; not-invented-here syndrome is a <a href="http://en.wikipedia.org/wiki/Not_invented_here">commonly-used pejorative</a>, so doing such in-house development does require justification.

And the cautionary tale I quoted above provides precisely the perfect example of the justification that I will provide. External technologies, whether protobuf, Lua or OpenSSL, are very good, and have years of development behind them, but in many cases they were never designed with the perfect consensus, determinism and cryptographic integrity in mind that cryptocurrencies require. The OpenSSL situation above is the perfect example; aside from cryptocurrencies, there really is no other situations where the fact that you can take a valid signature and turn it into another valid signature with a different hash is a significant problem, and yet here it’s fatal. One of our core principles in Ethereum is simplicity; the protocol should be as simple as possible, and the protocol should not contain any black boxes. Every single feature of every single sub-protocol should be precisely 100% documented on the whitepaper or wiki, and implemented using that as a specification (ie. test-driven development). Doing this for an existing software package is arguably almost as hard as building an entirely new package from scratch; in fact, it may even be harder, since existing software packages often have more complexity than they need to in order to be feature-complete, whereas our alternatives do not – read the <a href="https://developers.google.com/protocol-buffers/docs/encoding">protobuf spec</a> and compare it to the <a href="https://wiki.ethereum.org/index.php/RLP">RLP spec</a> to understand what I mean.

Note that the above principle has its limits. For example, we are certainly not foolish enough to start inventing our own hash algorithms, instead using the universally acclaimed and well-vetted SHA3, and for signatures we’re using the same old secp256k1 as Bitcoin, although we’re using RLP to store the v,r,s triple (the v is an extra two bits for public key recovery purposes) instead of the OpenSSL buffer protocol. These kinds of situations are the ones where “just using X” is precisely the right thing to do, because X has a clean and well-understood interface and there are no subtle differences between different implementations. The SHA3 of the empty string is c5d2460186...a470 in C++, in Python, and in Javascript; there’s no debate about it. In between these two extremes, it’s basically a matter of finding the right balance.