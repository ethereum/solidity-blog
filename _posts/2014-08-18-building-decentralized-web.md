---
id: 662
title: building the decentralized web 3.0
date: 2014-08-18T11:18:13+00:00
author: Taylor Gerring
layout: post
guid: https://blog.ethereum.org/?p=662
permalink: /2014/08/18/building-decentralized-web/
dsq_thread_id:
  - "2936900506"
enclosure:
  - |
    https://blog.ethereum.org/wp-content/uploads/2014/08/GLOBE_LARGE.webm
    2038953
    video/webm
    
  - |
    https://blog.ethereum.org/wp-content/uploads/2014/08/GLOBE_LARGE.mp4
    2176679
    video/mp4
    
image: /wp-content/uploads/2014/08/CCTV.jpg
tags:
  - decentralized web
  - web 3.0
---
<h2>how ethereum could shard the web</h2>
Given the state of our 25-year old web and all the problems inherited from legacy 1970's systems design, we should pause and take inventory of those components which are fundamentally broken and would offer a substantial return on development investment. Intersecting this concern with security, privacy, and censorship resistance, it should be painfully obvious that <a href="http://www.wired.co.uk/news/archive/2014-02/06/tim-berners-lee-reclaim-the-web" title="Tim Berners-Lee: we need to re-decentralise the web" target="_blank">an all-out attack on Internet infrastructure is already underway</a>. As netizens, a shared duty falls on us to explore, exploit, and implement new technologies that benefits creators, not oppressors.

And while cryptography first allowed us to secure our messages from prying eyes, it is increasingly being used in more abstract ways like the secure movement of digital value via cryptocurrencies. If PGP was the first major iteration of applied crypto and Bitcoin the second, then I anticipate that the interaction and integration of crypto into the very fabric of a decentralized web will be the refined third implementation, taking root and blossoming in popularity.

<!--more-->
<h3>the explosion of web services</h3>
Taking a look back at the brief history of the web, most would agree that Web 1.0 was epitomized by CGI scripts generating templated content on a server and delivering it to the client in a final form. This was a clear model of monolithic centralization, however, this basic form of interactivity was a huge improvement over the basic post-and-read format that comprised much of internet content at that time. Imagine having to reload the entire front page of Digg every time you wanted to click something:

<img class="size-full wp-image-683" src="https://blog.ethereum.org/wp-content/uploads/2014/08/digg-20061.png" alt="Digg homepage 2006" /> Digg in 2006, a prolific example of “Web 2.0” interactivity not afforded by traditional CGI scripts

As browser technology advanced, experimentation with <a title="AJAX (programming)" href="https://en.wikipedia.org/wiki/Ajax_(programming)">AJAX</a> calls began, allowing us to asynchronously perform actions without having to reload the whole page. Finally, you could upvote without submitting an HTML form and reloading everything. This movement to separate content from presentation—aided by CSS—pushed the web forward.

Today we have technologies like <a href="https://angularjs.org/">AngularJS</a> and <a href="http://emberjs.com/">EmberJS</a> which ask the designer to generate a client template with specific data holes to be filled in by some backend. Although these frameworks facilitate some of the programming glue for seamless and live updates, they also nudge the developer to work in a specific way. But this is only a moderate step towards Web 2.5.
<h3>amuse-bouche</h3>
The real Web 3.0 has yet to begin, but it could obliterate the notion of separating content from presentation by removing the need to have servers at all. Let's take a look at some of the underlying technologies the Ethereum Project aims to deliver:
<ul>
	<li><em>Contracts</em>: decentralized logic</li>
	<li><em>Swarm</em>: decentralized storage</li>
	<li><em>Whisper</em>: decentralized messaging</li>
</ul>
<img class="size-full wp-image-665" src="https://blog.ethereum.org/wp-content/uploads/2014/08/ethereum-protocols.png" alt="Ethereum protocols - decentralized web" /> Interaction including Ethereum contracts, Swarm storage, Whisper comms

Technologies like Swarm could serve as the underlying static hosting infrastructure, removing the need to highly distribute and cache specific content. Because &ldquo;<a href=" https://blog.ethereum.org/2014/08/16/secret-sharing-erasure-coding-guide-aspiring-dropbox-decentralizer/" title="Secret Sharing and Erasure Coding: A Guide for the Aspiring Dropbox Decentralizer" target="_blank">decentralized dropbox</a>&rdquo; has been discussed with such frequency, expect HTTP-like bindings or services to be built atop this type of blob storage, making integration with the decentralized web 3.0 even simpler. This effort will also allow replacement of typical content delivery networks (<a href="https://en.wikipedia.org/wiki/Content_delivery_network">CDN</a>) with a distributed hash table (<a href="https://en.wikipedia.org/wiki/Distributed_hash_table">DHT</a>) pointing to file blobs, much how BitTorrent works. Because of the flexibility offered by ethereum contracts, the model of content access could be creator pays, reader pays, or some hybrid system.

So we've just replaced the need to have caches, reverse proxies, CDNs, load balancers, and the like to serve static content to users. Another way in which Etheruem could impact this traditional infrastructure is by replacing business logic application tiers with on-blockchain contracts. Traditionally developed in a variety of web-friendly languages like Perl, PHP, Python, ASP, C#, and Ruby, ethereum contracts run in a fully-inspectable virtual machine that encourage simplicity and reuse. Business analysts and project managers might find this code transparency refreshing, especially since the same code can be written in <a href="https://github.com/ethereum/wiki/wiki/Serpent">Serpent</a> (a Python-like language), <a href="https://github.com/ethereum/cpp-ethereum/wiki/LLL">LLL</a> (a Lisp-like language), XML (a nightmare), or even in visual block form!

<img class="size-full wp-image-664" src="https://blog.ethereum.org/wp-content/uploads/2014/08/eth-visual-editor.png" alt="Ethereum blocks editor" /> Ethereum contract code visual editor

How could all this be possible? Taking a look at the latest <a href="https://github.com/ethereum/cpp-ethereum/wiki/PoC-6-JS-Bindings">ethereum proof-of-concept 6 JavaScript bindings</a>, we see that a sprinkling of JavaScript is all that’s required to monitor an account balance on the decentralized web:
<pre><code>&lt;div&gt;You have &lt;span id="ether"&gt;?&lt;/span&gt;.&lt;/div&gt;
&lt;script&gt;
eth.watch({altered: eth.secretToAddress(eth.key)}).changed(function() {
document.getElementById("ether").innerText = eth.toDecimal(eth.balanceAt(eth.secretToAddress(eth.key)))
});
&lt;/script&gt;</code></pre>
Because the ethereum protocol also acts as a large distributed key-store (a happy note for fans of <a href="https://en.wikipedia.org/wiki/NoSQL">NoSQL</a>), eventually user accounts, credentials, and reputation can be migrated on-blockchain with the help of the Whisper communication protocol. In this way, ethereum sets the stage for an total sharding of traditional infrastructure as we know it. No more complex high-availability infrastructure diagrams. In the ethereum ecosystem, even decentralized DNS is free.

<video height="360" controls preload autoplay></video>
<video controls preload autoplay loop>
  <source src="https://blog.ethereum.org/wp-content/uploads/2014/08/GLOBE_LARGE.mp4" type="video/mp4">
  <source src="https://blog.ethereum.org/wp-content/uploads/2014/08/GLOBE_LARGE.webm" type="video/webm">
  Your browser does not support the video tag.
</video>

Evaluating this context in a larger diagram of any systems infrastructure, it’s obvious that our current web isn't as privacy secure or censorship resistant as we desire. Economies of scale have allowed single institutions to offer a vast amount of processing power and storage on the internet for very low prices, thereby increasing their market share to a point where they individually control large segments of internet activity, often under the supervision of less-than-savvy governments. In a post-borders era where the Internet knows no bounds, such jurisdiction has little or no meaning.

As economics of the ethereum ecosystem mature such that open contracts for lowest-rate storage develop, a free market of content hosting could evolve. Given the nature and dynamics of P2P applications, popular content will readily scale as the swarm shares, rather than suffering from the buckling load of siloed servers. The net result is that popular content is delivered <em>faster</em>, not slower.

We’ve spent decades optimizing the protocols that the internet was first founded on, but it’s time to recognize opportunities lost by continually patching the old system instead of curating a new, optimized one. The future will likely bring with it a transition period between traditional and decentralized technologies, where applications live in a hybrid universe and users are unaware of the turbulent undercurrent. But they should be.

This metamorphosis will offer developers an opportunity to build the next-generation of decentralized, private, secure, censorship-resistant platforms that return control to creators and consumers of the next best idea. Anyone with a dream is free to build on this new class of next-generation decentralized web services without owning a credit card or signing up for any accounts.

Although we are not told to or expected to, we have an imperative to cherish and improve the very shared resources that some wish to disturb, manipulate, and control. Just as no single person fully understands the emerging internet collective intelligence, we should not expect any single entity to fully understand or maintain perfectly aligned motives. Rather, we should rely on the internet to solve the problems of the internet.

Because of this, blockchain technologies like Ethereum will allow for simplification and lowering of cost not seen since the introduction of infrastructure-as-a-service (<a href="https://en.wikipedia.org/wiki/Infrastructure_as_a_service#Infrastructure_as_a_service_.28IaaS.29">IaaS</a>). Extending the idea to beyond a simple web project, Ethereum hopes to demonstrate how fully decentralized autonomous organizations (<a href="https://blog.ethereum.org/2014/05/06/daos-dacs-das-and-more-an-incomplete-terminology-guide/">DAOs</a>) can live wholly within cyberspace, negating not only the need for centralized servers, but also trusted third-parties, realizing the dreams of early internet pioneers that envisioned an independent <a title="A Declaration of the Independence of Cyberspace" href="https://projects.eff.org/~barlow/Declaration-Final.html">new home of the mind</a>.