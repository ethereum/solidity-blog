---
layout: post
published: true
title: 'EF-Supported Teams: Development Report'
subtitle: '2019 Pt. 1'
date: '2019-06-21'
author: Ethereum Foundation Team
---

<center><img src="https://blog.ethereum.org/img/2019/06/ethdenver.jpg" alt="EthDenver Photo"></center>

Friends,

Over the last year, developers and researchers supported by Ethereum Foundation have kept their heads down and continued to produce at a staggering pace.

Their focus has remained on their work, and together we're all building a more complete Ethereum. Today, our regular all-team update series is relaunching in a changed and ever growing ecosystem. Ethereum features larger dApp, developer and user communities than ever before, the network has continued to improve, and steady progress has been made on initiatives large and small.

While many teams do release their own well-rounded updates, we thought it appropriate to bring the community the latest from many of the teams (some of whom are softer spoken) supported by the Foundation. Enjoy!

<div id="inline_toc" markdown="1">

#### Table of Contents
{:.no_toc}

* TOC
{:toc title="Table of Contents"}

</div>

## Aleth / C++ Ethereum

In preparation for the Constantinople / Petersburg upgrade Aleth had a number releases - starting from 1.5.x series and ending with the  [latest 1.6.0](https://github.com/ethereum/aleth/releases/tag/v1.6.0). Aleth received a lot of p2p network level improvements including a standalone devp2p discovery client called aleth-bootnode, and is compatible with the latest Ethereum revision.

Changelog: [https://github.com/ethereum/aleth/blob/master/CHANGELOG.md](https://github.com/ethereum/aleth/blob/master/CHANGELOG.md)

The EVMC project continues to advance by receiving new language bindings and other improvements while remaining ABI compatible with version 6.

Changelog: [https://github.com/ethereum/evmc/blob/master/CHANGELOG.md](https://github.com/ethereum/evmc/blob/master/CHANGELOG.md)

FInally, the ethash library (<https://github.com/chfast/ethash>) has received ProgPoW support and some integration fixes and improvements.

Changelog: <https://github.com/chfast/ethash/blob/master/CHANGELOG.md>

## DevOps

There are five main areas where devops resources are used: Websites, Bootnodes, Geth, Swarm, and Testing. Devops continues to improve our process for deploying infrastructure, mainly with Ansible and Terraform but also with Kubernetes. We are also doing a better job documenting and tracking how infrastructure is used by all of the development teams.

One mini-project which recently came to completion is collecting together recent code forks of EthStats and creating a release and new repo which we call "EthStats Classic" (<https://github.com/ethereum/eth-netstats>). Alethio EthStats is the 2.0 and will be maintained by the Alethio team (<https://github.com/Alethio/ethstats-network-dashboard>).

From within the Swarm team: We are building tools with Kubernetes that help us quickly provision a variety of Swarm deployments with up to 1000 Swarm nodes, out-of-the-box configured with tracing, monitoring and data visualization tools, which helps our development and testing efforts. These tools allow us to run a number of end-to-end tests as well as mock different network scenarios and simulations. We also run periodic integration tests on larger deployments in addition to our go-test test suite prior to every release, making sure we don't introduce regressions or performance degradation in Swarm.

## DevP2P Design

In early 2019, DevP2P Design completed work on a few key tasks:

- [EIP-778 ](https://eips.ethereum.org/EIPs/eip-778)(Ethereum Node Records) was approved in the allcoredevs call.

- Protocol specifications of eth/63 and les/2 have been moved to the <https://github.com/ethereum/devp2p> repository from GitHub wiki pages.

- Initial drafts of Node Discovery v5 spec have been published.

Meanwhile, we are hard at work on finishing the Node Discovery v5 specification.The "par", "pip" and "shh" protocol specs are being adapted for publishing in the devp2p repo and implementation of EIP-868 and Node Discovery v5 in Geth. We are also working on an EIP for RLPx v6, which will fix some crypto issues and on integrating EIP-778 node records into the handshake.

ENR extensions and supporting development tools have been added to Geth, both as an extension to v4 and as part of v5. Discovery v5 protocol and Geth implementation is nearing a completed state as refinements are made, and external auditors are now being introduced to the protocol for a security audit. Discovery v5 is receiving a lot of interest from the Eth 2.0 implementers community, and Felix has attended meetings with them to help answer questions on where the protocols complement each other.

The devp2p repo continues to gain traction as the home for Ethereum communication protocols, with a number of interesting proposals under review, and documentation such as LES and PIP completed. Much behind-the-scenes work and discussion has been going on regarding LES and its future direction. A number of improvements to LES code structure and robustness are in the Geth pull-request pipeline (big shout out to Gary Rong) addressing immediate concerns, while the long term direction is being researched and established. In the coming weeks the Geth team will meet to consolidate the ideas and make a decision on the direction of LES and its implementation.

## EF Grants

Through the first quarter of 2019, we publicly announced Wave 5 and released more recipients on stage at Ethereal. It has been an extremely exciting time for the program as we have quietly expanded our scope to encompass different project types and explored more ways in which we can support the community.

While we continue to accept new applications, we are also actively sourcing high affinity applications from core devs and researchers. Additionally, we will (in the coming months) publish a Grantee Retrospective highlighting the impact of past grants and the great teams behind the projects.

The goal for the program is to not only provide financial support, but to provide more bespoke support for high potential teams including research support, connections, communications, and more. By increasing the support system and incorporating more voices, we stay true to the mission of decentralizing the decision making process as well as truly doing everything in our power to advance the ecosystem. Stay tuned for more!

## EthereumJS

Our focus within the EthereumJS team is to serve the community with high-quality and robust JavaScript/TypeScript implementations of base-layer Eth 1.0 and increasingly Eth 1.x technologies and protocols. Please have a look at our organizational documentation at [ethereumjs.readthedocs.io](https://ethereumjs.readthedocs.io) for an introduction on what we are doing and some key entry points to our work.

### TypeScript

The center of attention in the early part of the year was the transition of our code base to TypeScript and we are nearing completion on our most used libraries. We have released TypeScript versions of core building blocks like our [blockchain](https://github.com/ethereumjs/ethereumjs-blockchain) and [transaction](https://github.com/ethereumjs/ethereumjs-tx) libraries (the latter also now with full hardfork and EIP-155 replay protection support);here is an open PR with a complete rewrite of the [devp2p](https://github.com/ethereumjs/ethereumjs-devp2p/pull/51) library in TypeScript waiting to be merged (kudos to Dmitriy Ryajov from MetaMask/Mustekala); and - probably the most exciting piece - the transition of our [VM](https://github.com/ethereumjs/ethereumjs-vm) implementation to TypeScript has now been completed and is already merged into the master branch. Watch out for a beta release to follow within the coming days!

### VM

We know the importance of a well-structured, modular and expandable JavaScript VM for the realization of powerful analysis and debugging functionality within development tools like Remix, Truffle and others. To this end, one other current focus of the last months was to make strong moves toward these goals and do significant refactoring work on the VM. Mainly realized by our new employee Sina Mahmoodi, the upcoming v4 VM release will ship with much more explicit and readable memory and stack handling, code modernization with the introduction of ES6 classes and async/await usage as well as a general refactoring and modularization of the EVM specific code structure. The latter -  together with some newly introduced EEI-like environment interface for accessing blockchain data - is in preparation for an already-in-the-works eWASM integration within the VM.

### Other Research

We are also dedicated to continue supporting and maintaining our existing code base to keep our libraries secure, robust and performant. The latest experimentation following these goals touches on using WASM modules for cryptographic primitives, BigInt usage in JavaScript/TypeScript together with fixed-width number handling as well as less error-prone handling and conversion of hexadecimal and buffer values. We'll let you know once we have concrete results to share on these various fronts.

## Ewasm

The past months have been really busy and fruitful. To ensure Ewasm is well-designed as an execution layer, the design process is driven by prototypes, analysis and benchmarking. In Q1 we began extensive benchmarking studies of both WebAssembly engines and EVM engines. In Q2 we released Scout, a prototype execution engine for Ethereum 2.0. Our focus in Q3 will be continued iteration on prototypes, informed by analysis and benchmarking.

### Ewasm during Q1 2019

#### Benchmarking

As benchmarking progressed, new questions arose which required more detailed benchmarks and sometimes new infrastructure. Benchmarking aims to answer open questions, including those related to:

 * different WebAssembly engines (including interpreters and various types of compilers)

 * various implementations of Ethereum precompiles

 * implementation of the same contracts in WebAssembly and EVM

 * different metering strategies

Benchmarked WebAssembly (Wasm) engines include a significant number of standalone engines, as well as the Wasm engines we have integrated into Ewasm clients (via Hera).

For the precompiles comparison, we have used go-ethereum, native-compiled Rust implementations and Wasm-compiled Rust implementations. The Rust implementation of every Constantinople precompile can be found at [ewasm-precompiles](https://github.com/ewasm/ewasm-precompiles). We have also used various manually optimised C versions.

For the EVM comparison we have selected optimised EVM implementations of contracts and implemented counterparts in WebAssembly. The EVM bytecodes were benchmarked using go-ethereum, parity-ethereum and [evmone](https://github.com/chfast/evmone). These comparisons have been especially fruitful, revealing new goals and requirements for our design.

The structure of WebAssembly is well-suited for metering optimizations. Several metering prototypes were developed and benchmarked, with attention to common classes of contracts and types of WebAssembly engines.

Results on EVM were published in Q2 (see below). Reporting on further results, including in-depth analysis of WebAssembly engines, is a work-in-progress and will be forthcoming in future publications.

#### Precompiles

Implementation of all the Constantinople as well as four newly proposed precompiles (blake2, ed25519, bls12-381, sha1) were completed and are available in the [ewasm-precompiles](https://github.com/ewasm/ewasm-precompiles) repository.

We were successful at syncing the entire Rinkeby chain using these implementations in go-ethereum.

#### Eth 1.x

Some members of the team participated in the Eth1.x / Istanbul meetups in San Francisco and Berlin. We gave updates on our progress and presented some of the EIP proposals for Istanbul, which have a significance for Ewasm. Three notable areas of change are [better protocol limits](https://eips.ethereum.org/EIPS/eip-1985), separation of init and runtime code and account versioning.

#### Eth 2.0

The Ewasm team has had ongoing discussions with the Eth 2.0 research team about Phase 2. We shared open questions, design considerations, and ideas. The Ewasm team participated -- mostly remotely -- in the Eth 2.0 research meeting prior to EDCON in April. The goal was to discuss execution engine requirements for Eth 2.0.

Our proposals are tracked on our design repository (https://github.com/ewasm/design/issues).

#### Testnet

The testnet is actively maintained at ewasm.ethereum.org. Contracts can be written in Rust using Rust tooling (ewasm-rust-api and chisel), which has been improved. Contracts can also be written in C, which has a less developed toolchain.

### Ewasm during Q2 2019

#### Eth 1.x

We have published a subsection of the benchmarking report: [EVM benchmarks](https://github.com/ewasm/benchmarking#evm-benchmarks-2019-05-23). The results demonstrate potential for significantly reducing the gas cost of computational opcodes in EVM, motivating us to propose [EIP-2045: Particle gas costs for EVM opcodes](https://eips.ethereum.org/EIPS/eip-2045).

The EVM benchmarks highlight new speed records set by the optimized EVM implementation: [evmone](https://github.com/ethereum/evmone). Created in a joint effort of the Ewasm and Aleth/C++ teams, evmone is designed to be imported as an execution module by any Ethereum client. Further analysis of the speedup attained by evmone and potential gas cost reductions in EVM is ongoing.

#### Eth 2.0

We released [Scout](https://github.com/ewasm/scout), a prototyping tool for Wasm execution scripts on Ethereum 2.0. Scout's release was announced on [ethresearch here](https://ethresear.ch/t/phase-2-execution-prototyping-engine-ewasm-scout/5509) and was presented at the Scaling Ethereum conference in Toronto ([video here](https://www.youtube.com/watch?v=RW7K3JQOZOg&t=6284)).

Scout is a prototype of a new approach to execution on Ethereum 2.0 based on "execution environments". Further experimentation and analysis is needed to determine if execution environments can meet the requirements of Ethereum 2.0, and Scout is designed for running such experiments. A number of researchers and developers are excited about this new approach (see the update on Serenity from the Research team, below).

#### Wasm on the Blockchain

The Ewasm team gave four presentations in Berlin at the inaugural [Wasm on the Blockchain](https://avive.github.io/wasm_on_the_blockchain/#/) workshop. Videos from the event have not yet been uploaded, but slides are available:

* [eWASM ewasm Ewasm](https://drive.google.com/open?id=0BxAMY3dJTXy-MmF4UVZ6bzhkTE1BTDFFQXlyWmJrRGRxeXAw) (a historical overview about Wasm on the Blockchain)

* [The Precompile Problem](https://drive.google.com/open?id=0BxAMY3dJTXy-UlNDcjhTVlVhRlE1M2dGTDZnenZhODFxdTNv)

* [Is WebAssembly Suitable for Blockchain?](https://drive.google.com/open?id=0BxAMY3dJTXy-RWwzTldSdElpRnlkSzNiMF9ETE40cU15YnZN)

* [Eth2 Execution](https://drive.google.com/open?id=0BxAMY3dJTXy-WF94VnJ6Qmd3bGp5SGxRTlY1MHVFRzFhMFB3)

#### Wasm Community Group Meeting

Some members of the Ewasm team also participated in the June meeting of WebAssembly's Community Group. This has allowed us to understand and discuss upcoming proposals for WebAssembly, many of which are relevant to Ewasm. Notes from the Community Group meeting are available in the [WebAssembly/meetings repo](https://github.com/WebAssembly/meetings/blob/master/2019/CG-06.md).

## Geth

The Geth team has been working on the next major release, v1.9.0, which has been in the works for the better part of 4 months now. Some teasers that we've dropped along the way revolve around significant [performance improvements](https://twitter.com/peter_szilagyi/status/1110487398926950400) for archive nodes and significant [storage reductions](https://twitter.com/peter_szilagyi/status/1118420506993156096) for full nodes. These seem to be confirmed by [external](https://twitter.com/LedgerQL/status/1114478908219514880) entities [too](https://twitter.com/eduadiez/status/1114167656628871168), so we're really happy about them. We've also been working on a new database format that permits moving a large chunk (estimated at ⅔, per a recent benchmark) of the data of a full node (not archive) onto a HDD, making it easier and more cost efficient to run Ethereum nodes.

Lower level networking: the team's been focusing on the new discovery specs (ENR and company), with the aim to replace both the old v4 (used by full node) as well as the hacky v5 (used by Geth light clients). This will be the first real upgrade of this infrastructure in the last 4 years, so we have a lot of lessons to incorporate. The aim is to build a much more robust system that can support coexistence of higher level protocols (les) and multiple Ethereum networks (mainnet, testnets, etc).

Higher level networking: the light client protocol sports transaction lookups by hash and new RPC APIs to support priority service to paying customers. The team reviewed directions for the LES and PIP protocols, and have agreed to start to establish common directions between Geth and Parity. The first cross-team meeting is coming up in May.

We've also focused on separating account management out of Geth into its own signer application called Clef. The first release will be CLI only, but we've been working on multiple proof of concept UIs to make sure we and others can build on top of the building blocks Clef provides. Clef's goal is to provide a secure way to manage your Ethereum accounts that can handle both keys as well as hardware wallets; but most importantly, one that can be used by all DApps on your system without each having to roll their own accounts handling!

Other smaller features we've been working on include a GraphQL-based API for querying chain data, built in support for smart card wallets, updated Ledger support, Prometheus monitoring support (in addition to InfluxDB which we've already rolled out), BlockScout integration into Puppeth, and more.

As we're drawing eerily close to the 1.9.0 release, we've kept this section shorter and will do an expanded blog post in the following couple of weeks!

## Grid

The Mist Browser was [sunset](https://medium.com/@avsa/sunsetting-mist-da21c8e943d2), but [Ethereum Grid](https://medium.com/ethereum-grid/introducing-ethereum-grid-1e65e7fb771e) was born. [Grid](https://github.com/ethereum/grid) is a desktop application that allows users to securely download, configure and use various clients and tools in the Ethereum ecosystem. Among its potential benefits, Grid can:

1) allow a less technical audience to safely interact with technical tools

2) help projects in the ecosystem reach wider audiences

3) provide a platform to accelerate hacking on Ethereum.

The Grid team would love your feedback on the [alpha software](https://grid.ethereum.org/) and to hear from projects interested in building a plugin on the platform!

## Moon

Our recent accomplishments are available on our roadmap as an early year written post! Check it out on github: <https://github.com/moonad/roadmap>.

## Play

Meet Play: Tooling for decentralized education

Play launched our first tool a month ago - an embeddable Solidity editor. You can paste in solidity code and get a live preview which allows you to publish the contract and interact with it on chain. Play can be embedded in any site or app - to demo your smart contract, teach, you name it! Try it out at <https://play.ethereum.org/editor-solidity/>

We also just launched our second tool, a static workshop/tutorial generator.

Stay tuned and connect with Play team <https://twitter.com/play_ethereum>

## Python Ecosystem [PyEVM/Trinity/Web3.py/Vyper]

### Web3.py

Web3.py has been working toward a v5 beta release. Version 5 includes work to standardize the supported RPC APIs based on EIP-1474, a new API to read from deployed contracts, implementing signing standards like EIP-712 and EIP-191, as well as many bug fixes. [Web3.py](http://web3.py) v5 also includes a new experimental package manager API. See the [release notes](https://web3py.readthedocs.io/en/latest/releases.html) for more info.

### Vyper

Vyper has benefited from a number of internal cleanup tasks. Examples of this included things such as a thorough lint check, the addition of type annotations to certain code modules and automated checking of those annotations, overall improvements to Vyper's test rigging and continuous integration pipeline, and various code refactors. All of this was part of an ongoing, project-wide push for improved maintainability and readability. The long term goal of this push is to make it easier for new contributors to join the project.

Vyper also continues to add planned features. For more information, visit Vyper's github [issues](https://github.com/ethereum/vyper/issues) and [pull requests](https://github.com/ethereum/vyper/pulls) pages.

### Trinity

Trinity has continued to improve with lots of bug fixes, performance improvements and Constantinople support. We've also been working on making the code base more modular to increase maintainability and code reuse between the Ethereum 1.0 and 2.0 parts of the code base. Trinity continues to improve its event-driven architecture and extensibility support.

Additionally, Trinity has received many updates working towards a first Ethereum 2.0 testnet. We continue to collaborate with the research team to integrate Eth2 into Trinity.

In Eth1.0 news: we're currently experimenting with a new feature we call Beam Sync. Imagine starting a Trinity node and having it start processing the latest blocks and serving RPC requests within minutes. Beam Sync will verify blocks as they're mined by prioritizing sync of the state used in the latest blocks.

And finally on the Eth1.x front, we've been working on the [Firehose Sync protocol](https://notes.ethereum.org/M7DM44r1SWqUBs0wrDi8_Q), a new state sync protocol which will radically decrease the amount of time it takes to start a node and download all the data needed to be a first-class node on the Ethereum network. Firehose and Beam Sync enhance each other. Beam Sync is possible with the current network, but will be dramatically improved with the Firehose Sync protocol.

### eWASM

We have also been laying some groundwork for eWASM in the last months. The [py-wasm](https://github.com/ethereum/py-wasm) library now has a fully functional implementation of the WebAssembly interpreter and we've begun preliminary work to integrate this into Py-EVM to implement an EWASM based execution environment for smart contracts.

### EthPM

EthPM is now fully integrated in [Web3.py](http://web3.py) v5. [ERC 1319](https://github.com/ethereum/EIPs/issues/1319) has been updated to include some useful events and functions. A registry [explorer](http://registry.ethpm.com) was built to provide an interface for interacting with various EthPM Registries. A CLI utility is in the works for downloading EthPM packages to disk.

## Remix

We finally released Remix 0.8 which comes with a massive UX redesign and the integration of a more stable version of the remix plugin API. The documentation has been improved, and will continue to be improved with future updates. Check out the 0.8.0 release on [Github](<https://github.com/ethereum/remix-ide/releases/tag/v0.8.0>), updated [app]([https://remix.ethereum.org/#appVersion=0.8.0](https://remix.ethereum.org/#optimize=false&version=soljson-v0.5.1+commit.c8a2cb62.js&appVersion=0.8.0)) and our [Medium post](<https://medium.com/remix-ide/remix-ide-renewal-release-0-8-4a55da933b70>).

We are regularly updating <https://remix-alpha.ethereum.org> with the latest progress!

We've also seen the integration of remix-tests and remix-debug in Embark and EtherAtom, started to migrate the remix [code base](github.com/ethereum/remix) to TypeScript and the long awaited support of the new AST ;) .

From the 0.8 release on, we will be putting in more effort into writing and promoting educational content, first for the Remix stack and then more broadly for the Ethereum ecosystem.

We plan to release patch versions of Remix more regularly now, and to kickstart a Remix website soon.

The desktop version (usable offline) will hopefully land soon (some of the work has already been done with the Grid integration and we need to check how we can merge efforts here).

We will continue to polish the remix-plugin API and improve the dev UX during the next weeks. Now that the base layer is done, we need to leverage decentralized storage for hosting plugins. This will probably take a few months.

Beside that we want to continue writing tutorials, workshops and do the necessary for onboarding beginners.

## Research [CBC]

Goals for 2019:

1\. Produce liveness proofs for a variety of synchrony assumptions, and for probabilistic and deterministic liveness requirements.

2\. Produce a sharding specification by Devcon 5, complete with cross-shard messaging fork choice rule and (possibly) a load balancer.

Work Completed in January-June 2019:

1\. Finality Inspector - Stanford Research Workshop, February 2019

- The Finality Inspector is an algorithm that finds the finality score of a given block (weight of validators that need to equivocate in order for the given block to be reverted) by searching for a finality criterion in the message DAG. The algorithm is quite efficient -- it runs in polynomial time in the number of total validators.

2\. Documentation for Finality Inspector - ETHParis, March 2019

- We made an informational blog post that describes the Finality Inspector and shows an execution of the protocol (for binary consensus) and the level of finality on proposed values.

3\. Liveness Strategies - May 2019

- So far we have come up with 3 liveness strategies for the CBC Casper family of protocols.

4\. LMD GHOST fork choice algorithms - IC3 Boot Camp, June 2019

- We described an efficient algorithm for maintaining the winning LMD GHOST tip of the blockchain. Upon seeing a new block, it updates the LMD GHOST tip in O(V^2) time (without any additional data stored in blocks), or O(V*log(V)) time (if blocks store a skip list of their ancestor blocks). This algorithm can be also used for executing the LMD GHOST section of the Eth 2.0 fork choice.

Community Outreach in January-June 2019:

1\. Vlad @ Stanford Blockchain Conference, February 2019

2\. Vlad, Aditya @ ETHCC, March 2019

3\. Vitalik, Aditya @ EDCON, April 2019

4\. Vlad @ CryptoChicks, June 2019

## Research [Plasma]

Shipping an end to end implementation of Plasma provided perspective which was critical in coming up with generalized plasma. This insight came from the fact that our testnet was far too special purpose & realizing we needed the ability to upgrade. We wouldn't have come to this if we hadn't been faced with the hard truth that taking our current implementation to production would be severely limited. Instead, a relatively small time investment in a redesign aided in our future proof design.

Announcements from the first part of the year include:

- Plasma Group Announcement -- [Website](https://plasma.group/), [Blog](https://medium.com/plasma-group), [Gitcoin](https://gitcoin.co/profile/plasma-group)

- Plasma Testnet Release & Spec -- [Specification](https://medium.com/plasma-group/plasma-spec-9d98d0f2fccf), [Plasma Chain Operator](https://github.com/plasma-group/plasma-chain-operator/)

- Generalized Plasma -- [Announcement](https://medium.com/plasma-group/towards-a-general-purpose-plasma-f1cc4d49c1f4), [Explainer](https://medium.com/plasma-group/plapps-and-predicates-understanding-the-generalized-plasma-architecture-fc171b25741), [Spec](https://pigi.readthedocs.io/en/latest/src/specs/generalized-plasma-state.html), [Sample Implementation](https://github.com/plasma-group/research/tree/master/gen-plasma)

Ongoing work focuses on Production Payments Plasma. In the couple months we will be taking Generalized Plasma into production with a fully audited & scalable payments network. Next steps will be to tackle:

- Plasma Contracts Audit

- Generalized Plasma node rewrite.

Plapp Plapp.

## Research [Serenity]

tldr; it's happening

Phase 0

[The beacon chain](https://github.com/ethereum/eth2.0-specs/blob/dev/specs/core/0_beacon-chain.md) has gone through a number of iterative releases, and is largely stable as client teams implement. Much work is targeted toward testing and auditing as we near the end of June Phase 0 spec freeze. There now exist a broad array of consensus test vectors that are being passed by client teams, and there is a now a fuzzing effort underway, fuzzing the python spec and the go spec with the intention of fuzzing clients soon. Runtime Verification has also begun formally specifying the beacon chain in K and formally verifying the Vyper deposit contract.

There are a few single-client testnets with public participation. Over the coming months, we expect to see short-lived, and then long-lived, multi-client public testnets.

Much of the Phase 0 work is now in the client teams' hands to bring the beacon chain to production. The client to do list includes: consensus tests, optimizations, efficient aggregation, stable p2p networks, state sync, validator UI/UX, security reviews, visualizations tools... Thank (or fund!) your local client team next time you get a chance. They are the unsung heroes in this process and deserve much more support and praise.

Phase 1

The Phase 1 spec on [shard data chains](https://github.com/ethereum/eth2.0-specs/blob/dev/specs/core/1_shard-data-chains.md) is primarily in place and has gone through many simplifications the past few months. Most of these simplifications manifest in the "[custody game](https://github.com/ethereum/eth2.0-specs/blob/dev/specs/core/1_custody-game.md)", reducing the complexity of computation and the complexity of the challenge games when unavailable data. The custody game is now more multi-party computation friendly to help encourage decentralized staking pools.

Recently Phase 1 was made executable and has been integrated into the spec test suite. This specification is to be iterated upon, simplified, and prototyped in the coming months.

Phase 2

Phase 2 has some exciting research underway. There has been much effort in simplifying and abstracting the execution consensus layer through a new path called "[execution environments](https://ethresear.ch/t/proposed-further-simplifications-abstraction-for-phase-2/5445)". Execution environments open up a world of possibility in how the highly scalable data layer of Eth 2.0 might be used. In particular, it might allow us to slot Eth1 into Eth2 to enable a more seamless transition for the community. Much of the research and debate on Phase 2 is happening on http://ethresear.ch/ so check it out!

Light clients

A [light client specification](https://github.com/ethereum/eth2.0-specs/blob/dev/specs/light_client/sync_protocol.md) relying upon Phase 1 exists and will be iteratively worked on in the coming quarter. We hope to see a couple of light client implementations being worked on in Q3 or Q4 after the core beacon chain implementations stabilize.

## Security [Security / Consensus Tests]

We have performed quite extensive metering of the execution of opcodes within Geth, to provide background and analysis for <https://eips.ethereum.org/EIPS/eip-1884>, which aims to rebalance opcodes that will otherwise lead to severe degradation of the Ethereum network.

The hive framework has been updated, and is again running on <https://hivetests.ethstats.net>/, and we have also re-launched a libfuzzer-based geth/parity differential fuzzer in a production environment. We plan to publicly release this fuzzer in the mid-term future.

Retesteth stability has been restored. We've also fixed bugs when running StateTests and BlockchainTests via test_ RPC interface on aleth. Retesteth could now be built on docker and run against any client that support test_ RPC interface. Geth + retesteth support has now been enabled.

## Solidity

The current focus points for Solidity development are as follows:

-   polishing the ABIEncoderV2 for a non-experimental release,

-   increasing the coverage of the SMT checker module,

-   work on the Yul Optimizer and code generator from Solidity to Yul (simple contracts like an ERC20 token already work)

-   code generation from Yul to eWASM assuming all types or 64 bits wide

-   syntactic and semantic fuzz testing of various components

-   planning changes for the upcoming breaking release 0.6.0

During the previous weeks, we enabled access to the Yul compiler and optimizer via standard-json. It is also possible to access the Solidity to Yul code generator using the ``--ir`` switch. For security reasons, you have to compile this yul code again to bytecode manually. Please try this out but keep in mind that both parts are still experimental! Furthermore, we are adding more checks to the decoder part of the ABIEncoderV2 component that prevents invalid data from getting through the decoding process.

## Swarm

Swarm saw significant stability improvements, notably hardening network connectivity code as well as the integration of the new LocalStore with reliable garbage collection. Our devops infrastructure and cluster provisioning graduated and allows testing complex network behaviour in a cluster of up to 1000 nodes.

Improvements in debugging, tracing, metrics and network simulation testing are now complemented by end-to-end smoke tests. Such tests are run regularly on deployments and allow continuous monitoring of performance. Since they closely match actual use, they model user perception and will be used to justify implementation changes or catch regressions.

Release v0.4 recently brought a much more reliable testnet. The 0.4 release now officially introduces ACT (Swarm's access control module) that enables users to control access to shared content or developers to offer authenticated protected areas/functions of dapps. It will also include improved Swarm Feeds.

We continue restructuring working methods, more principled engineering work, clean code principles.

Swarm now launched a product subteam and consolidated our roadmap adopting a more user-centered MVP approach to milestones.

Other recent updates are captured here: https://github.com/ethersphere/swarm/blob/master/CHANGELOG.md\
Finally, we  [had the Swarm Orange Summit in Madrid](http://summit.ethswarm.org/) and that was very successful and well received!

## Web3.JS

The newly introduced architecture (1.0.0-beta.38) of Web3.js is under active development and is improving day by day. We have implemented the possibility to pass a custom transaction signer, to configure the transaction confirmation workflow, new modules (Admin, TxPool, Miner, Debug), and some new methods for the Eth module (getChainId, requestAccounts, getPendingTransactions). To allow the new architecture to succeed, we're allowing breaking changes by moving this work to a 2.0-alpha version of Web3.js. Because version 1.0.0-beta.37 is widely used and to allow bug fixes for them, we've decided to define beta.37 as 1.0 stable. This gives us the possibility to enable bug fixes for the old architecture of Web3.js while coexisting peacefully with the new and exciting 2.0-alpha architecture.

In Numbers:

📝 1486 commits

🔧 244 fixed issues

🏆 ~135000 lines of code added and ~165000 removed

🏆 [17 Releases](https://github.com/ethereum/web3.js/releases)

The next steps of the Web3.js project will be explained in the coming announcement.

## Whisper

We are improving compatibility with Parity's version of Whisper. Our goal is to have a fully compatible WASM version of this, to simplify the usage of Whisper in Dapps.

Some discrepancy between the implementation and the spec has been found by status and has been fixed.

The documentation effort is well underway, and is available in the geth pages repository. The protocol details are to be moved to the Devp2p spec repo.

## ZKSnarks Research

We are moving forward with zkrollup implementations, and we're becoming more confident in using zkp (zero knowledge proofs) to scale arbitrary dapps to millions of users. Our goal is to provide zkrollup as a proof of concept and then start to explore scaling other kinds of dapps in this way.

High level languages are starting to mature which allow developers to build scalable dapps using snarks with less ramp up time. We are excited by these developments and excited to see developer adoption.

On the privacy front, we have built a [generic zcash style circuit](https://github.com/kobigurk/semaphore) which can be used for coin mixing, voting, anonymous DAOs, anonymous social media, and in other cool privacy focused areas. Our next deliverable in this direction will be a mixer for erc20 tokens. From there comes the potential for more cool privacy projects like, anonymous journalism and voting.

## ZoKrates

Over the last months, we've worked to achieve three main goals: to make ZoKrates more efficient, more powerful, and even easier to use.

We introduced support for Elliptic Curve Cryptography in ZoKrates programs and provided primitives leveraging EC cryptography, for example EdDSA verification, to developers. Together with other useful building blocks, these are now available as part of the ZoKrates standard library. This is accompanied by a new Python library called pycrypto which contains application code that makes it easy to interact with provable ZoKrates programs.

To make the most of these building blocks, we've improved optimizations for the ZoKrates compiler and started refactoring the module system to better support composition.

We have also specified the ZoKrates DSL grammar formally and are currently working on a more efficient parser implementation based on that specification.

As a step towards our goal of a Rust-only codebase, we added support for Bellman which enabled Mac OS X native builds as well as efficiency gains.

Finally, we extended our documentation (e.g., tutorial on how to proof pre-images of hashes) and released a blogpost which discusses the challenges of using zkSNARKs in dApps. For developer education, we conducted a workshop at the 2nd ZKProof Workshop, Berkeley. Further workshops are taking place in London and Split in June.
