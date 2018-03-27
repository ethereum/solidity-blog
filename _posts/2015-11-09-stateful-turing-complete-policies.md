---
id: 2286
title: Stateful Turing-Complete Policies
date: 2015-11-09T07:20:43+00:00
author: Vitalik Buterin
layout: post
guid: https://blog.ethereum.org/?p=2286
permalink: /2015/11/09/stateful-turing-complete-policies/
dsq_thread_id:
  - "4302189241"
---
<p>One of the major security challenges of the internet over the last twenty years has consistently been the rather simple problem of securing user accounts. Right now, users have accounts with hundreds of websites, and dozens of passwords, leading to <a href="http://www.pcworld.com/article/257045/6_5m_linkedin_passwords_posted_online_after_apparent_hack.html">large</a> <a href="http://www.digitaltrends.com/web/most-common-hacked-ashley-madison-passwords-revealed/">numbers</a> of <a href="http://www.pcworld.com/article/257045/6_5m_linkedin_passwords_posted_online_after_apparent_hack.html">hacks</a> as individual websites, often run by people not particularly skilled in the finer points of cryptography and internet security, find themselves exploited by increasingly clever hackers, and users frequently deal with the complexity of remembering hundreds of passwords by either <a href="http://gizmodo.com/the-25-most-popular-passwords-of-2014-were-all-doomed-1680596951">making them simple</a> or <a href="http://www.pcworld.com/article/219303/password_use_very_common_research_shows.html">making them all the same</a> - with often very <a href="http://www.itproportal.com/2013/05/29/hackers-crack-over-16000-passwords-90-cent-success/">unfortunate results</a>. Over time, a patchwork of ad-hoc solutions has certainly developed, including the use of one's email account as a universal backup, and "password manager" software like Lastpass, though at high cost: such solutions either retain much of the underlying complexity of password-bsaed access or give centralized companies very high degrees of control over your online life.</p>

<p>There are many calls to <a href="http://www.latimes.com/business/technology/la-fi-tn-yahoo-mail-password-20151014-story.html">get</a> <a href="http://qz.com/437920/is-it-time-to-finally-get-rid-of-the-password/">rid</a> <a href="http://www.markus-jakobsson.com/getting-rid-of-passwords-possible-now">of</a> <a href="http://www.engadget.com/2015/06/09/governments-join-fido-alliance/">passwords</a>, but the question is: what do we replace them with? There are many ideas, ranging from "one single password to rule them all" to smartphone authentication to specialized hardware devices and biometrics and all sorts of multi-factor M-of-N policies, but even these more complex constructions so far have typically been application-specific: many banks now give you a specialized access device to log into your bank account, but if you trust its security you cannot also use it to access your email. In general, we see that the problem of how to best manage user access control and minimize key loss and theft risks is complex enough that it never will be solved "once and for all", and so the best way to solve it is to allow a free market of solutions to flourish and let each user pick which ones work best for them; however, the way to make that actually happen is by <a href="https://www.youtube.com/watch?v=foEwoWRYIu4">unbundling</a> the "access control solutions" market from the "services" market. That is to say, exactly what we are to a large extent <em>not</em> doing right now.</p>

<p><center>
<img src="https://blog.ethereum.org/wp-content/uploads/2015/11/kartenleser-mit-access-card.jpeg" alt="" title="" />
<small>
The hardware access device to my UBS bank account. Remind me, why can't I also use this to secure my domains on Namecheap?
</small>
</center></p>

<p>So how do we do that? The first step is to introduce some well-placed use of the ultimate abstraction: Turing-complete code. Rather than, at the protocol level, allowing users to specify a password, or providing a pre-selected set of providers, or even a standard which relies on talking to a server of the user's choice, allow access policies to be specified in code to be executed in a deterministic virtual machine (where the EVM is a good a start as any). Code can include digital signature verifications using <em>any</em> cryptographic algorithm (so you get forward-compatibility with quantum-safe crypto for free), potentially including keys held on the user's computer, keys directly derived <a href="https://blog.ethereum.org/2014/10/23/information-theoretic-account-secure-brainwallets/">from a password</a>, keys held on a hardware device or any arbitrary policy including any combination of the above. This way, innovation can happen in access-control mechanisms <em>without any need</em> for websites (or other systems requiring authentication) to do anything to accomodate new changes. Additionally, the system neatly allows <em>organizations</em> to use the scheme using multi-person access controls right away, without any further need for integration.</p>

<p>The next step is Turing-complete operation-dependent code. For many applications, you want the ability to authorize some users to carry out some operations but not others; for example, you may want to authorize a sysadmin to change the IP address that a domain name points to, but not sell the domain outright. To accomodate this, the abstraction needs to change. A simple "Turing-complete-code as signature" setup might have the following form:</p>

<pre><code>VM(code, server-provided nonce ++ signature) ?= 1
</code></pre>

<p>Where <code>VM</code> is a virtual machine that runs code, taking a server-provided nonce and a signature as input, and the verification check is to see whether or not the output is 1. A simple example of <code>code</code> that could be put in is an elliptic curve digital signature verifier. To allow different authorization requirements depending on the operation, you want:</p>

<pre><code>VM(code, server-provided nonce ++ operation_data ++ signature) ?= 1
</code></pre>

<p>A signature would need to be provided with every operation that the user wants to carry out (this has the benefit of providing definite, third-party-verifiable, proof that an operation was authorized); the operation data (imagine the function name and the arguments encoded in an <a href="https://github.com/ethereum/wiki/wiki/Ethereum-Contract-ABI">Ethereum-style ABI</a>) would be added as an argument for the virtual machine, and the signature would have to be over both the nonce and the operation data.</p>

<p>This gets you quite far, but in some cases not far enough. One simple example is this: what if you want to give someone permission to withdraw small amounts of money but not large amounts, ie. a withdrawal limit? In that case, the problem that you must overcome is simple: what if someone limited by a withdrawal cap of $100 tries to evade it by simply running a script to withdraw $90 over and over again? To solve this, you need a smarter withdrawal limit; essentially, something like "maximum $100 per day". Another natural case is key revocation: if a key gets hacked or lost, you want to replace it, and you want to make sure that the world finds out that your policy was changed so that attackers cannot try to impersonate you under your old policy.</p>

<p>To get past this last hump, we need to go one step further: we need Turing-complete operation-dependent <em>stateful</em> policies; that is to say, operations should be able to change the state of the policy. And here is where not just cryptography, but specifically blockchains come in. Of course, you could just have a central server manage the whole thing, and many people are perfectly fine with trusting a central server, but <a href="https://blog.ethereum.org/2015/04/13/visions-part-1-the-value-of-blockchain-technology/">blockchains are moderately valuable</a> here because they are more convenient, provide a credible story of neutrality, and are easier to standardize around. Ultimately, as it would be quite harmful for innovation to permanently choose "one blockchain to rule them all", the thing that we want to standardize is a mechanism by which users can download modules to support <em>any</em> blockchain or centralized solution as they wish.</p>

<p>For blockchain-based applications, having a stateful policy enforced right on the blockchain makes natural sense; there is no need to involve yet another special class of intermediaries, and people can start doing it right now. The abstraction of an "account" that Ethereum offers makes it extremely easy to work with this approach: if your application works with simple users holding private keys, it also works for just about every kind of individual, multiparty, hardware-driven, military-grade or whatever other policy users will come up with in the future.</p>

<p>For other applications, users may want privacy, both in the state-changing operations that they perform and even in the nature of their policy at any one particular time. For this reason, you likely want a solution like <a href="http://oblivm.com/hawk">Hawk</a>, where the blockchain still ensures the security of the process but, thanks to the wonders of zero-knowledge-proof technology, <em>knows nothing about what is being secured</em>; before Hawk is implemented, simpler forms of cryptography such as ring signatures may suffice.</p>

<h3>Other Applications</h3>

<p>Account security is the first, and most basic, application for the concept of code as policy, there are also others. One simple one is a domain name registry. Onename, one of the popular "decentralized name registry" services, is currently planning on implementing a feature where top-level domains can choose fee policies for subdomains based on the number of letters, consonants and vowels. This is useful, but of course economically ugly: there are definitely hundreds of characteristics other than letters, consonants and vowels that can influence a domain name price, and people may even want to experiment with other registration strategies like <a href="http://github.com/ethereum/dapp-bin/tree/master/ether_ad">different types of auctions</a>.</p>

<p>Once again, an even nicer solution is to apply some simple modularity: let people create their own namespace in stateful Turing-complete code. If you are doing this on a platform where stateful Turing-complete code exists, you can just allow an address to control a subdomain, and then, tada, you support stateful Turing-complete subdomain policies already. This is the essence of object-oriented programming: expose an interface, and allow other objects, which can have arbitrarily complex internal code, satisfy that interface.</p>

<p>A further one is private stock trading. Particularly in the case of privately held companies, stock trading is not, and cannot, be completely free and unrestricted the way that trading of cryptocurrencies is; companies often want to have restrictions such as:</p>

<ul>
<li>Giving employees shares and allowing them to sell them only after some period of time</li>
<li>Requiring new shareholders to be approved by existing shareholders, with the possibility of such approvals coming with a cap on how many shares can be owned by that specific holder</li>
<li>Forced-buyout procedures</li>
<li>Restricting the maximum rate at which stocks are sold (ie. withdrawal limits) or requiring waiting periods or offering specific other holders right of first refusal</li>
</ul>

<p>Sure, you can create a private blockchain-based stock trading platform for one client, and provide the restrictions that that one client wants. But what if other clients want different restrictions? You may as well nip the problem in the bud, at least at the "core application layer", and solve it once and for all by... allowing each individual stock, represented as a sub-currency, to have <a href="http://github.com/ethereum/dapp-bin/tree/master/private_market">restrictions represented</a> as stateful Turing-complete code.</p>

<p>This functionality can be represented in the "token" API by extending it, for example, as follows:</p>

<ul>
<li><code>getMinimumBalance(account)</code>: get the minimum balance that an account can hold at the current time</li>
<li><code>getMaximumBalance(account)</code>: get the maximum balance that an account can hold at the current time</li>
</ul>

<p>In short, applications don't have policies; applications interact with <em>objects</em> (user accounts, currencies, etc), and objects have policies. Alternatively, even shorter:</p>

<p><center>
<img src="http://i.imgur.com/u2IZBtj.jpg" alt="" title="" />
</center></p>

<p>Are you building a blockchain-based financial derivatives application, and someone is asking you to add a feature to allow a vote between multiple data feed providers instead of just one? Don't even think about it; instead, just set one data feed provider address, and allow users to come up with their own policies; the upside is that whatever code they use or write, they'll be able to use to more securely provide data feeds for the arbitration dapp as well. Are you building a DNS system, and someone is asking you to introduce support for specific auction types for subdomains? Don't to it at the root DNS level; instead, allow subdomains to be addresses, and allow users to invent their own auction algorithms; whatever algorithms they invent, they'll be able to use for their registry for decentralized chat usernames as well.</p>

<p>This is the benefit of abstraction: account security policy design can become a self-contained field of study to itself, and whatever new solutions exist can instantly be applied everywhere. Some people will want to trust a third party; others will want to have a multi-signature authorization between five of their own different devices, and some will want a key to themselves with the option for three of five friends to come together to reset the key to a new one. Some will want an access policy where, if they make no transactions within twelve months, they are presumed dead and a lawyer will gain access in order to be able to execute on their will - for <i>all</i> of their digital assets. And some will want a policy which gives one key full control for applications that declare themselves low-security but two of three keys for applications that declare themselves high-security. Name registry pricing policy design can become self-contained as well - as can digital asset ownership restriction policy, a field that would interest everyone from small and large traditional corporations to community-based DAOs. And that is the power of a stateful Turing-complete code.</p>
