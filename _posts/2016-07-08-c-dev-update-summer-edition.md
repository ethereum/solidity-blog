---
id: 2871
title: 'C++ DEV Update &#8211; July edition'
date: 2016-07-08T17:57:33+00:00
author: Christian Reitwiessner
layout: post
guid: https://blog.ethereum.org/?p=2871
permalink: /2016/07/08/c-dev-update-summer-edition/
dsq_thread_id:
  - "4970430129"
---
Since the last C++ DEV Update, a lot of things happened in the engine room which were not really visible to the outside. This post wants to give an overview about what we are currently working on.

Apart from the features side, Bob has been working on a proposed process for re-licensing of the C++ runtime client code to Apache 2.0, as has been mentioned a few times in the past month or two. Expect more news on that very soon.
<h2>Eth Unit-Test Mode</h2>
Not only because it is essential for being able to perform our Solidity end-to-end tests via IPC, Dimitry Khoklov and others added some new RPC endpoints to the eth client which allow much more flexibility for testing smart contracts. If you use eth --test -d /tmp/test and connect to the ipc port at /tmp/test/geth.ipc (we recommend using <a href="https://github.com/ethereum/ethereum-console">ethereum-console</a> for that because it already has these features added) you can:
<ul>
 	<li>change the blockchain parameters (e.g. remove proof of work checking and pre-fund certain accounts)</li>
 	<li>mine a certain amount of blocks (at around 30 blocks per second)</li>
 	<li>modify the timestamp of the current block (to e.g. test timeouts in your contracts)</li>
 	<li>revert the blockchain to a given block number</li>
</ul>
This allows us to run our currently 305 Solidity end-to-end tests in around 46 seconds on a moderate computer. Each of these tests include at least two (often more) transactions and the same amount of mined blocks.

More information about these features can be found at <a href="https://github.com/ethereum/ethereum-console">https://github.com/ethereum/ethereum-console</a>.

Please note that this is currently only available for the binary that is provided via the <a href="https://launchpad.net/~ethereum/+archive/ubuntu/ethereum-dev">ubuntu dev ppa</a>.
<h2>Virtual Machine Speedup</h2>
Greg Colvin spent the last months speeding up the C++ implementation of the EVM interpreter. He harvested what he calls the low-hanging fruits (he worked for Oracle on the Java interpreter before…).   The most important improvements so far have been replacing 256-bit calculations with 64-bit calculations for gas metering, and making sure that no more metering calculations are done for each VM operation than necessary.  These and other changes resulted in the following results for Paweł Bylica’s nascent <a href="https://github.com/ethereum/test-tools">benchmark suite</a>. The following chart shows the speedup relative to the old cpp ethereum interpreter (cpp int (old)).

<a href="https://blog.ethereum.org/wp-content/uploads/2016/07/relative_speedup.png"><img class="alignnone size-full wp-image-2872" src="https://blog.ethereum.org/wp-content/uploads/2016/07/relative_speedup.png" alt="relative_speedup" width="600" height="371" /></a>

To be fair, we have to tell what these benchmarks measure. The first benchmark (where the evmjit goes off the scale with a speedup of 472x) does a million empty loops, and shows how slow the EVM’s computed goto is compared to the direct jump of a JIT - fixing that is next on the stack.  The second benchmark is a bad random number generator that does a million loops with four multiplications and four additions per loop.  It is dominated by 256-bit calculations, so a JIT makes less difference.  (Note that the Go JIT does not compile to native code, but to a faster interpreted representation.)

<em>In practice</em>, these speedups will <b>only</b> be relevant to “number-crunching” contracts because the computation time is otherwise largely dominated by storage access. On the other hand, the “rng” benchmark is quite similar to cryptographic operations which pulls such things further into the realm of actual on-chain implementations.

Paweł Bylica is working on a <a href="http://ethereum.github.io/evmjit/docs/group__EVMC.html">C-language interface</a> between the virtual machine implementation and the client that hosts it, with the goal of being able to plug different VMs into an Ethereum client. This way, geth can also potentially benefit from our changes to the C++ virtual machine and especially from the LLVM just-in-time compiler.

Note that these changes are not yet released, but they are part of the <a href="https://launchpad.net/~ethereum/+archive/ubuntu/ethereum-dev">ubuntu dev ppa</a>.
<h2>Remix</h2>
Yann Levreau and Liana Husikyan are working on our new EVM debugger remix. We released the alpha version some days ago:

<a href="http://ethereum.github.io/remix/">Application</a> - <a href="https://github.com/ethereum/remix">Instructions</a>

For now, you can “only” use it to inspect every single step in the execution of any transaction in the blockchain, look at the current stack, memory and storage contents and see the sequence of instructions. The next step will be to also allow source-level debugging where you can see the current position in the source code, step on line or instruction level and see the decoded values of the variables (instead of only the raw hex values).

The debugger is for you, the community, and we were delighted to hear that etherscan has already integrated Remix into their <a href="http://etherscan.io/remix?txhash=0x7a45da60e04d397897f43b254a25bb96ee694ab07f3dd73d7d65e66f524a5a94">blockchain explorer</a>.
<h2>Repository Reorganisation</h2>
Bob Summerwill is dedicated to bringing back C++-Ethereum to its former home, https://github.com/ethereum/cpp-ethereum and thus remove the unnecessary and confusing split into multiple sub-repositories. We are making great progress there, one of the first really visible steps was to decouple the testing infrastructure of Solidity from the virtual machine implementation. The Solidity tests can now be compiled without the virtual machine and they are run by communicating with a specially configured eth process (the one mentioned above) over the regular IPC interface.

The next steps here are to disentangle the rest of the code, modify the test automation and continuous integration accordingly and perform the actual move.

Together with this step, we unfortunately have to <b>say goodbye to Mix and AlethZero</b> (the spirit of mix will live on in the new remix project). The burden they drag along would be too big, because it includes Qt and a tight coupling with Solidity. As already explained in earlier posts, a loose IPC-based coupling of these tools to a small client implementation makes us much more flexible and the community support that comes with a change to JavaScript and Web-based tools like remix and browser-solidity is just overwhelming in comparison.
<h2>Formal Verification</h2>
We are extending the existing formal verification tools integrated with Solidity to cross-contract calls. This would enable automated proofs that e.g. a recursive call attack is not possible against a certain contract. Also, as why3 (the tool we use to do the heavy lifting) was recently ported to <a href="http://lists.gforge.inria.fr/pipermail/why3-club/2016-June/001350.html">browsers</a>, we can probably expect it to be available right inside browser-solidity and other tools like blockchain explorers!

There is a first <a href="https://gist.github.com/chriseth/c4a53f201cd17fc3dd5f8ddea2aa3ff9">proof of concept</a> including explanations that shows how automated verification can be used to show that it is impossible to steal money from a solidity contract, even if recursive calls are allowed.

This proof of concept will hopefully evolve into a usable tool in the next weeks.
<h2>Solidity Tools</h2>
Several people from the community and from inside the Foundation are currently working on tools for Solidity or the EVM in general. These include:
<ol>
 	<li>Solidity AST analysis for warnings by Dave Hoover (@redsquirrel)</li>
 	<li>A Read-Eval-Print version of Solidity by raineorshine: <a href="https://github.com/raineorshine/solidity-repl">Solidity-repl</a></li>
 	<li><a href="https://github.com/raineorshine/solgraph">Control-flow analysis graph</a> also by raineorshine</li>
 	<li><a href="https://github.com/arachnid/evmdis">EVM disassembler</a> by Nick Johnson</li>
</ol>
&nbsp;