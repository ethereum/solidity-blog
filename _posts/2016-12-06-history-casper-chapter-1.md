---
id: 3545
title: 'The History of Casper &#8212; Chapter 1'
date: 2016-12-06T20:04:46+00:00
author: Vlad Zamfir
layout: post
guid: https://blog.ethereum.org/?p=3545
permalink: /2016/12/06/history-casper-chapter-1/
---
Vitalik suggested last week that I share my basic research and design philosophy in a blog post, I agreed but complained that it was still changing. My friend Jon West told me that everyone would really appreciate it if I told everyone about my Casper research, I mostly agreed. Then someone on reddit told me to focus on Ethereum.

So here's the Casper tech story, given as a chronological history of the evolution of the key technology, ideas and language that are involved in "Casper research". Many of our favorite blockchain personalities are part of the story. This is my attempt to recount everything in an accessible, sequential way so that you can see where we are now (and where we're going) with our research efforts (so don't argue until the end of the story!). I'm going to try to release a chapter per day until it's complete.

Also note that this is my personal point of view, understanding what little I could manage through the process of working on proof-of-stake. Vitalik and Greg Meredith's accounts will vary, for example, as they each have their own view of Casper research.
<h2>Preface: How I started doing research at Ethereum</h2>
<h4>March 2013-April 2014</h4>
I immediately got hooked on the Blockchain technology story when Bitcoin first (really) caught my attention in March of 2013. This was during the "Cyprus crisis" run-up in the price of Bitcoin. I learned about cryptographic hashes, digital signatures and public key cryptography. I also learned about Bitcoin mining, and the incentives that miners have to protect the network. I was interested in computer science and security for the first time in my life. It was great.

Set against a narrative of dystopian libertarian economics, it was underground developers (like Amir Taaki) versus central bankers in an epic global battle to save the world from the fractional reserve banking system. The blockchain revolution was better than fiction.

I consumed content on reddit, listened to Lets Talk Bitcoin and a lot of Peter Todd content. I lost money on BTC-e (once because I took advice from the trollbox). I argued with my friends Ethan Buchman and Zach Ramsay about technology. We learned about MasterCoin and the possibility of building systems of top of Bitcoin, taking advantage of its Proof-of-Work network effect. When I first heard about proof-of-stake (PoS) in the 2013 alt-coin scene (thanks PPCoin!), I thought it sounded like heretical voodoo magic. Replacing miners with coins seemed like an inherently strange thing to try to do. I ended up deciding that the long-range attack problem was fatal, and any solutions were going to involve developer checkpoints of one form or another (an opinion I learned from Peter Todd). Being a Bitcoiner in 2013 was one of the most intellectually stimulating experiences of my life.

In Janurary or Feburary 2014, I read about Ethereum for the first time. I watched Vitalik's youtube videos, and I met him in person at the Toronto Decentral Bitcoin Meetups. He obviously knew way more of the tech story than I did, so I became hooked in, this time on Ethereum. Ethereum was the promise of decentralization made accessible to me, someone without much background. It was general purpose smart contracts that could do anything, disrupt any centralized system. It could be and do so many things that it wasn't always clear to me what role ethereum would actually play in the blockchain ecosystem. The blockchain tech story (as I see it) took an exciting turn with Ethereum, and I got to be closer to the action :)

Having been invited by Russel Verbeeten at one of these meetups, Ethan and I went to the hackathon prior to the 2014 Bitcoin Expo in Toronto. (Vitalik taught me how to use Merkle trees at this event.) I was thinking about properly incentivizing and decentralizing the peer review system for a couple of weeks, having recently had a paper rejected from an academic journal. Ethan and I tried putting this kind of system together at the hackathon. Ethan did most of the hard work using pyethereum, while I very slowly put together the first GUI I ever made. We came in second place at the hackathon (after Amir's "Dark Market", which became Open Bazaar). We got to meet the whole Ethereum team at the Expo, and we got ourselves invited to the public Skype channels! Charles Hoskinson offered us jobs: It was then, in April 2014, that we started volunteering for Ethereum. We even got @ethereum.org email addresses.

So I got into the blockchain space because I got hooked on the Bitcoin tech story, and then on the Ethereum tech story. I then got hooked on the proof-of-stake tech story, which I now know to be very compelling. I'm going to share it, being as faithful as possible to the timeline and manner in which the parts of picture have been coming together, in an effort to help bring everyone up to speed on our efforts. It may take a few chapters, but story time ain't over 'til it's over.
<h2>Chapter 1: Slasher + Security Deposits: The move from naive proof-of-stake to modern proof-of-stake.</h2>
<h4>May 2014 - September 12, 2014</h4>
When Vitalik first expressed interest in PoS to me in May 2014, first over Skype and then at a Bitcoin conference in Vienna, I was skeptical. Then he told me about <a href="https://blog.ethereum.org/2014/01/15/slasher-a-punitive-proof-of-stake-algorithm/">slasher</a>, which I think he had come up in January 2014. Slasher was the idea that you could lose your block reward if you sign blocks at the same height on two forks.

This gave Vitalik the ability to directly tackle (and arguably solve) the nothing-at-stake problem. (For the uninitiated, the "nothing-at-stake" problem refers to the fact that the PoS miners best strategy is to mine on all forks, because signatures are very cheap to produce). It also opened up our imaginations to a new space of interactive protocols for disincentivizing bad behaviour.

Still, I did not feel very satisfied with proof-of-stake at this time (despite Vitalik telling me a couple of times that he thinks "proof-of-stake is the future") because I was really in love with proof-of-work. So during the summer I mostly worked on proof-of-work problems (ASIC-hard PoW, security sharing between PoW Chains via "Proofs-of-Proof-of-Work", neither to completion). But I did suggest the use of security deposits to a couple of contract developers on a couple of different occasions. This planted the seed for insights made on the fateful post-Ethereum-meetup night of September 11th 2014 (kudos to Stephan Tual for organizing + getting me to that event!).

Ethan Buchman and I stayed up late talking about proof-of-stake at the "hacker" instead of the "party" section of Amir Taaki's squat in London. I connected the dots and internalized the power of security deposits for proof-of-stake. This was the night that I became convinced that PoS would work, and that making it work would be a huge amount of fun. It was also the first time I experienced the surprising size of the PoS design space, through long arguments about attacks and possible protocol responses.

Since the early morning of September 12th, 2014 I have firmly advocated (to everyone who would listen) that blockchains move to PoS because it would be more secure. Amir Taaki was unimpressed by my enthusiasm for proof-of-stake. At least Ethan and I were having the best time.

The use of security deposits always significantly leveraged slasher's effectiveness. Instead of forgoing some profit X, a provably faulty node would lose a security deposit (imagined to be on the order of size X/r) on which the block reward X was to be paid as interest (at rate r).

You place a deposit to play, and if you play nice you make a small return on your deposit, but if you play mean you lose your deposit. It feels economically ideal, and it's so programmable.

<strong>Adding deposits to slasher meant that the nothing at stake problem was officially solved.</strong>

At least, I had made up my mind that it was solved to the point where we could no longer understand why anyone would want to build a proof-of-stake system without security deposits, for fear of nothing-at-stake problems.

Also on September 12th, 2014 I met Pink Penguin for the first time, due to an introduction from Stephan Tual. I breathlessly recounted my PoS insights made the night before. And after I respectfully declined a job from from Eris Industries (now Monax) that week, Pink Penguin began sponsoring this research! (Thanks &lt;3!!)

At this point in the story I was unaware of the other, multiple independent discoveries of the use of security deposits in proof-of-stake systems made by Jae Kwon, Dominic Williams, and Nick Williamson.

Stay tuned... the next chapter is about the central role that ideas from game theory played in setting the design goals that led to Casper!

--------------------------------

NOTE: The views expressed here are solely my own personal views and do not represent those of the Ethereum Foundation. I am solely responsible for what I've written and am not am not acting as a spokesperson for the Foundation.