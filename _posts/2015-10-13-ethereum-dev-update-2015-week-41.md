---
id: 2200
title: Ethereum Dev Update 2015 / Week 41
date: 2015-10-13T00:43:48+00:00
author: Taylor Gerring
layout: post
guid: https://blog.ethereum.org/?p=2200
permalink: /2015/10/13/ethereum-dev-update-2015-week-41/
dsq_thread_id:
  - "4219299315"
---
In an effort to bring the community more information about the going-ons at Ethereum, we're planning to release semi-weekly updates of project progress. I hope this provides some high-level information without having to live on GitHub ;)

The Go and C++ teams have daily standups and are making regular progress along with several other improvements around the ecosystem. Concurrent with this, the teams are gearing up for DEVCON1, preparing talks and presentations along with finalizing more details in the coming weeks. If you haven't already registered, visit <a href="https://devcon.ethereum.org/">https://devcon.ethereum.org/</a> to join the fun next month in London. Now on to the updates...

<!--more-->
<h3>Mist</h3>
Hoping to have GUI wallet next week with chocie of Geth or Eth client. The team is spending time polishing remaining bugs and bundling binaries for the various platforms.
<h3>Light Client</h3>
The LES subprotocol being developed and prototyped into the clients. First release is only a start and work will continue on light-client features into the future, more robustly supporting mobile and embedded scenarios. Separately, work on fast-syncing will help aid in this transition.
<h3>Solidity/Mix</h3>
Improvements to passing certain types in Solidity has been added along with improving exception handling throughout. Multiple improvements have been pushed to Christian's <a href="https://chriseth.github.io/browser-solidity/">browser-solidity</a> project. Continued refinement is also being put into the Mix UI.
<h3>cpp-ethereum</h3>
The team is currently working towards 1.0 release and hoping to have new RC available soon. Refactoring of the error reporting mechanism ("Exceptions") and the ability for libraries to access storage of the calling contract by passing storage reference types is now possible. Additionally, the build system being overhauled with a switch to Jenkins.
<h3>go-etheruem</h3>
Continuing working towards Geth 1.3 release, which will consist primarily of internal API improvements and speedups in preparation for future features. More work being put into Windows builds, hoping to have source-built scripts available soon. Fast-sync code being added to client, allowing full-nodes to syncronize to the network much quicker.

&nbsp;