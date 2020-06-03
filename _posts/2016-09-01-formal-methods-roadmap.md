---
layout: post
published: true
title: 'Dev Update: Formal Methods'
date: '2016-09-01'
author: Christian Reitwiessner
category: Announcements
---
_This post was originally published on the [Ethereum blog](https://blog.ethereum.org/2016/09/01/formal-methods-roadmap/)._

Today, I am delighted to announce that Yoichi Hirai ([pirapira](https://github.com/pirapira) on github) is joining the Ethereum project as a formal verification engineer. He holds a PhD from the University of Tokyo on the topic of formalizing communicating parallel processes and created formal verification tools for Ethereum in his spare time.

In his own words:

> I’m joining Ethereum as a formal verification engineer. My reasoning: formal verification makes sense as a profession only in a rare situation where
>
> - the verification target follows short, simple rules (EVM);
> - the target carries lots of value (Eth and other tokens);
> - the target is tricky enough to get right (any nontrivial program);
> - and the community is aware that it’s important to get it right (maybe).
>
> My last job as a formal verification engineer prepared me for this challenge. Besides, around Ethereum, I’ve been playing with two projects: an online service called [Dr. Y’s Ethereum Contract Analyzer](http://dr-y.no-ip.net/) and [a github repository](https://github.com/pirapira/evmverif/) containing Coq proofs. These projects are at the opposite extremes of a spectrum between an automatic analyzer and a manual proof development.
>
> Considering the collective impact to the whole ecosystem, I’m attracted to an automatic analyzer integrated in a compiler. Many people would run it and some would notice its warnings. On the other hand, since any surprising behavior can be considered a bug, any surprise should be removed, but computers cannot sense the human expectations. For telling human expectations to the machines, some manual efforts are necessary. The contract developers need to specify the contract in a machine-readable language and give hints to the machines why the implementation matches the specification (in most cases the machine wants more and more hints until the human realizes a bug, frequently in the specification). This is labor intensive, but such manual efforts are justifiable when a contract is designed to carry multi-million dollars.

Having a person dedicated to formal methods not only gives us the ability to move faster in this important but also fruitful area, it hopefully also allows us to communicate better with academia in order to connect the various singular projects that have appeared in the past weeks.

Here are some projects we would like to tackle in the future, most of them will probably be done in cooperation with other teams.

Solidity:
- extending the Solidity to Why3 translation to the full Solidity language (maybe switch to F*)
- formal specification of Solidity
- syntax and semantics of modal logics for reasoning about multiple parties

Community:
- creating a map of formal verification projects on Ethereum
- collecting buggy Solidity codes, for benchmarking automatic analyzers
- analyzing deployed contracts on the blockchain for vulnerabilities (related: [OYENTE tool](https://eprint.iacr.org/2016/633))

Tools:
- provide a human- and machine-readable formalization of the EVM, which can also be executed
- developing formally verified libraries in EVM bytecode or Solidity
- developing a formally verified compiler for a tiny language
- explore the potential for interaction-oriented languages ("if X happens then do Y; you can only do Z if you did A")
