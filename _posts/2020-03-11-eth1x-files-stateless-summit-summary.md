---
layout: post
published: true
title: 'The 1.x Files: Stateless Summit Summary'
date: '2020-03-11'
author: Griffin Ichiba Hotchkiss
category: 'Research & Development'
---

![The 1.x Files](https://blog.ethereum.org/img/2019/12/the1xfiles_black.png "The truth is out there.")



# The Stateless Ethereum Summit

It'd be a fools errand to try and provide a representative or objective summary *immediately* following this week in Paris -- I and everyone else whom were present shall be spending the coming weeks refining our takeaways, and adjusting for the year ahead.

But for you, dear reader, who felt the Paris FOMO and have been eagerly awaiting an update, I will provide my personal and incomplete collection of high-level insights, decisions, and results of the first Stateless Ethereum Summit.

## What was it like?

The summit was two days in duration, with a bare-minimum structure of first meeting as one large group to discuss large or important topics, then breakouts into two or three simultaneous discussions. With about 30 attendees overall, the group sizes were just about perfect to allow both deep dives and easy-going Q/A. It was also of course an opportunity to put faces with usernames, and connect on a more human level with the whole group.

I think that for most people attending (including myself) the primary result of the summit was a "leveling up" in our understanding of the problems that need to be solved, and the proposed solutions. The handful of people that have been leading this initiative (Piper, Alexey, and their teams) had the opportunity to give the rest of us some good old-fashioned white-board time to get caught up and to ask all the little questions we were afraid to ask about in a forum post.

I highlight this because one of the main goals of this gathering was to more clearly present both the opportunities and challenges of the work to be done. The more clearly that work can be articulated to everyone interested, the easier it will be to join the effort and contribute. I would say that in this regard the summit was already a resounding success, and we've "hooked" some folks who were sitting on the sidelines up until now.

## What was discussed?

Well, everything, really. With only one pair of ears I heard most topics from the [tech tree](https://blog.ethereum.org/2020/01/28/eth1x-files-the-stateless-ethereum-tech-tree/) being talked about in context, and as stated in the previous section, this summit was really about coming together to agree upon the simple shared vision for Stateless Ethereum. What is the core problem we're solving? What is the first reasonable milestone to work towards? Is it worth it to investigate a zero-knowledge scheme for historical witnesses?

Here's what I think were the main topics:

* Syncing primitives
* The transition to Binary trie
* EVM
* Data delivery in the stateless paradigm
* The draft witness specification

Alexey wisely commented that the purpose of this summit was to do all the things that *couldn't be accomplished* on the Internet, and to save the things that *can* be done online for when we're remote again. One thing that works much better in person than online is disagreement, and relatively quick decision-making over complex issues. So in addition to the general re-cap and knowledge-sharing about the core topics of discussion, there was an emphasis on using the time to make arguments for or against decisions that need to be made, such as what to work on first, or what new tools are needed before work can begin at all. Most importantly, this summit was an opportunity to narrow and better define the scope of this work, and to collectively get some sense of what success looks like from multiple perspectives.

## What was decided? What's new?

Again, and I can't stress this enough: This is just my personal brain-dump of how the summit went. I haven't even gone over my notes and recordings yet. But these are my takeaways, in no particular order. They are all new insights that came out of the weekend's discussion that will affect things going forward.

* Sync, and more specifically the primitive `getNodeData` is the key thing that must change in order to move forward with this stateless quest. It's something that must be fixed before the transition to binary trie can happen, and it will require coordination between all client teams. Felix from the geth team led a very productive discussion on sync, and it became encouragingly clear that most of the alternative proposals for sync seem to be getting at the same thing from different angles. Fixing and improving sync will allow for a smoother transition to a binary trie as well.

* While formerly it was thought that the sound transition strategy to a binary trie would require a momentary halt to the chain and a re-computing of a new binary state, the new thinking is that the transition can be accomplished without network interruption with sufficient client coordination.

* The plans and ideas surrounding the creation of a full-fledged Ethereum-specific data delivery network for state have been more or less dashed by a combination of new insights. The first is that we had people with more expertise weigh in to explain just how hard building something like that would be. The second is that such a network can be incrementally built up from improvements to sync, and a much simpler version (that only serves headers, transactions, and receipts, for example) would provide value immediately and could be upgraded at a later time.

* EVM changes are the most complex, and there was no clear decision or resolution with regard to what EVM changes will need to be made for stateless compatibility. The trick here is that most proposals under consideration actually do *more* than is strictly necessary for stateless, and it's a question of weighing the value|complexity|effort for those additional improvements. I suppose it's worth noting that some gas operations are expected to get more expensive no matter what, but nothing has really been determined with regard to the EVM, and we won't be able to know what the best course is until we get more data.  

* *WE MUST CONSTRUCT ADDITIONAL PYLONS* -- This is a nerdy way of saying that some of the work going forward is focused on making the work itself more productive and fruitful. This meta-work comes in two flavors: Tools that will make data collection and analysis easier, and resources to help others contribute more effectively, such as stateless-specific documentation for new researchers joining the party. That said, I believe there is still substantial disagreement about *how much* work should be devoted in the short term to tool-building, and which tools are needed most. Over the coming weeks, we're going to revise the tech tree and embellish it into something more representative of the initiative that Stateless Ethereum has become. This will serve the purposes of both helping the community keep track of everything, and to help interested newcomers contribute more effectively.

As always, if you have questions, requests for new topics, or want to participate in stateless Ethereum research, come introduce yourself on ethresear.ch, and/or reach out to @gichiba or @JHancock on twitter.
