---
layout: post
published: true
title: 'Solidity Optimizer Bug'
date: '2017-05-03'
author: Martin Swende
category: Security Alerts
---
_This post was originally published on the [Ethereum blog](https://blog.ethereum.org/2017/05/03/solidity-optimizer-bug/)._

A bug in the Solidity optimizer was reported through the <a href="https://bounty.ethereum.org/" target="_blank">Ethereum Foundation Bounty program</a>, by Christoph Jentzsch. This bug is patched as of 2017-05-03, with the release of Solidity 0.4.11.
<h2 id="background" class="part" data-startline="5" data-endline="5">Background</h2>
<p class="part" data-startline="7" data-endline="7">The bug in question concerned how the optimizer optimizes on constants in the byte code. By "byte code constants", we mean anything which is <code>PUSH</code>ed on the stack (not to be confused with Solidity constants). For example, if the value <code>0xfffffffffffffffffffffffffffffffffffffffffffffffe</code> is <code>PUSH</code>ed, then the optimizer can either do <code>PUSH32 0xfffffffffffffffffffffffffffffffffffffffffffffffe</code>, or choose to encode this as <code>PUSH1 1; NOT;</code>.</p>
<p class="part" data-startline="9" data-endline="9">An error in the optimizer made optimizations of byte code constants fail for certain cases by producing a routine that did not properly recreate the original constant.</p>
<p class="part" data-startline="11" data-endline="11">The behavior described in the reported bug was found in a contract in which one method ceased functioning when another - totally unrelated - method was added to the contract. After analysis, it was determined that a number of conditions must exist at once for the bug to trigger. Any combination of conditions that would trigger the bug would consistently have the following two conditions:</p>

<ol class="part" data-startline="13" data-endline="15">
 	<li class="" data-startline="13" data-endline="13">The constant needs to start with <code>0xFF...</code> and end with a long series of zeroes (or vice versa).</li>
 	<li class="" data-startline="14" data-endline="15">The same constant needs to be used in multiple locations, for the optimizer to choose to optimize this particular constant. Alternatively, it needs to be used in the constructor, which optimises for size rather than gas.</li>
</ol>
<p class="part" data-startline="16" data-endline="16">In addition to the two conditions above, there are further, more complicated conditions that are required.</p>

<h2 id="analysis" class="part" data-startline="18" data-endline="18">Analysis</h2>
<p class="part" data-startline="20" data-endline="20">This bug is present in all released versions of Solidity from at least as far back as summer 2015 to the present. Although the bug has been present since 2015, it seems very hard to trigger by “random” code:</p>
<p class="part" data-startline="22" data-endline="22">We performed a static analysis of all contract code deployed on the blockchain, and found no occurrence of such an invalidly generated routine. Note, the fact that we have not found a bug in all the contract code does not guarantee the absence of such occurrences.</p>

<h2 id="improvements" class="part" data-startline="24" data-endline="24">Improvements</h2>
<p class="part" data-startline="26" data-endline="26">In order to provide better transparency and increased awareness of bugs in Solidity, we have started exporting information about Solidity-related vulnerabilities as JSON-files in the Solidity code repository(<a href="https://github.com/ethereum/solidity/blob/develop/docs/bugs.json">1</a>,<a href="https://github.com/ethereum/solidity/blob/develop/docs/bugs_by_version.json">2</a>). We hope that block explorers will integrate this information along with other contract-related information.</p>
<p class="part" data-startline="28" data-endline="28">Etherscan has already implemented this, which can be seen <a href="https://etherscan.io/address/0x83b5c924b74e0dc12386fa110c28faa1efedb07b#code" target="_blank">here</a> and <a href="https://etherscan.io/contractsVerified" target="_blank">here</a>.</p>
<p class="part" data-startline="30" data-endline="30">Concerning the bug itself, we added a mini-EVM to the optimizer which verifies the correctness of each generated routine at compile time.</p>
<p class="part" data-startline="32" data-endline="32">Furthermore, work has already started on a fully-specified and more high-level intermediate language. Future optimizer routines on this language will be much easier to understand and audit and it will replace the current optimizer.</p>
