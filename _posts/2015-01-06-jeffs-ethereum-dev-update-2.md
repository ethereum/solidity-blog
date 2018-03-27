---
id: 1313
title: Jeff’s Ethereum ÐΞV Update II
date: 2015-01-06T18:29:05+00:00
author: Jeffrey Wilcke
layout: post
guid: https://blog.ethereum.org/?p=1313
permalink: /2015/01/06/jeffs-ethereum-dev-update-2/
dsq_thread_id:
  - "3395733081"
---
First of all, <a href="https://answers.yahoo.com/question/index?qid=20080105074822AAveQrL">happy new year</a>! What a year it has been. With a little luck we'll surpass last year with an even more awesome year. It's been too long since I've given an update on my side of things and that of the Go team and mostly due to a lack of time. I've been so incredibly busy and so many things have happened these past 2 months I've hardly had time to sit down and assess it all.

As you may be well aware the audit is looming around the corner and my little baby (go-ethereum!) will undergo it's full inspection very, very soon. The audit teams will tear it apart and see if the repo contains anything incorrectly implemented as well as search for any major security flaws in the design and implementation. We've been pretty solid on tests, testing implementation details as well as consensus tests (thanks to Christoph) and will continue to add more tests over time. We'll see how they hold up during the audit (though I'm confident we'll be fine, it's still a little bit scary (-:)
<h2>Development</h2>
<strong>PoC-7</strong> has been released now for a about a week and has been quite stable (and growing in size!). We're already hard at work to finalising PoC-8 which includes numerous small changes:
<ul>
	<li>Adjusted block time back to <code>12s </code>(was <code>4s</code>)</li>
	<li>Op code <code>PREVHASH</code> has become <code>BLOCKHASH( N ) </code>and therefore<code> PREVHASH = BLOCKHASH(NUMBER - 1)</code></li>
	<li>We've added an additional pre-compiled contract at address 0x04 which returns the given input (acts like copy / memcpy)</li>
</ul>
<h4>Ongoing</h4>
<h5>P2P</h5>
Felix has been hard at work on our new P2P package which has now entered in to v0.1 (PoC-7) and will soon already undergo it's first upgrade for PoC-8. Felix has done an amazing job on the design of the package and it's a real pleasure to work with. Auto-generated documentation can be found at <a href="http://godoc.org/github.com/ethereum/go-ethereum/p2p">GoDoc</a>.
<h5>Whisper</h5>
A month or so back I finished the first draft of <a title="Whisper Go" href="https://github.com/ethereum/go-ethereum/wiki/How-to-Whisper">Whisper</a> for the Go implementation and it's now passing whisper messages nicely around the network and uses the P2P package mentioned earlier. The Go API is relatively easy and requires almost zero setup.
<h5>Backend</h5>
The backend stack of ethereum has also received its first major (well deserved) overhaul. Viktor's been incredibly hard at work to reimplement the download manager and the ethereum sub protocol.
<h5>Swarm</h5>
Since the first day Dani joined the team he's passionately been working on the <a title="Cademlia Peer Selection" href="https://github.com/ethereum/wiki/wiki/Cademlia-Peer-Selection">peer selection algorithm</a> and <a title="Distributed Preimage Archive" href="https://github.com/ethereum/wiki/wiki/Distributed-Preimage-Archive">distributed preimage archive</a>. The DPA will be used for our Swarm tech. The spec is about 95% complete and roughly about 50% has been implemented. Progress is going strong!

Both <a href="https://github.com/ethereum/go-ethereum/tree/develop/p2p">go-ethereum/p2p</a> and <a href="https://github.com/ethereum/go-ethereum/tree/develop/whisper">go-ethereum/whisper</a> have been developed in such a way that neither require ethereum to operate. If you're developing in Go and your application requires a P2P network or (dark) messaging try out the packages. An example sub protocol can be found <a title="Peer to peer setup tutorial" href="https://github.com/ethereum/go-ethereum/wiki/Peer-to-Peer">here</a> and an example on how to use Whisper can be found <a title="Whisper Go" href="https://github.com/ethereum/go-ethereum/wiki/How-to-Whisper">here</a>.
<h2>Ams Hub</h2>
Now that the <a href="https://twitter.com/jeffehh/status/530747769611231234">hub</a> is finally set up you're free to drop by and grab a coffee with us. You can find us in the rather posh neighbourhood of Amsterdam Zuid near Museumplein (Alexander Boerstraat 21).

In my next post I hope I'll have a release candidate for PoC-8 and perhaps even a draft implementation of <strong>swarm</strong>. But until then, happy whispering and mining!