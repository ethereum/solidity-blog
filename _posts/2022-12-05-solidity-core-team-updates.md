---
layout: post
published: true
title: 'Solidity Core Team Updates'
date: '2022-12-05'
author: Solidity Team
category: Announcements
---

More than two years have passed since we [introduced Solidity core team members](https://blog.soliditylang.org/2020/09/18/meet-the-team/) on the blog and we realized it is high time for some updates: Meet new team members, find out who moved on to other adventures and learn about recent changes in the team structure!

Before we dive in, a reminder that the Solidity programming language and compiler are **open-source community projects**. This post dives into the core team that leads the development. Nevertheless, we cannot stress enough how invaluable community contributions and interactions are for Solidity. We value feedback and input and want to thank our community for the continued support!

## Thank you to our alumni 💐

Like in any other project, there naturally comes a time for some team members to move on to other challenges. Since our last update, we have bid farewell to Christian (@christianparpart), Damian (@wechman), Đorđe (@mijovic), Hari (@hrkrshnn), Mathias (@marenz),  and Nishant (@nishant-sachdeva). We were extremely glad to have them and wish them all the best for their future! Luckily, some alumni remain in our orbit and continue to contribute with code and input to language design discussions.

## Introducing new team members 💝

On a happy note, we are excited to introduce new core team members who have joined us along the way. Please meet Matheus, Nikola, Nuno and Rodrigo!

In order to give you a chance to get to know them better, we asked all new joiners to respond to a selection of questions. Read their responses below. 

### Matheus - @[matheusaaguiar](https://github.com/matheusaaguiar)

#### What is your professional background?

_I have a BSc in Computer Science and a MSc in Artificial Intelligence. After finishing college, I worked as a C++ backend developer on a small educational game project for about 2 years. Subsequently, I worked as a Data Structure and Algorithms teacher for 8 years. I joined the Solidity team in May 2022 and am working on the Solidity Compiler since._

#### What made you join the Solidity team?

_After being a teacher for a fair share of years, I wanted to work with the industry on new emerging technologies and challenge myself. Besides that, I always wanted to work on an open-source project. It is really exciting to be learning and building with such great devs and user community._

#### What do you like to do in your free time?

_I enjoy playing digital games such as ~~cracktorio~~ Factorio and StarCraft II, but also like to gather some friends and play board games while drinking some beers 🍻. Other than that, I really like skateboarding, running and football of course 🇧🇷!_


### Nikola - @[nikola-matic](https://github.com/nikola-matic)

#### What is your professional background?

_I have a BSc in Comp Sci, and a ~10 year long obsession with C++, which kinda narrowed down my options as far as jobs were concerned, because no one's really gonna write a web app in C++. Having said that, I ended up working on a ton of interesting projects throughout my career, like writings high-perf server-client applications for SMS messaging, hardware virtualization on the cloud, EV routing, and a ton of other stuff. Unfortunately, up until I joined the EF and the Solidity team, I've never worked on an open source project, so this is a first for me (hopefully not the last)._

#### What is your role in the Solidity team? On which parts of Solidity do you work the most?

_I've only joined the team ~4 months prior to writing this, so not that much. I did implement the option for the user to pass the clean-up steps for the optimizer sequence (both CLI and JSON), so at least that's something. At the moment, and for the foreseeable future, I'll be working on helping Chris (chriseth) with speeding up the `--via-ir` pipeline, as well as getting the experimental version of the standard library merged._

#### What do you think is Solidity’s biggest challenge?

_Fundamentally, I think that we as the Solidity team don't use the language enough, which is especially concerning seeing as we design and implement it. This is something that the community has pointed out in the past as well, and is something we're well aware of. Thankfully, we are working on this, and plan to have internal team projects and hackathons in the future, which should hopefully give us a different perspective going forward._


### Nuno - @[NunoFilipeSantos](https://github.com/NunoFilipeSantos)

#### What is your professional background?

_I have a MSc in Software Engineering and all my life I have been highly motivated to work with open-source software. In recent years, I have worked for SUSE, the largest independent open-source company in the world, and Timescale, an open-source time-series database extension for PostgreSQL as a Product Manager. I am usually also an informal Agile coach for most of my teams with a heavy focus on execution and team happiness/motivation. I have been a remote worker for almost 10 years now and have been 100% remote since 2018._

#### What would you like to see in the Solidity ecosystem in the future?

_Solidity will start having a more community-friendly roadmap and I would love it if the community could engage with us on those issues, especially in our forum since they can positively influence our plans. Another personal motivation is improving developer ergonomics with better debugging tools or documentation._

#### What do you like to do in your free time?

📺 _Besides being a professional couch potato and a sucker for good movies and TV shows, I also occasionally do some light gaming._
🤖 _When I have the right inspiration I also work on my IoT projects in my house where I have to negotiate with the most demanding stakeholder ever, my wife._
⛳️ _I am also an enthusiastic amateur golfer, which means on a sunny, windless day I'm probably in the links doing some shots!_

#### What made you join the Solidity team?

📣 _The culture on this team is unlike anything I have ever experienced! We are the embodiment of radical candor by caring about the people we talk to and by not being afraid of saying what we mean, we do let the best argument win._
👨‍💻 _As a software engineer myself, helping shape the future of Solidity is very motivational for anyone that has ever had any interest in WEB3._

### Rodrigo - @[r0qs](https://github.com/r0qs)

#### What is your professional background?

*I have a BSc in Computer Science, and after my bachelor's, I worked for almost five years as a system administrator of High-Performance Computing clusters using mainly open-source software and developing all sorts of scripts and tools to manage the systems. Then, I decided that it was time to explore new areas and learn a new programming language, so I did an MSc in Distributed Systems. My MSc thesis focused on implementing and evaluating a Collision-fast Atomic Broadcast protocol based on a variation of the Distributed Consensus problem, which I implemented in Scala.*

*After my master's, I worked as a software engineer in a Brazilian IT company building an API Gateway and API Manager Platform that worked as an abstraction layer for legacy web services. The system was mainly implemented in Javascript, Ruby, and Go.*

*Over the years, I developed some small projects in Solidity in my free time, and eventually, in 2018, I decided to start a PhD in Distributed Systems to fulfill my desire to learn more about Ethereum. My PhD was focused on designing and developing a transparent and privacy-preserving verifiable credential system for academic diplomas that could work on a global scale. The core functionality was implemented in Solidity and Circom. Finally, in 2022, I had the opportunity to join the Solidity team.*

#### What is your role in the Solidity team? On which parts of Solidity do you work the most?

*I work primarily on Solc-js (a JavaScript wrapper around the Solidity compiler), task automation, and Continuous Integration scripts in the Solidity repository. I also collaborate with the development of some minor changes in the compiler, and I'm looking forward to contributing to more compiler features and optimizations in future.*

#### What made you join the Solidity team?

*Being part of the Solidity team sounded like an exciting challenge to me - a natural step for learning more about the Solidity language, and an excellent way to contribute and learn more about the Ethereum ecosystem.*

## Changes in Team Structure 🧑‍💻👩‍💻

We also want to use this post as an opportunity to share insights into recent updates to our team structure. We believe that the new team structure and processes will support us to productively deliver on our goals and focus on our priorities. Moving forward, the team leadership consists of a Technical Product Manager and a Tech Lead.

### Introducing Nuno as Solidity Product Manager 🎯

End of September, we were happy to onboard Nuno as our new Technical Product Manager. Nuno supports the team with leading people and processes, as well as facilitating the product/roadmap planning and overall development coordination. Nuno has worked with open-source projects all of his life and we are inspired to learn from and with him!

### Welcoming Daniel (@ekpyron) as new Solidity Tech Lead 🧭

Furthermore, we'd like to announce that one of our long-time Solidity core team members, Daniel, has stepped up to become Solidity's next Tech Lead.

Daniel started contributing to Solidity in February of 2018. He is a mathematician by training, but already worked as a part-time software developer ever since his studies. This year, he received a PhD in Computational Metaphysics.

Daniel demonstrated technical leadership throughout his work on the optimizer and viaIR pipeline in Solidity and has been a vital sparring partner for language design decisions since he joined the team.

We are glad to have him as a new Tech Lead and confident he is the perfect choice to continue Chris' legacy.

If you want to hear more from Daniel, consider checking out his talk on ["Generating EVM Bytecode from Yul in the new via IR Pipeline"](https://www.youtube.com/watch?v=RJQdycaEgIE) at this year's Solidity Summit or a more general update on ["Technical Details of the Solidity Compiler"](https://archive.devcon.org/archive/watch/6/technical-details-of-the-solidity-compiler/?tab=YouTube) at Devcon.

### Thank you to Chris (@chriseth) & Alex (@axic) for their work! 👏

As you may have guessed from the previous paragraph, Chris (@chriseth) has decided to step down from his Team Lead position in the Solidity team. We fully respect this decision and cannot thank him enough for his relentless work and dedication during the last 8 years. Chris, you have been and will continue to be an inspiration to the entire team!

To our benefit, we're happy that Chris remains a treasure in our team. Besides contributing to Solidity features, language design and reviews, he's also venturing into Formal Verification. If you're interested, check out his recent work on [Yools](https://github.com/leonardoalt/yools), an experimental symbolic execution engine for Yul.

In addition, we want to use this opportunity to also extend a big "Thank You!" to Alex (@axic) for his invaluable contributions as a long time co-lead of the Solidity team - a position he gradually stepped back from troughout the last two years.

Alex is currently leading the Execution Environment R&D team ([Ipsilon](https://notes.ethereum.org/@ipsilon/about)) at the Ethereum Foundation. He continues to contribute with language design input and discussions. He also recently gave a talk on [his vision for Solidity v1.0 and beyond](https://archive.devcon.org/archive/watch/6/what-would-solidity-10-and-20-look-like/?tab=YouTube) at Devcon.

### Contribute to the future of Solidity 💪

Last but not least, we are inviting **you** to join our language design efforts! We are adjusting the rhythm of our language design calls to make them more accessible to community contributors.

In the future, we will dedicate the first Wednesday team meeting of each month to language design topics. You can find a public rolling agenda of those calls [here](https://notes.ethereum.org/B89X_grPTli5wcCd0VqruA). Feel free to add your topics ahead of time.

You can find the call details in the ["Contributing" section](https://docs.soliditylang.org/en/latest/contributing.html) of the Solidity docs.

If you are not into language design but you are still eager to contribute to Solidity, there are many other ways!

You can get started by:
- Reporting issues
- Fixing and responding to issues tagged as “good first issue”
- Improving the documentation
- Translating the documentation into more languages
- Providing language feedback in the [Solidity forum](https://forum.soliditylang.org/)

All details on how to contribute can be found in the [docs](https://docs.soliditylang.org/en/latest/contributing.html).

We look forward to hearing from you on [Github](https://github.com/ethereum/solidity), [Matrix](https://matrix.to/#/#ethereum_solidity:gitter.im), or the [forum](https://forum.soliditylang.org/)! 🚀