---
id: 2540
title: 'Ethereum DEV Update: C++ Roadmap'
date: 2016-02-12T15:31:56+00:00
author: Christian Reitwiessner
layout: post
guid: https://blog.ethereum.org/?p=2540
permalink: /2016/02/12/ethereum-dev-update-c-roadmap/
dsq_thread_id:
  - "4573347990"
---
Although this is my first post in this blog, many people might already know me as the person behind Solidity. I recently took on the lead of the C++ team and would like to share my vision for the future development.

Ethereum is a free software project that anyone is free to use and improve. From what we have seen at our conferences and meetups, there are so many people working on projects on top of Ethereum, but we only have a small team working on the actual platform. Ethereum should be an open project that is inviting for anyone to propose improvements, actually implement them and get them accepted.

As the new C++ team lead, I will do my best to get the entry barrier as low as possible not only for users (DApp developers) but also for developers (platform developers). This is a necessary step, and I am convinced that this is the way software should be developed, especially software that aims to be beneficial to humanity as a whole.

Of course, the community can only support us, we cannot ask anyone to develop an important feature with a deadline in their free time. This is why the core team will focus on the key software components and many other things will be optional. We will drop any work duplicated between the Go and C++ teams unless it is essential for finding consensus bugs and work hard to remove the invisible “language barrier” and bring the teams closer together to function as one cohesive team.

This means our priorities will be Solidity and Mix (the IDE and smart contract debugger), while the C++ implementation of ethereum/web3 ("eth") will align its interface with the Go implementation Geth, so existing front­ends like Mist, the Ethereum wallet, the Geth console and other tools can attach to both backends without changes.

On the other hand, this means that we cannot support the development of the C++ UI AlethZero into a fully­fle dged DApp browser and would be happy to see its unique features (mainly the blockchain explorer) being integrated as a DApp into Mist.

On a more technical side, our current roadmap is as follows, while most of the items for eth depend on our capacity and perhaps on how much help we get from external contributors.

If you want to help, please read our <a href="https://github.com/ethereum/webthree-umbrella/blob/develop/CONTRIBUTING.md">contributors guide</a> or reach out for us on <a href="https://gitter.im/ethereum/cpp-ethereum">gitter</a>.
<h3>Solidity:</h3>
The general idea is to make Solidity more flexible and allow generic data types to be implemented.

Specific planned features:
<ul>
	<li>anonymous functions to be used in internal callbacks for functions like map, reduce, sort and others, but also for asynchronous cross-contract (or cross-chain) callbacks</li>
	<li>type templates, especially for libraries to allow reusable data structures</li>
	<li>inline assembly</li>
</ul>
Inline assembly and type templates together will hopefully speed up the implementation of other features.
<h3>Mix:</h3>
The unique feature of Mix is its source-level Solidity debugger. We plan to extend it so that you can also debug existing contracts. Furthermore, we want to integrate existing solutions for package management and testing (dapple, truffle, embark, ...).

Specific features:
<ul>
	<li>debug past transactions in the live or a test chain</li>
	<li>run javascript unit tests directly from the IDE</li>
	<li>github integration (load from / save to repository, load libraries directly from github)</li>
	<li>why3 (formal verification) integration</li>
</ul>
<h3>Eth:</h3>
We will make eth more flexible and interoperable with other components of the Ethereum ecosystem. This allows us to remove features that are already implemented elsewhere, so that eth will be easier to build and maintain. Furthermore, we can more easily target embedded devices and different platforms. We also want to make the just-in-time compiler available to other implementations.
<ul>
	<li>reduce external dependencies if possible, improve build process</li>
	<li>replace eth console by nodejs application (will remove v8 as dependency) or "geth attach"</li>
	<li>fast sync</li>
	<li>improve just in time compiler on small numbers</li>
	<li>state trie pruning</li>
	<li>light client</li>
	<li>incorporate private / consortium chain executable flu into normal executable (depending on capacity)</li>
</ul>