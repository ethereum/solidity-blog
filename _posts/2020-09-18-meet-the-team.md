---
layout: post
published: true
title: 'Meet the Solidity team! 🧑‍💻👩‍💻'
date: '2020-09-18'
author: Solidity Team
category: Announcements
---

As you might know, Solidity is an **open-source community project** mainly developed and maintained by a core team. 

Today, we would like to introduce some of our team members and share insights into their professional background, which components of Solidity they mostly work on, what they would like to see in Solidity and in the ecosystem in future and more! Since almost all of our work happens on Github you can find each team member's Github handle next to their name. 

Before we dive in, it should be noted that we are incredibly grateful for all community contributions! Some of our core team members even started out as community contributors and it is needless to say Solidity is truly depending on its developer and contributor community for feedback, bug reports, feature requests, tooling and input on the language design. If you'd like to learn more about the wider community contributors, you can find a collection of our top contributors in the [Solidity 5th birthday post](https://blog.soliditylang.org/2020/07/08/solidity-turns-5/) as well as by browsing though the [Solidity Github repository](https://github.com/ethereum/solidity).

Now, without further ado, please meet some of the faces behind Solidity! We are lucky to call this small but international group of friendly and bright minds our team:

- - -

### Alex - [@aarlt](https://github.com/aarlt)

#### What is your professional background?

Before joining Solidity, I mainly worked for the automotive industry as a software engineer specialising in embedded systems. I worked on AUTOSAR - a standardised software architecture used within the automotive industry. I helped develop an AUTOSAR RTE - a runtime environment (or middleware) of AUTOSAR SWC’s (software components). I also implemented some basic AUTOSAR BSW modules. Beside this I worked on V2X based communication technologies. The term V2X is used to refer to “Vehicle to Everything” communication and includes vehicle-to-vehicle and vehicle-to-infrastructure and many more. These kind of communication technologies are particularly useful to increase safety and to support autonomous driving. With this technology vehicles are not just limited to use their own sensor data, but they can also access information sent by surrounding vehicles, infrastructure entities (e.g. traffic lights), pedestrians, etc.

#### What is your role in the Solidity team? On which parts of Solidity do you work the most?

I started to contribute to Solidity in my free-time from 2018 on. I joined the team in February of this year. Right now, I would not say that I’m working only on one specific part. In the past I was working on different things: I worked on the build-system, I did some work on the testing infrastructure, fixed some bugs, worked on the code-generator. Recently, I’m adding support for ewasm to our semantic tests. So additionally to executing our tests against a classic evm1 virtual machine, an ewasm virtual machine can be used to verify that the semantic of the generated evm1 code is the same as for the generated ewasm code.

#### What would you like to see in Solidity in the future?

I think Solidity needs to support compile-time evaluation of more complex expressions and functions as soon as possible. If a result can be already calculated at compile time, there is no need to execute that calculation in an expensive on-chain environment. Everything that could be done at compile-time should be done at compile-time.

- - -

### Alex - [@axic](https://github.com/axic)

#### What is your professional background?

I have a long-standing interest in open source. Before my "professional career" I have been an early contributor to MPlayer and [FFmpeg](https://ffmpeg.org), and thus my first paid roles also revolved around multimedia. Video encoding and streaming, mostly on embedded resource-constrained devices, to be precise. At some point I made a switch to financial software (payment systems) for a good number of years, unfortunately in a closed-source setting. This was a good background for working on Bitcoin startups before finally arriving at Ethereum late 2015 after Frontier was launched.

#### What is your role in the Solidity team? On which parts of Solidity do you work the most?

I keep myself busy mostly with design discussions and helping out on management related work as a *Co-Lead*. When time allows and I get the opportunity to code, then I usually find myself working on Yul or lately on the SMTChecker.

Since I am involved with other parts of Ethereum, I try to bridge the gap between other technical teams and Solidity.

#### What would you like to see in Solidity in the future?

Short-term: Compiling the Eth2 deposit contract, Uniswap V2, and Gnosis Safe with the new Yul IR, and to being able to "verify" them with the SMTChecker.

Long-term: It would be nice for the language to move on from the inheritance centered design. I really like some aspects of Rust after working with it, especially traits, enums, and partially the module system. Late last year I shared some example code of a hybrid Rust-Solidity language, but it would not be wise to blatantly follow Rust, considering our special requirements. We already have plenty of discussions regarding these topics, so I am very hopeful.

- - -

### Bhargava - [@bshastry](https://github.com/bshastry)

#### What is your professional background?

I graduated from Technische Universität Berlin with a Ph.D. in computer science. My Ph.D. thesis focused on developing techniques to find security vulnerabilities quicker in open-source software.

#### What is your role in the Solidity team? On which parts of Solidity do you work the most?

In the Solidity team, my role is to help the team find bugs and security issues as soon as possible, preferably before an upcoming release. More specifically, I develop semantic fuzzers for different compiler sub-systems e.g., compiler frontend, optimiser etc.

#### What do you like to do in your free time?

I enjoy finding security issues with popular open-source software implementations and blogging about my experiences on my personal blog over at https://bshastry.github.io/blog. I also enjoy tapping the closest piece of furniture to the beat of electronic music that is being played.

- - -

### Chris - [@chriseth](https://github.com/chriseth)

#### What is your professional background?

During my studies, I have worked on different small open source projects related to Wikipedia and biodiversity. After my PhD in Computational Complexity Theory, where I worked on multi-objective optimization and structural complexity theory, I gathered some experience in the classical closed-source software industry. In 2014, I started the Solidity project and really enjoy the open collaboration of the professional free software world that is very similar in spirit to the research community.

#### What is your role in the Solidity team? On which parts of Solidity do you work the most?

With help from Alex Beregszaszi I'm leading the Solidity team. This means that I'm doing a lot of code review and design work, but there is also enough time to write code. My favourite part of the project is the Yul optimizer because you can make a lot of impact with tiny but well-thought-out components.

#### What would you like to see in Solidity in the future?

To me, the biggest reward is seeing people building something helpful to humanity on top of your own work. By extension, I think we should make it easier to re-use safe and tested Solidity code created by others. Because of that, I think that features like generics or templates should be the next big focus for Solidity.

- - -

### Christian - [@christianparpart](https://github.com/christianparpart)

#### What is your professional background?

Over the years, I worked in various kinds of jobs, starting as a C++ developer in the printing world, to other languages, such as C#/.NET, C (on Solaris),
and later Ruby, but then also Linux data center related jobs, focusing on high availability related projects, and finally moving to mid-management.
Programming in C++ and compiler related topics is what I always enjoyed the most. So it felt natural to what was about to come next. During that path, one of the things I did is implementing a proprietary command line interpreter, as well as domain specific languages from language design to compiler implementation to custom virtual machine. I really enjoyed that, and now, I am very pleased to work on the Solidity compiler.

#### What is your role in the solidity team? On which parts of Solidity to you work the most?

While bug-fixing is always important, what I enjoy the most is the syntactical analysis. Also, the Yul IR is very interesting and I am looking forward to help pushing forward our Yul code generator. There were quite some house-keeping tickets that were fun touching,
but also moving the Compiler's C++ standard from C++11 to C++14, and just by the end of last year to C++17 was quite a push.
Besides that, I developed a proof-of-concept Solidity language server that I would love to be an official part of the Solidity project some time in the future.

#### What would you like to see in Solidity in the future?

In the past years, I had a dive into functional languages, especially F#. This experience deeply changed the way I am programming C++ today.
I would like to see some of the features that are usually found in functional languages to be also be present in Solidity, too,
such as: immutable-by-default, algebraic data types and pattern matching.

- - -

### Daniel - [@ekpyron](https://github.com/ekpyron)

#### What is your professional background?

I'm a mathematician by training, but already during my studies I started to work part-time as a software developer. Among others, I built the 3D graphics engine for an interactive real-time visualization project for live electronic music and the engine for a 3D mobile game, but also worked on large-scale molecular dynamics simulation software and in machine learning, before I joined the Solidity team (while I'm still part-time writing my Ph.D. thesis).

#### What is your role in the Solidity team? On which parts of Solidity do you work the most?

I think I have touched pretty much every single component of the compiler in one way or another by now. Lately, I have found myself reviewing more code than I actually write, but I still also like discussing and implementing new language features, helping in completing the new IR code generation path and in particular devising and implementing new optimizer steps for the Yul optimizer, but also basically anything else that comes up.

#### What would you like to see in Solidity in the future?

The Ph.D. thesis I'm working on is in "computational metaphysics" (i.e. the application of automated, computerized formal reasoning techniques to abstract philosophical theories), so I'm basically doing Formal Verification for philosophers. Leo is currently pioneering the integration of formal methods directly into the Solidity compiler with the SMTChecker, but we're also considering to empower the Yul optimizer with formal reasoning and I think there lie more interesting innovations ahead in that area, both for provably correct optimization or maybe even code generation in the compiler, but also as constructs in the language that help in reasoning about it, so I'd like to see more of that. Also `copyof`. And feature-complete Yul IR codegen and Yul memory objects.

- - -

### Đorđe - [@mijovic](https://github.com/mijovic)

#### What is your professional background?

My professional background is really interesting. I started at a small company developing casino games for the ItalyVLT market. Then I worked in the Microsoft Office Sway team. After that, I decided to try startup work environments in InsideMaps and Wolf3d. I have been working on computer vision algorithms in both companies. My blockchain journey started with joining the Solidity team in February 2020.

#### What is your role in the Solidity team? On which parts of Solidity do you work the most?

So far, I was mostly working on the IR part of Solidity, implementing new features for translating Solidity source to intermediate representation (Yul). Recently I started working on the SMTChecker (Solidity's formal verification tool) more, which is very interesting to me. Personally, I would like to see IR and SMT finished as soon as possible, covering all Solidity features.

#### What made you join the Solidity team?

Working on an open-source project for the first time was very motivating for me, especially combined with such an exciting and high potential environment like the blockchain space.

- - -

### Franzi - [@franzihei](https://github.com/franzihei)

#### What is your professional background?

After studying International Business Administration, I joined the innogy Innovation department working as a project manager and later portfolio manager with a focus on distributed technologies & blockchain (and what impact they can have on the energy sector). As part of that, I worked with several energy x blockchain projects like Co-tricity and Share&Charge. Following that, I became Head of Communications at brainbot, a blockchain development studio, after which I decided to join the Solidity team. :)

#### What is your role in the Solidity team? On which parts of Solidity do you work the most?

I'm supporting the team with communications and project management. This includes maintaining all communication channels like the @solidity_lang Twitter and the Solidity blog, organizing conferences and hackathons like this year's Solidity Summit and the upcoming Solidity Underhanded Contest as well as exploring all kinds of tools which facilitate a better exchange between the Solidity core-team and the wider Solidity ecosystem. These efforts mostly focus on fostering the language design process, e.g. with language design discussions and updates via the solidity-users forum.

#### What would you like to see in the Solidity ecosystem in the future?

We're building a strong Solidity ecosystem of tool builders, auditors, security researchers, super users and contributors which all regularly contribute to relevant language design discussions. I would love to see this ecosystem steadily grow and hope we find good interaction modes to strengthen the exchange between the different stakeholders. I also hope that we'll manage to constantly evolve and eventually define a transparent and lightweight process for the language design, which allows for collaboration and innovation.

- - -

### Hari - [@hrkrshnn](https://github.com/hrkrshnn)

#### What's your professional background:
I'm a recent graduate in Math (I like Geometry, Topology, Hyperbolic Geometry, etc.). During my studies, I also used to work on a Linear-Programming solver at the Zuse Institute.

#### What is your role in the Solidity team? On which parts of Solidity do you work the most?

Nowadays, I quite like working on the Yul optimizer. It is interesting since ultimately it's an open problem and allows for a lot of room for improvement.

#### What would you like to see in the Solidity ecosystem in the future?

It would be really nice to see new programming languages created for the EVM that target Yul. This would mean that a lot of Yul tooling becomes more important. Additionally, it would be interesting to see external research on improving, say, the optimizer, among other things.

- - -

### Kamil - [@cameel](https://github.com/cameel)

#### What is your professional background?
Over the years I worked in a variety of software projects and I don't really have one thing I could say I specialize in. My first job was at a video post-production company rolling out its own in-house editor for video post-processing and adding special effects. It was implemented in C++ and was one of the first programs of this type to take advantage of GPGPU capabilities back when it was still a novelty. Later on, I often worked on the backend side of multiple complex Python-based web apps, involving things like statistical modeling, video processing, chemical retrosynthesis or interfacing with blockchain. Along the way I had the chance to get familiar with network programming and admin/devops-oriented technologies, including shell scripting, web servers, containers and cloud services. Recently I've been particularly interested in blockchain-related technologies. So far, the biggest project in this space I took part in was a service designed and implemented for Golem. A few years ago I co-founded a software company called Code Poets where I'm currently responsible for the technological side. I still enjoy programming more than anything though. Having worked in various roles at different scales, I find that I like the low-level challenges much more than high-level system design or managing a project as a whole. Working as a part of the Solidity team is a good way for me to use my experience in a meaningful way while being able to learn more about the Ethereum ecosystem.

#### What is your role in the Solidity team? On which parts of Solidity do you work the most?

My biggest task so far was implementing the yul-phaser - an internal tool based on genetic algorithms that will help us fine-tune the Yul optimizer once the code generator is complete and supports all Solidity features. Apart from that I'm trying to get familiar with all parts of the compiler. So far, I worked mostly on IR code generator, wasm code generator and also on the supporting infrastructure for the repository, including the overhaul of the way older compiler binaries are hosted and nightly builds.

#### What do you think is Solidity's biggest challenge?

In my opinion one of the biggest challenges is getting a clear and cohesive vision of what the language should eventually look like. The language is still evolving and there are many planned features, some of which could fundamentally impact the way people write smart contracts. Templates/generics, contract invariants, Rust-style enums, variables being immutable by default, range types, range based loops, fixed point variables to name just a few. There are also some widely used patterns that may end up having support in the language at some point but right now it's still not clear if and how - upgradeable contracts are one example. Finalizing the language specification requires many tough decisions about what should and what shouldn't end up in it. Getting there will involve a huge amount of design work. Until it's done, we still rely heavily on being able to regularly introduce backwards-incompatible changes to avoid being left with a fossilized stub of a language where you have to fish for "the good parts" and ignore the rest. Fortunately, with the bigger team this year even the older design issues are finally getting some much needed attention and hopefully this gets us closer to releasing Solidity 1.0 in not so distant future.

- - -

### Leo - [@leonardoalt](https://github.com/leonardoalt)

#### What is your professional background?
I have an academic background in logic and theoretical computer science. During my PhD I worked on SMT solvers and logics with applications to symbolic model checking. Before joining the Solidity team I worked on formal verification (FV) of microprocessors.

#### What is your role in the Solidity team? On which parts of Solidity do you work the most?

I work mostly on the SMTChecker - a FV component inside the compiler that statically checks for assertion violations and other problems - and language design. I also work on Act and other topics as part of the Ethereum Foundation's Formal Verification team.

#### What do you think is Solidity's biggest challenge?

One of the main challenges is designing the language while not using it every day. Sometimes it's hard for us to get a feeling for what's wrong/bad/good in the language, and mass feedback is challenging to collect.

- - -

### Mathias - [@marenz](https://github.com/marenz)

#### What is your professional background?

I worked nine years for a data analytics company, mostly in the D programming language. One of my main projects was writing & maintaining a software that handled lots of data in real time and aggregated it in a database.

#### What is your role in the Solidity team? On which parts of Solidity do you work the most?

I don't have a fixed role. There isn't even a part I worked most on. I am touching almost all parts of Solidity except for the SMT code, that's still a new area for me.

#### What would you like to see in Solidity in the future?

I would love to see more attention on making the language more fun to work with while keeping it safe to use and easy to read and validate. Those two concepts sometimes clash with each other, but often they don't or a good compromise can be found.
  
- - -

**Want to hear more from us?**  
Follow [@solidity-lang](https://twitter.com/solidity_lang) on Twitter | Join the [solidity-users](https://groups.google.com/g/solidity-users) forum
