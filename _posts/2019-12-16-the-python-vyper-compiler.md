---
layout: post
published: false
title: 'The Python Vyper Compiler'
date: '2019-12-16'
author: Piper Merriam
---

Earlier this year, a preliminary security audit was performed by the Consensys Diligence
team on the Python-based Vyper compiler.  [You can read the results for yourself here](https://diligence.consensys.net/audits/2019/10/vyper/).

We encourage you to read the report yourself, however, there are two main take
aways.

1. There are multiple serious bugs in the Vyper compiler.
2. The codebase has a high level of technical debt which will make addressing these issues complex.

The existing maintainers of the Python-based Vyper compiler are planning to
address the issues and we wish them the best of luck.  To continue following
the project, you can now find it here under the `vyperlang` github
organization.

> https://github.com/vyperlang/vyper

Here's the long story of how we got here.

The Vyper project has a complicated history.  It was originally authored by
Vitalik as proof-of-concept replacement for Serpent, its predecessor.  The idea
behind the language was to develop something that was designed at the language
level to naturally exhibit a high degree of safety.

Shortly after its creation Vyper found itself without a dedicated maintainer.
Luckily, there were enthusiastic community members that took up the torch and
continued development of the project.  Starting in early 2019 the Ethereum
Foundation python team re-involved itself in the project.  Over the course of
this year we worked with the project maintainers to try and focus on improving
the code quality and architecture of the project.  After a few months of work
we were skeptical that the python codebase was likely to deliver on the idea
that Vyper promised.  The codebase contained a significant amount of techincal
and architectural debt and from our perspective it didn't seem like the
existing maintainers were focused on fixing this.

Earlier this year in August, we decided to try and see what it would take to
produce a version of the Vyper compiler built on fundamentally different
achitecture.  The goal was to write a compiler in Rust that leverages the
existing work by the Solidity team and uses the YUL intermediate representation
to allow us to target EVM or EWASM during compilation.  A Rust based compiler
can be easily compiled to WASM making the compiler much more portable than one
based in Python **and** by building on top of YUL we would get the EVM and
EWASM compilation for free, only requiring the compiler to handle the
transformation from a Vyper AST to YUL.

Then, around Devcon, the Vyper audit linked above came out.  At this stage we
were sufficiently far along with our Rust based Vyper compiler to be confident the
direction was going to work well.  The audit mostly served to confirm things we
already suspected about the python codebase and seemed to validate the
direction we'd taken.

Upon speaking with the maintainers of the Python Vyper codebase we found they
intended to continue with the project.  At this stage I decided it was best to
separate the project from the Ethereum Foundation since we didn't plan to have
any continued involvement in the python codebase.  We didn't want the
association with the Ethereum Foundation to inadvertantly be received as
signalling that the project was safe to use.

So at present there are currently two *"Vyper"* compilers.  We at the Ethereum
Foundation are working towards building a compiler written in Rust to deliver
on the original idea of Vyper.  The existing Vyper containers are continuing to
work towards the same goals in the Python codebase.  We're hopefull that we can
find a way to continue together towards a single "Vyper" with multiple
implementations.
