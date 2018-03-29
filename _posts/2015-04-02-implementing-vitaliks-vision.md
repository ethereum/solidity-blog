---
id: 1543
title: Implementing Vitalik’s vision
date: 2015-04-02T13:54:15+00:00
author: Taylor Gerring
layout: post
guid: https://blog.ethereum.org/?p=1543
permalink: /2015/04/02/implementing-vitaliks-vision/
dsq_thread_id:
  - "3648679374"
---
Although the core concept was proven last summer when lead developers Vitalik Buterin, Gavin Wood, and Jeffrey Wilcke initiated the PoC (proof-of-concept) series in Python, C++, and Go, a lot of engineering effort has gone to solidifying these implementations. We're all anxious to release a stable command line client and issues flagged in the external audit are being resolved when uncovered. As soon as this process is complete, a general release should be around the corner. In the meantime, the rest of the ecosystem is continuing to evolve to more refined versions.

<!--more-->

One of the challenges with a project of this scope and complexity is simply agreeing on any particular thing. Translating human abstractions into code is a tough task, as <a href="http://queue.acm.org/detail.cfm?id=1999945" target="_blank">even the words we choose</a> influences different ways of expressing ideas with code. As we build more around these core ideas and truly experience how reality meets theory, we’re able to refine the underlying specifications to remove ambiguity. This all pays off as the team shares a clean and well-understood vision that should translate effectively to other developers of varying disciplines.

<a href="https://blog.ethereum.org/wp-content/uploads/2015/03/Screen-Shot-2015-03-28-at-15.39.17-1.png"><img src="https://blog.ethereum.org/wp-content/uploads/2015/03/Screen-Shot-2015-03-28-at-15.39.17-1.png" alt="Number of git commits over time" /></a>
Number of git commits over time

Complementing this, a variety of test suites add an important level of confidence that the various implementations adhere to a specification. On the consensus front, we have used a series of VM and State tests to simulate known results and have added randomization and fuzzing to increase this testing space. A series of system and networking tests are also being run, ensuring clients communicate in an expected manner or respond to certain networking scenarios predictably.

Not only are tests designed for success, but all sorts of error conditions are fed into the test suites to ensure that known errors (such as running out of gas or being abruptly disconnected from a peer) are reported correctly. What this means is that when changes are made to the behaviour of the clients, the decision is driven by data and metrics, rather than theory and gut feelings.

<a href="https://blog.ethereum.org/wp-content/uploads/2015/03/peerconnections.jpg"><img class="size-full wp-image-1550" src="https://blog.ethereum.org/wp-content/uploads/2015/03/peerconnections.jpg" alt="Graphing node connectivity allows us to visually inspect the health of network" width="711" height="658" /></a> Graphing node connectivity allows us to visually inspect the health of network

Beyond testing of the core components, we have begun implementing tests on the exterior layers to ensure that the clients respond to a multitude of calls to the underlying system in the same way with easily-consumable JSON. These specifications are linked <a href="https://github.com/ethereum/wiki/wiki" target="_blank">in the wiki</a> and drive the implementation and communication across diverse teams. This helps to ensure that what is in our heads is reliably translated into code, and that all of this is tested and audited.

All of these improvements are important to the health of the overlaying ecosystem and help shine a spotlight on otherwise very nuanced problems. In addition to refining the core virtual machine and serialization layers, security audits were initiated with a third-party firm experienced in decentralization and cryptographic technologies.

[caption id="attachment_1551" align="alignnone" width="1279"]<a href="https://blog.ethereum.org/wp-content/uploads/2015/03/Ethereum-Infrastructure.png"><img class="size-full wp-image-1551" src="https://blog.ethereum.org/wp-content/uploads/2015/03/Ethereum-Infrastructure.png" alt="A rough snapshot of development efforts" width="1279" height="898" /></a> A rough snapshot of development efforts[/caption]

So how does all of this underlying technobabble expose itself to the general user? For the Frontier rollout, we are aiming to release stable binaries on major operating systems (Mac, Windows, Linux). The main interfaces will consist primarily of the command-line client, interactive JavaScript console, and JSON-RPC. In go-ethereum, we’ve recently renamed this client to “Geth”, a nod to both <a href="http://masseffect.wikia.com/wiki/Geth" target="_blank">Mass Effect</a> and <a href="http://www.thefreedictionary.com/Geth" target="_blank">Old English</a>.

Using the command-line client will be a familiar process to many. For example, you can create a new account with `geth account new`, at which point the program will prompt you for a password to secure the account. Starting geth as a miner can be done by calling it with a parameter, like so: `geth -mine`. This is a great way to start interacting with the system, but has its own set of limitations. For example, once you’ve started mining, how does one pause it?

<a href="https://blog.ethereum.org/wp-content/uploads/2015/03/geth9.gif"><img src="https://blog.ethereum.org/wp-content/uploads/2015/03/geth9.gif" alt="Using Geth’s interactive JavaScript console" /></a>
Using Geth’s interactive JavaScript console

For a more useful way to control the program, Geth has a JavaScript console backed by a <a href="https://github.com/ethereum/wiki/wiki/JavaScript-API" target="_blank">JavaScript API</a>. Start it with a simple command: `geth console`. With Geth, this starts the <a href="https://github.com/ethereum/go-ethereum/wiki/JavaScript-Console" target="_blank">interactive console</a>, giving users the familiarity of JavaScript scripting to control the system from the terminal. For example, to find out a bit more about the local node, run `admin.nodeInfo()`.

This control is partly powered by <a href="https://github.com/ethereum/ethereum.js" target="_blank">ethereum.js</a>, a JavaScript library that helps provide a friendly interface to not only the console, but also DApps. It relies on a well-documented <a href="https://github.com/ethereum/wiki/wiki/JSON-RPC" target="_blank">JSON-RPC</a> interface for automated processing or integration into existing infrastructure. Work on many of these components is ongoing as rigorous effort is being put in to ensuring that the implementations speak the same language and are as generally compatible as possible.

<a href="https://blog.ethereum.org/wp-content/uploads/2015/03/Screen-Shot-2015-03-28-at-22.06.02.png"><img class="size-full wp-image-1548" src="https://blog.ethereum.org/wp-content/uploads/2015/03/Screen-Shot-2015-03-28-at-22.06.02.png" alt="A quick view of build status" /></a>
A quick view of build status

Of course, all of this is built publicly on GitHub and includes various continuous integration aspects such as automated builds and reporting on major platforms. This is not a program defining a spec—this is a spec implemented and cross tested between machines and humans spanning the globe.

Concurrently, work is progressing on several other components to the ecosystem, including GUI interfaces for Mist (the DApp browser) and Mix (the IDE). Many of the tools we use and build upon are new and sometimes require patching upstream to ensure everything works as expected. This is a great benefit for all open source development, and one of the many positive byproducts of all the hours spent actually engineering software. Although the wait may seem long, progress and ongoing confidence in the whole system is increasing steadily.

Beyond the technical ins and outs of development, our communications team has continued interacting with the community to produce public education material like tutorials and videos. Furthermore, our admin staff helps process payments and comply with various local laws including tedious things like employment taxes.

<a href="https://blog.ethereum.org/wp-content/uploads/2015/03/Screen-Shot-2015-03-28-at-15.31.34.png"><img src="https://blog.ethereum.org/wp-content/uploads/2015/03/Screen-Shot-2015-03-28-at-15.31.34.png" alt="Who needs sleep?" /></a> Who needs sleep?

Being part of the Ethereum project has been an exceptional experience filled with excitement. What started as only <a href="http://vitalik.ca/ethereum.html" target="_blank">an idea</a> little more than a year ago has spawned an exceptional engineering effort led by professionals implementing all the necessary infrastructure required to build a revolutionary decentralized application platform.

When Netflix introduced <a href="http://techblog.netflix.com/2012/07/chaos-monkey-released-into-wild.html" target="_blank">Chaos Monkey</a>, they asserted, <em>“We have found that the best defense against major unexpected failures is to fail often. By frequently causing failures, we force our services to be built in a way that is more resilient”</em>. In the process of developing Ethereum, we’ve experienced first-hand the benefits that come from <a href="http://en.wikipedia.org/wiki/Wolff%27s_law" target="_blank">continually straining a system</a>.

And while being tasked to craft the very platform that you dream about is a huge honour, we build it because we want it to exist. Thanks for your patience as we think, sleep, eat, and <a href="http://en.wikipedia.org/wiki/Eating_your_own_dog_food">dogfood</a> the platform. We're as anxious as anyone to see what kind of DAapps will be launching after mainnet goes live!