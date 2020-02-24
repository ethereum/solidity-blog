---
layout: post
published: true
title: 'EF-Supported Teams: Research & Development Update'
subtitle: '2019 Pt. 2'
date: '2019-12-03'
author: Ethereum Foundation Team
category: ESP
---

<center><img src="https://blog.ethereum.org/img/2019/12/devcon-lights-edit.jpg" alt="Devcon Photo - Lights"></center>

Friends,

Since the last EF-Supported Teams report, progress has been made across the board. From improved network conditions, to the coming Istanbul release and Eth1.x and Eth2 development, all areas core to the functionality and sustainability of Ethereum are advancing.

This series focuses on teams and efforts from the Foundation and larger Ethereum ecosystem that are working to grow and improve Ethereum as a whole. In this edition, we're covering updates from many teams highlighted in [the previous report](https://blog.ethereum.org/2019/06/21/ef-supported-teams-development-report-2019-pt-1/), fully supported projects that are core elements of the Ethereum ecosystem like Eth2.0 Research, Geth and Solidity, and other ecosystem efforts.

Enjoy!

<div id="inline_toc" markdown="1">

#### Table of Contents
{:.no_toc}

* TOC
{:toc title="Table of Contents"}

</div>

## Aleth / C++ Ethereum

_Authored by Andrei Maiboroda_

The Aleth team was one of the client teams working on Istanbul upgrade to the Eth1.x chain, and [Aleth 1.7.2](https://github.com/ethereum/aleth/releases/tag/v1.7.2) was released with full support for Istanbul.

### EVM & other consensus

Important milestones for the evmone project:

After the initial release of this experimental fast EVM implementation, we focused on squeezing even more performance out of it. The [0.2.0](https://github.com/ethereum/evmone/releases/tag/v0.2.0) release is ~66% faster than the previous one. Following this, the [0.3.0](https://github.com/ethereum/evmone/releases/tag/v0.3.0) release brought evmone to compatibility with the Istanbul spec. For those more interested in the concepts evmone tries to explore and for more information about optimizing EVM, please see the slides from the [Optimization techniques for EVM implementations](https://docs.google.com/presentation/d/1MgXd159bmx37Q70Ng_ggHcClcYLafYqrxNEBFKAMqWI) Devcon5 presentation and the [Efficient gas calculation algorithm for EVM](https://github.com/ethereum/evmone/blob/master/docs/efficient_gas_calculation_algorithm.md#efficient-gas-calculation-algorithm-for-evm) article.

The EVMC project (the cross-language API for EVM implementations) has received required updates with support for Istanbul. All this is packaged as the [EVMC 7.0.0](https://github.com/ethereum/evmc/releases/tag/v7.0.0) release.

There were a number of optimizations in the aleth-interpreter to get rid of unnecessary state access in the implementation of some opcodes. These inefficiencies become apparent thanks to the great EVM test suite provided by [evmone project](https://github.com/ethereum/evmone).

The aleth-interpreter also switched from using boost::multiprecision to [intx library](https://github.com/chfast/intx). This is a step towards shipping aleth-interpreter without boost dependency, but also allowed to do some interesting benchmarks to see how inefficient boost implementation of 256-bit integer was for our needs.

After Istanbul, we opted to implement a couple of EIPs proposed for the future hard fork (Berlin): [EIP-1380](https://eips.ethereum.org/EIPS/eip-1380) Reduced gas cost for call to self and [EIP-2046](https://eips.ethereum.org/EIPS/eip-2046) Reduced gas cost for static calls made to pre-compiles. These implementations now can be activated in the testeth tool, see below

### Networking

We have implemented an optimization that has for a while been adopted by other mainnet clients: propagating new blocks to peers immediately after PoW check instead of waiting for a full validation and execution. We've also set a cap on the number of previously dropped transactions that are remembered by the transaction pool (now it's 1024 such transactions).

The client version reported during devp2p handshake was fixed, and allowed Aleth's version to be correctly shown on [ethernodes](https://ethernodes.org/client/aleth).

### RPC

We've made a number of fixes to better conform to input/output formats used in RPC interfaces of other clients. Many methods got a significant performance boost thanks to a fix getting rid of unnecessary block transaction re-executions. This may be noticed in use cases with many frequent RPC requests, for example when using [retesteth](https://github.com/ethereum/retesteth) with aleth.

### Database

Rebuilding the indices from existing blocks database was fixed and optimized, this will allow us to optimize and modify the layout of the index database in the future.

### Testeth tool

Consensus tests folder structure was reorganized by the testing team, and testeth now supports it - all the tests covering fork rules before Istanbul are in LegacyTests suite.

State tests can now be generated and run with a string like "ForkName+EIP_number" in place of a regular fork name in expect section. This allows for anyone planning to prototype new EIPs in aleth to generate tests for them before EIP is accepted for the fork - this is the basic idea of the [EIP-centric forking process](https://notes.ethereum.org/@holiman/S1ELAYY7S?type=view), being adopted currently by All Core Devs community. As an example of the mechanism, two new EIPS mentioned above (EIP-2046 and EIP-1380) can be activated in testeth, and we've created a couple of [state](https://github.com/ethereum/tests/commit/b7dfd94d252714f53557260421e52eed8d76c0e2)  [tests](https://github.com/ethereum/tests/commit/8bbbd484227684339ec64ed6a0083d0b137436c8) to illustrate this feature.

We've also fixed and streamlined the feature of testeth to run any custom test file (outside of predefined consensus test structure) and made it's output to be more conformant with the evm tool of go-ethereum. This allowed us to integrate testeth into [goevmlab](https://github.com/holiman/goevmlab) project, and now aleth's EVM implementation is participating in cross-fuzzing effort alongside 3 other major clients' EVMs.

Among other improvements:

- Better build process for those building aleth from sources.

- Improvements to docker image.

## Ecosystem Support Program

_Authored by ESP Team_

### Taiwan-Specific Grants

We recently [awarded](https://blog.ethereum.org/2019/11/14/Announcing-a-Taiwan-specific-Wave/) a round of five grants of $2,000-$5,000 at [Crosslink Taipei](https://crosslink.taipei/). This is the most recent in a series of local-specific waves intended to recognize the contributions of communities around the world.

### Growing Non-Financial Support

We're continuing to expand our definition of "support" for projects where a regular grant is not the right fit. Some of the non-financial support we've provided is feedback rounds with expert advisors, connecting teams that are working on similar things, AWS credit, invitations to participate in events and more.

### Website Improvements

Our new website [https://Ecosystem.Support/](https://ecosystem.support/) is growing! We've started small with a FAQ section and some changes to the front page - we'll follow with a grants showcase and dedicated blog for regular updates.

We don't want valuable skills going to waste, so we've revamped our inquiry forms to be a bit more open-ended, including a [dedicated pathway](https://ecosystem.support/explore/) for people who are interested in contributing to the ecosystem but aren't sure yet where they fit. Feel free to take a look around, and to apply!

## Ewasm

_Authored by Alex Beregszaszi and Paul Dworzanski_

Since the last update, the Ewasm team's focus has shifted towards research on Eth 2.0, working in close collaboration with other teams.

As the launch of Eth 2.0 phase 0 approaches, the phase 2 execution layer is under active development, in parallel with the development of phases 0 and 1. Multiple proposals have been made for the architecture of phase 2. The Ewasm team has been working on informing designs with prototypes and benchmarks, building upon a minimal foundation in Scout.

### Scout

The [Scout specification](https://ethresear.ch/t/phase-2-execution-prototyping-engine-ewasm-scout/5509) is a minimal interface for EEs (Execution Environments). This minimal interface is just enough to prototype stateless EEs, which are needed to validate the stateless model, and to inform the design of Ewasm and phase 2.

Scout has three implementations:

-   [scout](https://github.com/ewasm/scout) in Rust, designed for rapid prototyping and collaboration (it uses a Wasm interpreter with support for profiling),

-   [scout.ts](https://github.com/ewasm/scout.ts) in Typescript for rapid prototyping and browser support,

-   and [ScoutOne](https://github.com/ewasm/scoutone) in C++, designed for performance and production use, to be embeddable by Eth 2.0 clients.

### Execution Environments

Unlike the Eth 1 stateful model which has known scaling problems, Eth 2 is proposed to be stateless, where state is stored off-chain, and only a hash representing the state is stored on-chain, with witnesses passed as part of the transactions.

The stateless model presents new challenges. Prototypes and measurements are needed to validate its viability.

The Ewasm team has put great effort into prototyping and measuring stateless EEs, which we classify as follows:

1. An EE which must be compatible with Eth 1 data structures and execution.
   
   -   SMPT (Stateless Merkle Patricia Trie) using RLP to serialise the witness and transaction data, and using the Eth1 signature scheme.
   
   -   An EVM implementation in Assemblyscript.
   
   -   [biturbo](https://github.com/ewasm/biturbo) (previously known as TurboToken), using Multiproof to more efficiently encode witness data and also supporting EVM execution.

2. Designs without the need for backwards compatibility.
   
   -   [KMM (Katajainen Makinen Merkle) Token](https://github.com/poemm/stateless_kmm_token) EE which is optimized for witness size and execution time.
   
   -   Groth16 verifier implementation for supporting zk-SNARKs within Eth 2.
   
   -   STARK verifier implementation.

Of notable interest is the active research in the interaction between the Eth 1 and Eth 2 chains. To aid the evaluation of the "switchover" proposal, where Eth 1 is moved into a EE on Eth 2, the Eth 1 EEs mentioned above were prototyped. The team is also actively evaluating proposals bridging the two networks, and their implications on EE design.

Our ultimate goal is to provide a good developer experience for existing and new DApps.

This EE work has been feeding back into the design of Scout and Eth 2.

### Fast Cryptography

For Ewasm to be successful, we must execute expensive crypto on-chain. Luckily, crypto often has runtime bottlenecks in bigint arithmetic. First we have benchmarked various implementations of cryptographic primitives to identify bottlenecks. Then we designed a fast native bigint API to address these bottlenecks. Finally we augmented the highly optimised [websnark](https://github.com/iden3/websnark) library, in collaboration of its creators, to call this bigint API.

The results are encouraging: with this bigint API implemented in interpreters, we approach native speeds on elliptic curve operations (!), which are building blocks for lots of crypto. We can now execute pairings at near-native speeds. This is the biggest recent success story of Ewasm.

This work has allowed the EE prototypes above to operate within the performance constraints of Eth 2.

### Speed, Metering, Size

Ewasm has many other projects related to speed, metering (reducing metering runtime overhead and approximating runtime accurately), and bytecode size. From Wasm engine optimization, to metering analysis, to bytecode transformations, the Ewasm team is working hard to design the best execution system possible.

### Tooling

We are continuously working on tooling for Ewasm.

Bindings for Eth 2 and bigint APIs are provided for [Assemblyscript](https://github.com/ewasm/scout.ts/tree/master/assembly) and [Rust](https://github.com/ewasm/ewasm-rust-api).

If a Wasm bytecode requires augmentation, an extensible tool named [chisel](https://github.com/wasmx/wasm-chisel) is developed to provide various transformations (such as bytecode size reduction and adjustments of imports/exports) needed by both Ewasm and non-Ewasm use cases.

## Formal Verification

_Authored by Leo Alt_

The new Formal Verification team is working on tools, supporting other Foundation teams with formal models and proofs, and combining efforts with members of the Ethereum FV community.

Specifically recent work has focused on:

-   Solidity's SMTChecker, an unbounded model checker for Solidity smart contracts.

-   KYul, Yul semantics in K. KYul is further used to support the Solidity compiler by computing bisimulation proofs for non optimized and optimized Yul code.

-   Leading the development of a smart contract specification language with support from various members of the FV community. The specification language, used to describe contract properties, aims at being simple and supported by many FV tools.

-   Supporting the Eth2 research team and Runtime Verification in the Beacon Chain verification efforts.

-   Maintaining HEVM (together with Dapphub), a fully compliant Haskell EVM implementation.

-   Supporting the Testing team extending the coverage of the Ethereum tests with the gaps discovered in different EVM implementations.

## Geth

_Authored by Péter Szilágyi_

With the v1.9.0 release out in July, the Geth team has been mostly busy iterating on the existing code base; fixing anything issues discovered; and [prepping the client for the Istanbul hard fork](https://github.com/ethereum/go-ethereum/releases/tag/v1.9.8). Apart from these maintenance changes - totalling 8 releases - the team's also been laying the groundwork for some new things to come.

We've built up a treasure trove of 5 EIPs revolving around ENRs (Ethereum Node Records) to be used in networking and peer discovery. With the help of these, Geth nodes have been gradually extended to advertise a lot more information about themselves than the existing protocols allowed (IPv4 and IPv6 addresses, Ethereum network affiliation, configured and applied forks, light server capabilities, etc). The fork id work has already been deployed on top of the eth protocol, permitting Geth nodes to cleanly partition the network between incompatible machines. The records are also being indexed by a new discovery service exposed over DNS (not yet finalized), which makes eclipse attacks exponentially harder and will permit Ethereum to run in environments where UDP is locked down (DNS over HTTPS).

Performance wise, we are working on multiple fronts. On one end of the spectrum, we are trying to reduce the load on the network by making transactions (and maybe even blocks) propagate smarter; whilst on the other end we are working on a state snapshotter that can follow the live chain and act as an acceleration structure for EVM execution as well state sync. In between, we are working on various configuration options for Geth that would permit users to discard parts of the database that are of no use for them (without impacting network health), saving precious SSD capacity. These are all promising paths and we'll be sharing some numbers in the near future.

A significant amount of work went into the light client infrastructure too, permitting server operators to assign and manage client priorities and resource allowances via the RPC API. Whilst long term, light client incentivization is planned to work through the peer-to-peer protocol, the current feature set already permits an operator to gather payments outside of Geth and synchronize those with Geth's internal accounting. This should immediately allow anyone to create a paid light server service (though be aware, that this is at a prototype phase). Work is currently being done towards the fully P2P payment layer.

## Javascript Team

_Authored by: Samuel Furter, Holger Drewes, Marc Garreau, Everton Fraga, Richard Moore_

You might have already heard, as it was no secret, but will take the occasion of this EF Dev Report to officially announce: **the EF has formed a powerful new JavaScript team** which brings together the following well-established projects under one roof:

- [Web3.js](https://github.com/ethereum/web3.js).

- [Ethers.js](https://github.com/ethers-io/ethers.js/).

- [EthereumJS](https://github.com/ethereumjs/).

- [Grid](https://grid.ethereum.org/).

People from these different teams have already started to contribute across project lines and to [discuss interoperability issues](https://github.com/ethereum/js-organization/issues/1) and we expect strong synergies to unfold in the mid-term future. We used this first quarter as a new team to grow together, build trust and set up necessary organizational structures. Expect to hear more in 2020 when we will present and execute on a coherent strategy and vision to maximize our impact to support the Ethereum JavaScript/TypeScript developer ecosystem (you are very much invited to [join the discussion](https://github.com/ethereum/js-organization)). This will go beyond the reach of the former single projects.

The current projects will not be forgotten, however. We are very much aware that we have to care for and further develop on tools which are widely used within the community. So here are the respective updates to shine some light on what happened within these projects during the last quarter.

### Web3.js

We have released multiple patches for [Web3.js](https://github.com/ethereum/web3.js) since the last EF blog post and switched over to semantic versioning. Those patches added TypeScript support, extended the transaction signing functionalities, added the transaction confirmation workflow properties, added the long-outstanding JSON-RPC method ``getChainId``, added the ``connected`` event to the subscriptions, extended the provider interface with the method ``supportsSubscription`` and additional utility functions to work with bloom filters.

Further details about the new features and improvements can be seen in our release announcements on [GitHub](https://github.com/ethereum/web3.js/releases).

Currently, we're focused on reducing the bundle size, improving performance, adding reconnection for the `WebsocketProvider`, and improving the TypeScript DX.

On top of all of these great improvements, we've also had the chance to welcome [Chris](https://github.com/cgewecke/) to the EF-JS Team. Chris currently supports [Web3.js](https://github.com/ethereum/web3.js) development, but will be involved in all other EF-JS packages as well.

### Ethers.js

We've been getting [v5](https://github.com/ethers-io/ethers.js/tree/ethers-v5-beta) ready for public consumption, and adoption has been steadily growing. A huge thanks to everyone trying it out and reporting issues.

The focus on v5 has been adding extensibility and improving the API for framework developers, including a new framework, ethers-app, with a focus on dapp developers.

Since the number of new issues has dropped off, we expect to launch v5 to production quite soon, with just a few small changes in the pipeline and a couple of nodes left in the completely revamped documentation.

### EthereumJS

Most noteworthy on the [EthereumJS](https://github.com/ethereumjs) side are the releases of the different components targeting `Istanbul` support: the VM has gotten a larger [v4.1.0](https://github.com/ethereumjs/ethereumjs-vm/releases/tag/v4.1.0) update in September and we are currently [ironing out](https://github.com/ethereumjs/ethereumjs-vm/pull/607) the last bugs to make the VM fully compliant with the official test suite. Other updates to be mentioned in the `Istanbul` context are the releases on the [transaction](https://github.com/ethereumjs/ethereumjs-tx/releases/tag/v2.1.1), [block](https://github.com/ethereumjs/ethereumjs-block/releases/tag/v2.2.1) and [common](https://github.com/ethereumjs/ethereumjs-common/releases/tag/v1.4.0) (hardfork and network logic) libraries.

### Grid

Since the last EF blog post update, several major upgrades were made to [Ethereum Grid](https://grid.ethereum.org/). The app now lives in your operating system taskbar and provides a simple UI to download, configure and run Ethereum clients and tools. The plugin system continues to be refined with each new integration, but equally exciting are Grid Apps. Apps have been made available for testing RPC methods, querying block data via Geth's GraphQL implementation, signing transactions with Clef, and more. The team has been busy writing tutorials for what you can do using Grid, which you can find on the [Medium publication](https://medium.com/ethereum-grid/).

## Python Ecosystem [PyEVM/Trinity/Web3.py/Vyper]

_Authored by Piper Merriam_

### Web3.py

Recent work has been towards improved stability and documentation. Current focus is on adding async support for the library.

### Trinity

The Trinity client is working towards a beta release which would include the newly developed "Beam Sync" (https://medium.com/@jason.carver/intro-to-beam-sync-a0fd168be14a). We are also focusing on collaborative efforts with the broader ecosystem of client teams to try and tackle some of the larger problems like state bloat and figuring out a migration path for Eth 1.x into the 2.0 world.

### EthPM

We continue to focus on ecosystem tooling. The [`ethpm-cli`](https://ethpm-cli.readthedocs.io/en/latest/) continues to improve allowing installation of packages from various sources as well as constructing and publishing packages.

## Remix

_Authored by Yann Levreau_

Regarding Remix, we have quite a lot of updates to share. In recent months, our team has been at work:

-   Improving Remix Plugin and working with the communities in various ways - `@GrandSchtroumpf`.

-   Implementing a WebSocket Plugin for Edi Sinovcic's github integration.

-   Help Quorum to integrate their Remix plugins.

-   Work with Waffle (Ethworks) on their plugins.

-   Work with the VSCode ethereum team to integrate the plugin engine as a VSCode extension.

-   Switch loading plugin resources to be fully decentralized (IPFS for the moment).

-   Remix libraries: `@Aniket` joined the team recently for improving, maintaining and promoting the remix libraries repository <https://github.com/ethereum/remix>. That includes the critical work in remix-debug (transaction debugging), remix-tests, remix-solidity, remix-analyzer (new module added: warning for ether transfer in loop)

-   Remix libraries: finalizing remix-simulator and adding more tests to remix-debug `@iurimatias`

-   Improving the IDE's file explorer for supporting folders and standard features. Working with Ethworks on new themes. Using the Monaco editor. Set compiler version according the Solidity pragma - `@Lianahus @ryestew @Aniket @GrandSchtroumpf`

-   We introduced Remix desktop here <https://medium.com/remix-ide/remix-desktop-8c1e9e946ee1>, and we are now finalizing it. Keep an eye out for a release very soon! - `@yann300 @lianahus`

-   Remix Workshop is a plugin running within the Remix plugin API. 

It allows for creating tutorials, registering tutorials, and for students and learners to practically run tutorials. The scope of tutorials being made is very broad (Solidity, Vyper, general Etherum concept, etc...). It depends on the tutorials creators! The first POC was successful and we are now moving toward releasing a first version with the help of different teams from the community for receiving advice and feedback. -  `@GrandSchtroumpf @ryestew`

-   Workshops and outreach beyond the community: one aspect of our work is also to contribute in the educational effort through various ways. We expect this to take more importance in 2020. - `@ryestew @Aniket @GrandSchtroumpf @team`

-   Organize workshops / meetups (ESCE, Consensys, Devcon, Dappcon).

-   Meet with organisations and people from outside of the community. Not necessarily to onboard them technically, but more to give a first impression of what is blockchain, ethereum and more.

-   Practically build a tool for that (see Remix workshop)

## Research [CBC]

_Authored by Aditya Asgaonkar_

For the Casper CBC team, our focus as of late has been on:

1.  Describing the minimal CBC Casper protocol along with validator strategies for liveness in a unified framework (named Valid Labeled State transition & Message production system, VLSM). Public document is WIP, will be released soon.

2.  Formal verification of VLSM

3.  Using CBC Casper elements to improve the Eth2.0 design, such as <https://ethresear.ch/t/cross-shard-messaging-system/6201>

4.  Community outreach efforts such as this AMA: <https://www.reddit.com/r/ethereum/comments/dsiz9j/ama_we_are_the_cbc_casper_research_team/>

Stay tuned for more soon!

## Research [Plasma]

_Authored by Plasma Group_

Since May, we have been hard at work furthering advancements in scaling technology. There are 4 different teams in the ecosystem building out the Generalized Plasma spec for multiple blockchains, including OmiseGO, Matic, Cryptoeconomics Lab and Plasm. As many of our peers are working hard on production payments, we began to look into slightly less researched areas for scaling, such as application development and general composability. Developments on this front include the Optimistic Virtual Machine - a universal dispute language for layer2 constructions, as well as our demo with Uniswap for Devcon5 - a scalable payments and exchange game that can be found at [Unipig Exchange](https://unipig.exchange). The game is built on an optimistic rollup chain - a design which emerged from conversations with Barry Whitehat & Vitalik at the Scaling Ethereum conference in early June.

As we wrap up the end of the year, we are preparing to push out a paper for the OVM, as well as writing up more in-depth documentation for Optimistic Rollup. For now, those who are interested in learning about it can find a description of it on our blog where we describe how to build it for arbitrary smart contracts, along with some early documentation outlining the path from plasma to optimistic rollup on:

Our Forum: <https://plasma.build/t/rollup-plasma-for-mass-exits-complex-disputes/90/1> 

On Github: <https://gist.github.com/karlfloersch/1bf6ab7871f41e3a5a921c0a007ad5c6> 

Plasma Call: <https://youtu.be/5RpYoU6xD_M?t=1136>

## Research [Serenity / Eth2]

_Authored by EF Team_

Following Devcon5, Danny and the Eth2 research team began work on a weekly series of Eth2 Quick Updates, and recently on a series focusing on validating in Eth2. For the latest news and progress as we draw nearer to the launch of Phase 0, see the links below, and stay tuned to the [EF Blog](http://blog.ethereum.org)!

In general, progress continues toward Phase 0 testnets and mainnet launch. Phase 1 specifications and prototypes move in parallel, while Phase 2 continues active and fruitful R&D.

[Validated: Staking on Eth2 #0](https://blog.ethereum.org/2019/11/27/Validated-Staking-on-eth2-0/) - 2019-11-27

[Eth2 Quick Update #4](https://blog.ethereum.org/2019/11/21/eth2-quick-update-no-4/) - 2019-11-21

[Eth2 Quick Update #3](https://blog.ethereum.org/2019/11/08/eth2-quick-update-no-3/) - 2019-11-08

[Eth2 Quick Update #2](https://blog.ethereum.org/2019/10/31/eth2-quick-update-no-2/) - 2019-10-31

[Eth2 Quick Update #1](https://blog.ethereum.org/2019/10/23/eth2-quick-update/) - 2019-10-23

## Security [Security / Consensus Tests]

_Authored by Martin Holst Swende_

On the security side, there has been a lot of action regarding the Istanbul hard-fork. The old python-based fuzzer (Evmlab) [has been rewritten in Go](https://github.com/holiman/goevmlab), and has been used to create EIP-targeted fuzzers. These fuzzers have been used to generate test-cases (finding implementation flaws in both Besu and Nethermind) and used to run millions of test-cases comparing Geth and Parity - and as of late November, we also got Aleth and Nethermind VMs running on the same fuzzer framework. So we are now up to four EVMs doing differential fuzzing!

Meanwhile, we are also running libfuzzer-based fuzzers on Geth and Parity, an effort that is being led by `@cryptomental`.

A while ago, it was announced on the Bounty page that $15K USD was allocated towards security audits of EIPs. Out of this, we have awarded $5K each to Neville Gretch [contract-library.com](https://contract-library.com/) and Hubert Ritzdorf (Chainsecurity) for their work helping to assess the security impact of EIP-1884.

Several other bounties have been awarded, most of which will be shared publicly soon.

## Solidity

_Authored by Christian Reitwiessner_

The Solidity language and compiler continues to stabilize and add features requested by the community. These include options to output Solidity code for use in a variety of flexible formats, stability and security changes. The team is working on a new 0.6.0 release as well as updates to the 0.5.x brach.

### Webassembly

Solidity supports experimental preview output of webassembly code using the --ewasm switch. We extended most of the Yul optimizer stages to cope with eWasm code, are working on the glue code that translates EVM-flavoured Yul to eWasm-flavoured Yul and have a working prototype for eWasm binary code generation which is needed to deploy contracts.

### Solidity 0.6.0

We are almost finished with implementing breaking changes and can hopefully release Solidity 0.6.0 later this year. Some of the new changes include:

-   Require explicit "virtual" and "override" keywords for overriding functions.

-   ABIEncoderV2 is no longer experimental.

-   A fallback function is split into a "receive ether" function and an actual "fallback" function.

-   Abstract contracts have to (and can be) marked "abstract".

-   Structs and enums can be defined at file-level.

-   Disallow setting the length of a storage array arbitrarily.

-   Support push() to add new default-initialized element to dynamic storage array.

-   Add "leave" statement to Yul / Inline Assembly to return from current function.

-   Support multiple return values in NatSpec.

-   Better error message formatting on the commandline.

-   Metadata hash is now IPFS by default and can be switched to Swarm or removed.

-   Allow "revert reason strings" to be removed from the binary.

### SMT Checker

The SMTChecker has a new model-checking engine that supports loops and allows checking assertions considering an unbounded number of transactions. Read more information about the changes here: <https://medium.com/@leonardoalt/smtchecker-toward-completeness-1a99c02e0133>. 

We are currently working on supporting function calls in the new engine, which will enable multi-contract analysis even when the called code is unknown.

### Yul Optimizer

The Yul optimizer can now take side-effect-free-ness of user-defined functions into account and thus optimize across such function calls. It is able to remove redundant sload and mload calls and can take conditional local values of variables into account.

Compiler Interface:

-   If standard-json is used, the compiler only generates bytecode for the selected contract or stop after parsing and analysis if no bytecode is requested.

-   The option --error-recovery can be used to recover from most parser errors so that you can create something like an AST also for invalid input.

Apart from the changes listed above, we implemented numerous small bugfixes and features.

## State Channel Research

_Authored by Liam Horne_

Over the last several months Ethereum's state channels R&D community has made rapid progress.

Most excitingly, state channels are live on mainnet. [Connext](https://medium.com/connext/connext-v2-0-is-on-mainnet-b818864d3687), a micropayments service built on top of our work, has been live in production since September 2019. The scalability and UX enhancements brought by state channels are no longer theoretical, they're benefiting users right now. [Go try it out!](https://daicard.io/)

Behind the scenes, R&D has been busy over the last 6 months. This summer, the two primary state channel research groups - Counterfactual and Magmo - unified their work into a single project and protocol, simply called "StateChannels". This unification has allowed us to move at a faster rate, and also provide an easier experience for Ethereum's application developers, who do not need to think about which channels standard they intend to support.

More specifically, over the last few months we have:

-   Completed [merging CF and Magmo codebases](https://github.com/statechannels/monorepo).

-   Implemented a Client API documented [here](https://app-wallet-interface.netlify.com/#initial-api) and documented the protocol [here](https://nitro-protocol.netlify.com/).

-   Re-wrote our Solidity contract codebase to yield significant gas savings for both the "happy case" (612K → 165K) and the "challenge case" (1100K → 200K).

-   Witten the [ForceMove protocol in a formal specification language](https://github.com/magmo/specs/blob/force-move/ForceMove/ForceMove.tla) called TLA+. This allowed us to find many interesting optimizations to the protocol and identify unintuitive attack vectors. [You can read more about that on our research forum](https://research.statechannels.org/t/channel-checkpointing-in-forcemove/24).

### What's next for state channels?

-   We're working on 2 demo applications, built entirely on top of the Client API and run in the browser through our reference hub.

-   On-boarding new projects, recruiting new contributors, and continuing to make state channels extremely developer friendly.

## ZoKrates 

_Authored by Jacob Eberhardt_

We're excited to share a significant new update on progress toward making ZoKrates a powerful, yet user-friendly toolkit for zkSNARKs on Ethereum.

Great news for ZoKrates developers: In-browser development of ZoKrates code is now supported in [Remix](https://remix.ethereum.org/). You can find the ZoKrates plugin through the plugin manager in the left tab.

A long standing request was a richer type system, and with our last release, we shipped exactly that: ZoKrates now supports complex user-defined types in the form of structs as well as multi-dimensional arrays. To enable seamless interaction with ZoKrates programs using these new types from the outside world, we've added a JSON-ABI, which allows easy programmatic access.

In an effort to make ZoKrates more readable, we refactored our module system and changed file endings of ZoKrates source code files to `.zok`. Internally, the parser re-implementation based on a formal DSL grammar ---mentioned in our last update post--- was completed successfully.

Finally, more optimizations were introduced on compiled programs to reduce execution and proof generation time. To educate the community, we presented these results as well as applications using ZoKrates at Devcon V in Osaka!
