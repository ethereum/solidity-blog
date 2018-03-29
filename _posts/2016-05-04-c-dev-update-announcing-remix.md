---
id: 2677
title: 'C++ DEV Update: Announcing Remix'
date: 2016-05-04T19:22:21+00:00
author: Christian Reitwiessner
layout: post
guid: https://blog.ethereum.org/?p=2677
permalink: /2016/05/04/c-dev-update-announcing-remix/
dsq_thread_id:
  - "4799926451"
---
After almost three months into the "reboot" of the C++ team, I would like to give an update about the team itself, what we did and what we plan to do.

<strong>Team update</strong>

The so-called C++ team currently consists of Paweł Bylica (@chfast), Greg Colvin (@gcolvin), Liana Husikyan (@LianaHus), Dimitry Khokhlov (@winsvega), Yann Levreau (@yann300), Bob Summerwill (@bobsummerwill), me (@chriseth) and (kindly "donated" by Eris Industries) RJ (@VoR0220).

Paweł is the original author of the llvm-based EVM-to-native just-in-time compiler, re-joined in April and will continue improving the JIT.

Greg joined in February and already achieved substantial speedups for the C++ implementation of the Ethereum Virtual Machine, using his experience from implementing the Java Virtual Machine for his former employer Oracle.

Liana and Yann are working on Solidity and its IDEs (yes, plural, see below!).

Dimitry is in charge of the consensus tests and is also working on the C++ core.

Bob joined in February (having been part of the community for a longer time) and is currently leading the ambitious effort of disentangling the C++ codebase. He was also a major contributor to the <a href="https://ethereum-homestead.readthedocs.io/en/latest/">homestead guide</a>.

RJ joined Eris industries in March and is working on the Solidity compiler.

Furthermore, the coordination and co-operation between the C++ and the Go teams is on a completely different level than before. One of the reasons why we improve the interpreter is to get a water mark for the go-ethereum interpreter and lessons learnt during that process will directly feed into the go interpreter, just to name one example.

<strong>Announcing Remix</strong>

Developing Solidity itself and providing resources and tools for people writing smart contracts and dapps is one of the largest areas of work for the C++ team. An essential such tool is a debugger for Solidity and the Ethereum Virtual Machine so that developers can "look inside" the virtual machine and find the exact spot in their code that is not doing what they expect it to do. Our IDE <a href="http://ethdocs.org/en/latest/contracts-and-transactions/mix.html">Mix</a> is a wonderful piece of software that provides exactly this. Unfortunately, most people do not use it and prefer <a href="http://github.com/chriseth/browser-solidity">browser-solidity</a> or just some unit testing tools.

This is understandable, people do not want to switch editors (I guess this is also why we got a vast number of Solidity plugins for existing IDEs in the past months) or install extra software. Furthermore, the relative amount of external contributions we received for the html5+js-based minimalistic IDE browser-solidity compared to C++/Qt-based Mix is just overwhelming.

Because of that and also in an effort to increase modularity, reusability and openness, we decided to rethink the way we want to provide developer tools: With the <a href="https://github.com/ethereum/remix">remix project</a> we will create a set of reusable html5+js modules for developing and debugging smart contracts.

This means that it will be possible to integrate a debugger for EVM and Solidity into browser-solidity (which will also move to a more prominent place in the future), but also into visual studio code, atom, sublime, basically any IDE that is html5+js-based. You will even be able to fire up the debugger inside Mist, also for transactions in the past!

It is probably a bit too early to try out remix, but if you want, follow the instructions in the repository, but be sure to use the latest develop version of cpp-ethereum as backend node.

For everyone else: Here is a screenshot of an early proof of concept version:

<a href="https://blog.ethereum.org/wp-content/uploads/2016/05/remix.png"><img src="https://blog.ethereum.org/wp-content/uploads/2016/05/remix.png" alt="Screenshot of an early version of remix" /></a>

<strong>Other Tasks</strong>

Concerning the current focus for other projects, we are improving the runtime performance of the virtual machine. Greg already made great progress in that area and still has a lot of ideas. We are currently setting up general benchmarks, so that we can compare the performance of different implementations and the difference between interpreters and just-in-time compilers. We plan to make the just-in-time compiler available to other implementations like py-ethereum and of course go-ethereum.

For Solidity, the main areas of work are currently fixed-point types, structs as part of the ABI and extending the usefulness of libraries via "inlineable" functions and templates. Furthermore, we would like to invite the community to write and publish useful libraries. Special thanks to Alex Beregszaszi (@axic), Nick Johnson (@Arachnid) and Andreas Olofsson (@androlo) for making a great start there!

Finally, we want to reduce the pain that is currently caused when working with the C++ codebase, especially due to external and intra-dependencies. We are already almost at the point where Solidity can be compiled in isolation and the goal is to move back to our old home, the ethereum/cpp-ethereum repository, splitting parts off only where it makes sense, namely for Mix, Solidity and EVMJIT.