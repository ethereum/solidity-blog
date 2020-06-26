---
layout: post
published: true
title:  "Solidity v0.1.0 turns 5! A walk down memory lane..."
date:   2020-07-08
author: Franziska Heintel 
category: Announcements
---

![Solidity v0.1.0 turns 5](/img/2020/07/solidity_turns_5.jpg)

With happiness and a tad of nostalgia, we'd like to share that [Solidity v0.1.0](https://github.com/ethereum/solidity/commits/15dc5954c3a2e2a9ce96f2f77d41adef98a4cced) turns 5 years old today! (To be fair, v0.1.0 wasn't an *actual* release, but it marks the time where the Solidity team started appointing version numbers.) We are puzzled over how fast time flew by. We'd like to use this opportunity to take a look back and walk down the Solidity memory lane together with you.

**In short:** The Solidity language evolved rapidly, the ecosystem went through highs and lows, contributors came and went (but luckily most of them stuck around!) and we're still out here, still learning, still trying to push Solidity to the next, better stages.

Read on for a retrospective of the last five years of language development, thoughts on the language design process, an overview of people behind Solidity, and a short interview with Chris on the past, the present and the future of Solidity.

## Thoughts on 5+ years of language design

*Trivia, important milestones and views on the language development process from 5+ years of Solidity development, courtesy of Alex' recent [Solidity Summit talk](https://axic.github.io/notes/summit/musings_on_language/#/). Thank you!*

Developing a language when everything around you is moving can be quite challenging. Here's our view on why Solidity has been and continues to need to evolve rapidly - and why this is a good thing.

### 1. Goals and value proposition of a development language

Solidity's initial goal was to become a friendly, easy to use language with the aim to attract developers and make the entry-barrier to Ethereum development as low as possible.

That implied aspects like:
- JavaScript-like syntax.
- Allowing some weird implicit things.
- Lacking features, but being easy to learn.

Over the years, the goal changed. While still trying to stay as user-friendly as possible, the additional focus now rather lays on developing a much safer language.

For Solidity, that means:
- Solidity is verbose.
- Solidity is explicit.
- Solidity tries to highlight "risky" constructs (gas usage).

### 2. Embedding Solidity as a valuable and practicable language into the Ethereum ecosystem

The ecosystem we develop in was and continues to be a moving target. This isn't necessarily a bad thing, however, it requires some additional flexibility and adaptability:
- **Ethereum** in itself is constantly evolving and learning. Protocol and EVM developers are busy coming up with new features, new restrictions, repricings, even new protocols (Eth 2.0); most of which has an impact on the Solidity language design.
- **Developers** are still learning and frankly, sometimes trying all sorts of things. Here, from a language perspective, it's important to find the delicate balance between allowing too much, or too little.
- **The security community** developed and matured alongside with Ethereum and it took some time for it to reach a healthy size and to build the necessary tools supporting their work.

To summarize: Building Ethereum is a (learning) journey. And so is Solidity. Together with the broader Solidity community we're continuing to figure out:

- what features are needed or are bad.
- what is good or bad syntax.
- what is not enough or too much verbosity.
- how to deliver changes quickly to developers.

### 3. Steadily adjusting and improving the language design process

So far, questions around new features and implementation details have mostly been discussed in [Github issues](https://github.com/ethereum/solidity/issues), the solidity-dev [Gitter channel](https://gitter.im/ethereum/solidity-dev), and during the public Solidity development team meetings.

The first [Solidity Summit](https://solidity.ethereum.org/2020/06/09/solidity-summit-recap/), which was held virtually in May this year, was the next step to further expand the language design discussions and invite and engage a broader circle of experts, ranging from tool developers to Solidity power users, contributors, security researchers and auditors.

Currently, we're exploring more ways to interact with the aforementioned groups and include them better into the language design process. 
- We revived the solidity-users forum, where proposals for new language features and qualities of existing aspects of the language can be discussed.
- We're sharing (feature) feedback surveys on a semi-regular basis.
- We are hosting dedicated language design discussion calls, where one topic/issue/feature implementation is debated per call.

For the future, we could imagine evaluating a more structured process, similar to Python Enhancement Proposals (PEPs) or Vyper Improvement Proposals (VIPs), which could be introduced step-by-step. However, we think it might be a bit too early for that at this point.

In any case, we're happy to hear your thoughts on how we can improve the language design process to be more collaborative. If you'd like to get involved and share your ideas, feel free to join the [solidity-user forum](https://groups.google.com/g/solidity-users)!

## Highlights and milestones from v0.1.0 - v0.6.10

For a collection of notable Solidity events throughout the last years check out the roadmap below!

![Solidity Roadmap Part 1](/img/2020/07/solidity_roadmap_1.png)

![Solidity Roadmap Part 2](/img/2020/07/solidity_roadmap_2.png)

![Solidity Roadmap Part 3](/img/2020/07/solidity_roadmap_3.png)


## The people behind Solidity

A programming language is nothing without its community, its maintainers and the developers that build cool stuff with it! That’s why we would like to thank the core team and more importantly the community contributors for their continued support over all these years.

### The core team 

The Solidity programming language is an open-source, community project mainly developed and maintained by a core team. The core team is sponsored by the Ethereum Foundation.

This team currently consists of [@a3d4](https://github.com/a3d4), [@aarlt](https://github.com/aarlt), [@axic](https://github.com/axic), [@bshastry](https://github.com/bshastry), [@cameel](https://github.com/cameel), [@chriseth](https://github.com/chriseth), [@christianparpart](https://github.com/christianparpart), [@ekpyron](https://github.com/ekpyron), [@franzihei](https://github.com/franzihei), [@hrkrshnn](https://github.com/hrkrshnn), [@leonardoalt](https://github.com/leonardoalt), [@marenz](https://github.com/marenz), and [@mijovic](https://github.com/mijovic).

### The wider community & contributors 

We're incredibly grateful for all community contributions, not only via commits but also via participation in Github issues, by filing bug reports or in other community functions. Also a shout-out to our former team members!

We'd especially like to thank (in alphabetical order) [@arkpar](https://github.com/arkpar), [@asinyagin](https://github.com/asinyagin), [@benjaminion](https://github.com/benjaminion), [@bobsummerwill](https://github.com/bobsummerwill), [@chfast](https://github.com/chfast), [@ChrisChinchilla](https://github.com/ChrisChinchilla), [@Denton-L](https://github.com/Denton-L), [@djudjuu](https://github.com/djudjuu), [@elopio](https://github.com/elopio), [@erak](https://github.com/erak), [@ethers](https://github.com/ethers), [@federicobond](https://github.com/federicobond), [@fulldecent](https://github.com/fulldecent), [@gavofyork](https://github.com/gavofyork), [@ghallak](https://github.com/ghallak), [@guanqun](https://github.com/guanqun), [@jamesray1](https://github.com/jamesray1), [@jvmaia](https://github.com/jvmaia), [@LefterisJP](https://github.com/LefterisJP), [@liangdzou](https://github.com/liangdzou), [@LianaHus](https://github.com/LianaHus), [@mocamircea](https://github.com/mocamircea), [@NicolaiSoeborg](https://github.com/NicolaiSoeborg), [@pirapira](https://github.com/pirapira), [@random-internet-cat](https://github.com/random-internet-cat), [@roadriverrail](https://github.com/roadriverrail), [@sifmelcara](https://github.com/sifmelcara), [@VoR0220](https://github.com/VoR0220), [@winsvega](https://github.com/winsvega).

Note that this is a non-exhaustive list. There are certainly many more great contributors, however, listing all of them would go beyond the scope of this article. 😊


## The past, the present and the future of Solidity: Interview with Christian Reitwiessner

**Can you tell us a bit about the beginnings of Solidity? What changed most about your work on Solidity between then and now?**

*The biggest difference between now and then is that back in the days you basically did not know if somebody would ever use what we are building. Of course we maybe had the feeling it is going to be something big, but we did not know for sure. Now, Solidity is a product which we are continuing to improve while it is out there in production, being used, holding incredible amounts of value and powering some really cool decentralized applications.*

*Another difference is that in the beginnings, it seemed easier to get feedback. Maybe this was due to the fact that the community was much smaller. The conversations seemed more technical in a way. Now, the community became bigger and more diverse (which is a great thing!). With that it became a bit more complex in terms of collaboration and feedback especially on technical implementation details.*

**Looking back, what was your most memorable moment of the past 5 years of Solidity development?**

*There were many - people using the compiler before it even had a command line interface, people happily picking up the newest features, seeing the ecosystem grow and especially finally meeting people in real life you have been working with for so long.*

**What is your favorite aspect about Solidity?**

*Solidity looks rather high-level but is still very close to the EVM. At the same time, people with some background in programming usually understand what Solidity code is about.*

**Where do you think Solidity needs to improve the most?**

*It needs to be even easier to write safe smart contracts. This means we need to work on our SMTChecker, debuggers, IDEs, templates, libraries and so on.*

**What does Eth2 mean for Solidity?**

*On the technical level, it means we have to focus more on the Ewasm backend of Solidity, but this is a relatively straightforward task. On the conceptual level, on the other hand, it means that smart contracts will be asynchronous, which is a big challenge for everyone. The atomicity of the EVM makes many things very easy: If something is odd, just revert. Solidity needs to find a good language construct that makes the danger of asynchrony visible but also helps avoiding some pitfalls and allows the "common use-case" to be easy to write.*

**Anything else you would like to say?**

*Let's continue our journey to make Smart Contracts as safe and accessible to everyone as possible!*

With that being said, we'd like to wrap this post up and like to close with a virtual toast:

### To all the contributors over the last years: Your input, dedication and work has been incredible. THANK YOU. To the next 5 years! 🍾