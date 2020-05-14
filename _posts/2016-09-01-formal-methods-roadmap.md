---
layout: post
published: true
title: 'Dev Update: Formal Methods'
date: '2016-09-01'
author: Christian Reitwiessner
category: Announcements
---
_This post was originally published on the [Ethereum blog](https://blog.ethereum.org/2016/09/01/formal-methods-roadmap/)._

Today, I am delighted to announce that Yoichi Hirai (<a href="https://github.com/pirapira">pirapira</a> on github) is joining the Ethereum project as a formal verification engineer. He holds a PhD from the University of Tokyo on the topic of formalizing communicating parallel processes and created formal verification tools for Ethereum in his spare time.

In his own words:

<section class=" section--body section--first section--last">
<div class="section-content">
<blockquote>
<p id="fe7f" class="graf--p graf-after--h3" style="text-align: left;">I’m joining Ethereum as a formal verification engineer. My reasoning: formal verification makes sense as a profession only in a rare situation where</p>

<ul class="postList" style="text-align: left;">
 	<li id="d4bb" class="graf--li graf-after--p">the verification target follows short, simple rules (EVM);</li>
 	<li id="ec15" class="graf--li graf-after--li">the target carries lots of value (Eth and other tokens);</li>
 	<li id="6453" class="graf--li graf-after--li">the target is tricky enough to get right (any nontrivial program);</li>
 	<li id="70dc" class="graf--li graf-after--li">and the community is aware that it’s important to get it right (maybe).</li>
</ul>
<p id="cecf" class="graf--p graf-after--li" style="text-align: left;">My last job as a formal verification engineer prepared me for this challenge. Besides, around Ethereum, I’ve been playing with two projects: an online service called <a class="markup--anchor markup--p-anchor" href="http://dr-y.no-ip.net/" target="_blank" rel="nofollow" data-href="/r/?url=http%3A%2F%2Fdr-y.no-ip.net%2F">Dr. Y’s Ethereum Contract Analyzer</a> and <a class="markup--anchor markup--p-anchor" href="https://github.com/pirapira/evmverif/" target="_blank" rel="nofollow" data-href="/r/?url=https%3A%2F%2Fgithub.com%2Fpirapira%2Fevmverif">a github repository</a> containing Coq proofs. These projects are at the opposite extremes of a spectrum between an automatic analyzer and a manual proof development.</p>
<p id="3333" class="graf--p graf-after--p" style="text-align: left;">Considering the collective impact to the whole ecosystem, I’m attracted to an automatic analyzer integrated in a compiler. Many people would run it and some would notice its warnings. On the other hand, since any surprising behavior can be considered a bug, any surprise should be removed, but computers cannot sense the human expectations. For telling human expectations to the machines, some manual efforts are necessary. The contract developers need to specify the contract in a machine-readable language and give hints to the machines why the implementation matches the specification (in most cases the machine wants more and more hints until the human realizes a bug, frequently in the specification). This is labor intensive, but such manual efforts are justifiable when a contract is designed to carry multi-million dollars.</p>
</blockquote>
<p class="graf--p graf-after--p">Having a person dedicated to formal methods not only gives us the ability to move faster in this important but also fruitful area, it hopefully also allows us to communicate better with academia in order to connect the various singular projects that have appeared in the past weeks.</p>
<p class="graf--p graf-after--p">Here are some projects we would like to tackle in the future, most of them will probably be done in cooperation with other teams.</p>
Solidity:
<ul class="postList">
 	<li class="graf--li graf-after--p">extending the Solidity to Why3 translation to the full Solidity language (maybe switch to F*)</li>
 	<li class="graf--li graf-after--p">formal specification of Solidity</li>
 	<li class="graf--li graf-after--p">syntax and semantics of modal logics for reasoning about multiple parties</li>
</ul>
Community:
<ul class="postList">
 	<li id="9996" class="graf--li graf-after--p">creating a map of formal verification projects on Ethereum</li>
 	<li id="8be5" class="graf--li graf-after--li">collecting buggy Solidity codes, for benchmarking automatic analyzers</li>
 	<li class="graf--li graf-after--li">analyzing deployed contracts on the blockchain for vulnerabilities (related: <a class="markup--anchor markup--li-anchor" href="https://eprint.iacr.org/2016/633" target="_blank" rel="nofollow" data-href="/r/?url=https%3A%2F%2Feprint.iacr.org%2F2016%2F633.pdf">OYENTE tool</a>)</li>
</ul>
<p id="b476" class="graf--p graf-after--li">Tools:</p>

<ul class="postList">
 	<li id="88ee" class="graf--li graf-after--p">provide a human- and machine-readable formalization of the EVM, which can also be executed</li>
 	<li id="1885" class="graf--li graf-after--li">developing formally verified libraries in EVM bytecode or Solidity</li>
 	<li id="47d0" class="graf--li graf-after--li">developing a formally verified compiler for a tiny language</li>
 	<li class="graf--li graf-after--li">explore the potential for interaction-oriented languages ("if X happens then do Y; you can only do Z if you did A")</li>
</ul>
</div>
</section>

<div id="disqus_link_container"></div>
<script>jQuery(document).ready(function() { EthBlogUtils.display_disqus_link();});</script>