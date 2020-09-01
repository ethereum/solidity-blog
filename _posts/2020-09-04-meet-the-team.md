---
layout: post
published: true
title: 'Meet the Solidity team!'
date: '2020-09-04'
author: Solidity Team
category: Announcements
---

As you might know, Solidity is an **open-source community project** mainly developed and maintained by a core team. 

Today, we would like to introduce some of our team members and share insights into their professional background, which components of Solidity they mostly work on and more. Since most of our work happens on Github you can find each team member's Github handle next to their name. 

It should also be noted that we are incredibly grateful for all community contributions! Some of our core-team members even started out as community contributors and it's needless to say Solidity is truly depending its developer community for feedback, bug reports, feature requests, tooling and input on the language design.

But now, without further ado, please meet some of the faces behind Solidity! We are lucky to call this small but international group of friendly and bright minds our team:

## Alex - [@aarlt](https://github.com/aarlt)

### What's your professional background?

*Before joining Solidity, I mainly worked for the automotive industry as a software engineer specialising in embedded systems. I worked on AUTOSAR - a standardised software architecture used within the automotive industry. I helped develop an AUTOSAR RTE - a runtime environment (or middleware) of AUTOSAR SWC’s (software components). I also implemented some basic AUTOSAR BSW modules. Beside this I worked on V2X based communication technologies. The term V2X is used to refer to “Vehicle to Everything” communication and includes vehicle-to-vehicle and vehicle-to-infrastructure and many more. These kind of communication technologies are particularly useful to increase safety and to support autonomous driving. With this technology vehicles are not just limited to use their own sensor data, but they can also access information sent by surrounding vehicles, infrastructure entities (e.g. traffic lights), pedestrians, etc.*

### What is your role in the Solidity team? On which parts of Solidity do you work the most?

*I started to contribute to Solidity in my free-time from 2018 on. I joined the team in February of this year. Right now, I would not say that I’m working only on one specific part. In the past I was working on different things: I worked on the build-system, I did some work on the testing infrastructure, fixed some bugs, worked on the code-generator. Recently, I’m adding support for ewasm to our semantic tests. So additionally to executing our tests against a classic evm1 virtual machine, an ewasm virtual machine can be used to verify that the semantic of the generated evm1 code is the same as for the generated ewasm code.*

### What would you like to see in Solidity in the future?

*I think Solidity needs to support compile-time evaluation of more complex expressions and functions as soon as possible. If a result can be already calculated at compile time, there is no need to execute that calculation in an expensive on-chain environment. Everything that could be done at compile-time should be done at compile-time.*

## Alex - [@axic](https://github.com/axic)




## Bhargava - [@bshastry](https://github.com/bshastry)

### What's your professional background?

*I graduated from Technische Universitaet Berlin with a Ph.D. in computer science. My Ph.D. thesis focused on developing techniques to find security vulnerabilities quicker in open-source software.*

### What is your role in the Solidity team? On which parts of Solidity do you work the most?

*In the Solidity team, my role is to help the team find bugs and security issues as soon as possible, preferably before an upcoming release. More specifically, I develop semantic fuzzers for different compiler sub-systems e.g., compiler frontend, optimiser etc.*

### What do you like to do in your free time?

*I enjoy finding security issues with popular open-source software implementations and blogging about my experiences on my personal blog over at https://bshastry.github.io/blog. I also enjoy tapping the closest piece of furniture to the beat of electronic music that is being played.*


## Christian - @christianparpart



## Chris - @chriseth



## Daniel - @ekpyron



## Đorđe - [@mijovic](https://github.com/mijovic)

### What's your professional background?

*My professional background is really interesting. I started at a small company developing casino games for the ItalyVLT market. Then I worked in the Microsoft Office Sway team. After that, I decided to try startup work environments in InsideMaps and Wolf3d. I have been working on computer vision algorithms in both companies. My blockchain journey started with joining the Solidity team in February 2020.*

### What is your role in the Solidity team? On which parts of Solidity do you work the most?

*So far, I was mostly working on the IR part of Solidity, implementing new features for translating Solidity source to intermediate representation (Yul). Recently I started working on the SMTChecker (Solidity's formal verification tool) more, which is very interesting to me. Personally, I would like to see IR and SMT finished as soon as possible, covering all Solidity features.*

### What made you join the Solidity team?

*Working on an open-source project for the first time was very motivating for me, especially combined with such an exciting and high potential environment like the blockchain space.*


## Franzi - [@franzihei](https://github.com/franzihei)

### What's your professional background?

*After studying International Business Administration, I joined the innogy Innovation department working as a project manager and later portfolio manager with a focus on distributed technologies & blockchain (and what impact they can have on the energy sector). As part of that, I worked with several energy x blockchain projects like Co-tricity and Share&Charge. Following that, I became Head of Communications at brainbot, a blockchain development studio, after which I decided to join the Solidity team. :)*

### What is your role in the Solidity team? On which parts of Solidity do you work the most?



### What would you like to see happening in the Solidity ecosystem in the future?




## Hari - [@hrkrshnn](https://github.com/hrkrshnn)

### What's your professional background:
*I'm a recent graduate in Math (I like Geometry, Topology, Hyperbolic Geometry, etc.). During my studies, I also used to work on a Linear-Programming solver at the Zuse Institute.*

### What is your role in the Solidity team? On which parts of Solidity do you work the most?

*I'm a software developer at Solidity. Nowadays, I quite like working on the Yul optimizer. It is interesting since ultimately it's an open problem and allows for a lot of room for improvement.*

### What would you like to see in the Solidity ecosystem in the future?

*It would be really nice to see new programming languages created for the EVM that target Yul. This would mean that a lot of Yul tooling becomes more important. Additionally, it would be interesting to see external research on improving, say, the optimizer, among other things.*

## Kamil - @cameel


## Leo - [@leonardoalt](https://github.com/leonardoalt)

### What's your professional background?
*I have an academic background in logic and theoretical computer science. During my PhD I worked on SMT solvers and logics with applications to symbolic model checking. Before joining the Solidity team I worked on formal verification (FV) of microprocessors.*

### What is your role in the Solidity team? On which parts of Solidity do you work the most?

*I work mostly on the SMTChecker - a FV component inside the compiler that statically checks for assertion violations and other problems - and language design. I also work on Act and other topics as part of the Ethereum Foundation's Formal Verification team.*

### What do you think is currently Solidity's biggest challenge?

*One of the main challenges is designing the language while not using it every day. Sometimes it's hard for us to get a feeling for what's wrong/bad/good in the language, and mass feedback is challenging to collect.*

## Mathias - [@marenz](https://github.com/marenz)

### What's your professional background?

*I worked nine years for a data analytics company, mostly in the D programming language. One of my main projects was writing & maintaining a software that handled lots of data in real time and aggregated it in a database.*

### What is your role in the Solidity team? On which parts of Solidity do you work the most?

*I don't have a fixed role. There isn't even a part I worked most on. I am touching almost all parts of Solidity except for the SMT code, that's still a new area for me.*

### What would you like to see in Solidity in the future?

*I would love to see more attention on making the language more fun to work with while keeping it safe to use and easy to read and validate. Those two concepts sometimes clash with each other, but often they don't or a good compromise can be found.*