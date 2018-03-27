---
id: 2937
title: 'Hive: How we strived for a clean fork'
date: 2016-07-22T09:36:16+00:00
author: Péter Szilágyi
layout: post
guid: https://blog.ethereum.org/?p=2937
permalink: /2016/07/22/hive-strived-clean-fork/
dsq_thread_id:
  - "5005045179"
---
The DAO soft-fork attempt was difficult. Not only did it turn out that we underestimated the side effects on the consensus protocol (i.e. DoS vulnerability), but we also managed to introduce a data race into the rushed implementation that was a ticking time bomb. It was not ideal, and even though averted at the last instance, the fast approaching hard-fork deadline looked eerily bleak to say the least. We needed a new strategy...

The stepping stone towards this was an idea borrowed from Google (courtesy of Nick Johnson): writing up a detailed <em>postmortem</em> of the event, aiming to assess the root causes of the issue, focusing solely on the technical aspects and appropriate measures to prevent recurrence.
<blockquote><i><span style="font-weight: 400">Technical solutions scale and persist; blaming people does not. ~ Nick</span></i></blockquote>
From the postmortem, one interesting discovery from the perspective of this blog post was made. The soft-fork code inside [go-ethereum](https://github.com/ethereum/go-ethereum) seemed solid from all perspectives: a) it was thoroughly covered by unit tests with a 3:1 test-to-code ratio; b) it was thoroughly reviewed by six foundation developers; and c) it was even manually live tested on a private network... Yet still, a fatal data race remained, which could have potentially caused severe network disruption.

It transpired that the flaw could only ever occur in a network consisting of multiple nodes, multiple miners and multiple blocks being minted simultaneously. Even if all of those scenarios held true, there was only a slight chance for the bug to surface. Unit tests cannot catch it, code reviewers may or may not catch it, and manual testing catching it would be unlikely. Our conclusion was that the development teams needed more tools to perform reproducible tests that would cover the intricate interplay of multiple nodes in a concurrent networked scenario. Without such a tool, manually checking the various edge cases is unwieldy; and without doing these checks continuously as part of the development workflow, rare errors would become impossible to discover in time.

And thus, [hive](https://github.com/karalabe/hive) was born...
<h2>What is hive?</h2>
Ethereum grew large to the point where testing implementations became a huge burden. Unit tests are fine for checking various implementation quirks, but validating that a client conforms to some baseline quality, or validating that clients can play nicely together in a multi client environment, is all but simple.

[Hive](https://github.com/karalabe/hive) is meant to serve as an easily expandable test harness where <strong>anyone</strong> can add tests (be those simple validations or network simulations) in <strong>any</strong> programming language that they are comfortable with, and hive should simultaneously be able to run those tests against <strong>all</strong> potential clients. As such, the harness is meant to do black box testing where no client specific internal details/state can be tested and/or inspected, rather emphasis would be put on adherence to official specs or behaviors under different circumstances.

<em>Most importantly, hive was designed from the ground up to run as part of any clients' CI workflow!</em>
<h2>How does hive work?</h2>
Hive's body and soul is [docker](https://www.docker.com/). Every client implementation is a docker image; every validation suite is a docker image; and every network simulation is a docker image. Hive itself is an all encompassing docker image. This is a very powerful abstraction...

Since [Ethereum clients](https://github.com/karalabe/hive#adding-new-clients) are docker images in hive, developers of the clients can assemble the best possible environment for their clients to run in (dependency, tooling and configuration wise). Hive will spin up as many instances as needed, all of them running in their own Linux systems.

Similarly, as [test suites](https://github.com/karalabe/hive#adding-new-validators) validating Ethereum clients are docker images, the writer of the tests can use any programing environment he is most familiar with. Hive will ensure a client is running when it starts the tester, which can then validate if the particular client conforms to some desired behavior.

Lastly, [network simulations](https://github.com/karalabe/hive#adding-new-simulators) are yet again defined by docker images, but compared to simple tests, simulators not only execute code against a running client, but can actually start and terminate clients at will. These clients run in the same virtual network and can freely (or as dictated by the simulator container) connect to each other, forming an on-demand private Ethereum network.
<h2>How did hive aid the fork?</h2>
Hive is neither a replacement for unit testing nor for thorough reviewing. All current employed practices are essential to get a clean implementation of any feature. Hive can provide validation beyond what's feasible from an average developer's perspective: running extensive tests that can require complex execution environments; and checking networking corner cases that can take hours to set up.

In the case of the DAO hard-fork, beyond all the consensus and unit tests, we needed to ensure most importantly that nodes partition cleanly into two subsets at the networking level: one supporting and one opposing the fork. This was essential since it's impossible to predict what adverse effects running two competing chains in one network might have, especially from the minority's perspective.

As such we've implemented three specific network simulations in hive:

* [The first](https://github.com/karalabe/hive/tree/master/simulators/dao-hard-fork/extradata) to check that miners running the full Ethash DAGs generate correct block extra-data fields for both pro-forkers and no-forkers, even when trying to naively spoof.

* [The second](https://github.com/karalabe/hive/tree/master/simulators/dao-hard-fork/network-split) to verify that a network consisting of mixed pro-fork and no-fork nodes/miners correctly splits into two when the fork block arrives, also maintaining the split afterwards.

* [The third](https://github.com/karalabe/hive/tree/master/simulators/dao-hard-fork/synchronisation) to check that given an already forked network, newly joining nodes can sync, fast sync and light sync to the chain of their choice.

The interesting question though is: did hive actually catch any errors, or did is just act as an extra confirmation that everything's all right? And the answer is, *both*. Hive caught [three fork-unrelated bugs](https://github.com/karalabe/hive#trophies) in Geth, but also heavily aided Geth's hard-fork development by continuously providing feedback on how changes affected network behavior.

There was some criticism of the go-ethereum team for taking their time on the hard-fork implementation. Hopefully people will now see what we were up to, while concurrently implementing the fork itself. All in all, I believe [hive](https://github.com/karalabe/hive) turned out to play quite an important role in the cleanness of this transition.
<h2>What is hive's future?</h2>
The Ethereum GitHub organization features [4 test tools already](https://github.com/ethereum?utf8=%E2%9C%93&amp;query=test), with at least one EVM benchmark tool cooking in some external repository. They are not being utilised to their full extent. They have a ton of dependencies, generate a ton of junk and are very complicated to use.

With hive, we're aiming to aggregate all the various scattered tests under one *universal client validator* that has minimal dependencies, can be extended by anyone, and can run as part of the daily CI workflow of client developers.

We welcome anyone to make contributions to the project, be that adding new clients to validate, validators to test with, or simulators to find interesting networking issues. In the meantime, we'll try to further polish hive itself, adding support for running benchmarks as well as mixed-client simulations.

With a bit or work, maybe we'll even have support for running hive in the cloud, allowing it to run network simulations at a much more interesting scale.

<div id="disqus_link_container"></div>
<script>jQuery(document).ready(function() { EthBlogUtils.display_disqus_link();});</script>