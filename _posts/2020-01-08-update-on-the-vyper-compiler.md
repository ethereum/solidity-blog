---
layout: post
published: true
title: 'Update on the Vyper Compiler'
date: '2020-01-08'
author: Piper Merriam
---

![Eth wallpaper](https://blog.ethereum.org/img/2019/09/eth-wallpaper.jpg)

The idea behind the Vyper Project was to develop something that was designed at the language level to naturally exhibit a high degree of safety. The project was originally authored by Vitalik as a proof-of-concept replacement for Serpent, its predecessor, but shortly after its creation Vyper found itself without a dedicated maintainer. Luckily, there were enthusiastic community members that took up the torch and continued development of the project, and we (the EF Python Team) became re-involved in the project for some time earlier this year.

This fall, a preliminary security audit was performed by the Consensys Diligence team on the Python-based Vyper compiler. [You can read the results for yourself here](https://diligence.consensys.net/audits/2019/10/vyper/). 

We encourage you to read the report, however, there are two main take-aways.
1. There are multiple serious bugs in the Vyper compiler.
2. The codebase has a high level of technical debt which will make addressing these issues complex.

Since the existing Python-based Vyper implementation is not yet production ready, it has been moved out of the ethereum github organization into its own organization: vyperlang. The existing maintainers are planning to address the issues independently once again, but we will  continue to follow the project closely here: > https://github.com/vyperlang/vyper

Meanwhile, our team continues work on a Rust-based compiler in tandem. More on that below, but first, here’s a bit more on how we got to where we are today.

Over the course of this year we worked with the project maintainers to focus on improving the code quality and architecture of the project. After a few months of work we were skeptical that the python codebase was likely to deliver on the idea that Vyper promised. The codebase contained a significant amount of technical and architectural debt, and from our perspective it didn't seem like the existing maintainers were focused on fixing this.

### Exploring Rust
Earlier this year in August, we explored producing a version of the Vyper compiler built on fundamentally different architecture. The goal was to write a compiler in Rust that leverages the existing work by the Solidity team and uses the YUL intermediate representation to allow us to target EVM or EWASM during compilation. A Rust based compiler can be easily compiled to WASM, making the compiler much more portable than one based in Python. By building on top of YUL we would get the EVM and EWASM compilation for free, only requiring the compiler to handle the transformation from a Vyper AST to YUL.
We were sufficiently far along with our Rust based Vyper compiler when the Python Vyper audit was released, and were confident in the directionl. The audit confirmed many concerns around the python codebase and helped to validate the direction we've taken.

### The work continues
That said, the maintainers of the Python Vyper codebase do intend to continue with the project. While we don’t plan to have continued involvement in the python codebase, we wish them luck but also wanted to make note of recent events to avoid inadvertently signalling that the project was safe to use.

So at present there are currently two "Vyper" compilers: The EF-supported work towards building a compiler written in Rust to deliver on the original idea of Vyper, and the Python effort which will work independently toward the same goals in the Python codebase. We're hopeful that we can continue working together towards a single "Vyper" with multiple implementations, and we’ll keep everyone up to date as the project moves forward. 
