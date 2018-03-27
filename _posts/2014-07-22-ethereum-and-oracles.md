---
id: 568
title: Ethereum and Oracles
date: 2014-07-22T15:27:47+00:00
author: Vitalik Buterin
layout: post
guid: https://blog.ethereum.org/?p=568
permalink: /2014/07/22/ethereum-and-oracles/
dsq_thread_id:
  - "2864416473"
tags:
  - contracts
  - ethereum
  - oracles
  - smart contracts
  - smart oracles
---
<p>One of the more popular proposals for implementing smart contracts differently from the way they are typically presented in Ethereum is through the concept of oracles. Essentially, instead of a long-running contract being run directly on the blockchain, all funds that are intended to go into the contract would instead go into an M-of-N multisig address controlled by a set of specialized entities called "oracles", and the contract code would be simultaneously sent to all of these entities. Every time someone wants to send a message to the contract, they would send the message to the oracles. The oracles would run the code, and if the code execution leads to a withdrawal from the contract to some particular address then the oracles circulate a transaction sending the funds and sign it.</p>

<p>The approach is still low-trust, as no single oracle has the ability to unilaterally withdraw the funds, but it has a number of particular advantages:</p>

<ol>
<li>Not every node in the blockchain needs to perform the computation - only a small number of oracles do</li>
<li>It theoretically does not require as a platform anything more complicated than Bitcoin or Ripple as they currently stand</li>
<li>Contracts have a somewhat higher degree of privacy - although exit transactions are still all visible, internal computations may not be. The scheme can also be augmented with <a href="https://en.wikipedia.org/wiki/Secure_multiparty_computation">secure multiparty computation</a> protocols so the contract can even contain private information (something that would take <a href="http://bitcoinmagazine.com/10055/cryptographic-code-obfuscation-decentralized-autonomous-organizations-huge-leap-forward/">efficient and secure obfuscation</a> to work directly on Ethereum)</li>
<li>Contracts can rely on external information (eg. currency prices, weather) since it is much easier for N nodes to come to consensus on the result of an HTTP request than an entire blockchain. In fact, they can even rely on data from proprietary APIs, if the oracles subscribe to the APIs and pass along the costs to the contract users.</li>
</ol>

<p>Given all of these advantages, it is undeniably clear that oracles have the potential to be a very useful paradigm for smart contracts going forward. However, the key question is, how will oracle-based computation and blockchain-based computation, as in Ethereum, interact with each other?</p>

<h3>Oracles Are Not Always Better</h3>

<p>First of all, one important point to make is that it will not always be the case that the oracle-based method of contract execution will be more efficient than the blockchain-based approach (not to mention non-currency/non-contract uses of the blockchain such as name registries and the <a href="https://www.youtube.com/watch?v=oZVsIiQ1ME0">People's Republic of DOUG</a> where oracle systems do not even begin to apply). A common misconception is that the primary feature of Ethereum is that it is Turing-complete, and so while Bitcoin only allows quick scripts for verification Ethereum contracts are means to do much harder and computationally intensive tasks. This is arguably a misconception.

The primary feature of Ethereum is not Turing-completeness; in fact, we have a <a href="https://github.com/ethereum/wiki/wiki/%5BEnglish%5D-White-Paper#computation-and-turing-completeness">section in our whitepaper</a> which makes the argument that even if we explicitly removed the ability of Ethereum contracts to be Turing-complete it would actually change very little and there would still be a need for "gas". In order to make contracts truly statically analyzable, we would need to go so far as to remove the first-class-citizen property (namely, the fact that contracts can create and call other contracts), at which point Ethereum would have very limited utility.

Rather, the primary feature of Ethereum is state - Ethereum accounts can contain not just a balance and code, but also arbitrary data, allowing for multi-step contracts, long-running contracts such as <a href="http://blog.ethereum.org/2014/05/06/daos-dacs-das-and-more-an-incomplete-terminology-guide/">DOs/DACs/DAOs</a> and particularly non-financial blockchain-based applications to emerge. For example, consider the following contract:</p>

<pre><code>init:
    contract.storage[0] = msg.data[0] # Limited account
    contract.storage[1] = msg.data[1] # Unlimited account
    contract.storage[2] = block.timestamp # Time last accessed
code:
    if msg.sender == contract.storage[0]:
        last_accessed = contract.storage[2]
        balance_avail = contract.storage[3]

        # Withdrawal limit is 1 finney per second, maximum 10000 ether
        balance_avail += 10^15 * (block.timestamp - last_accessed)
        if balance_avail &gt; 10^22:
            balance_avail = 10^22

        if msg.data[1] &lt;= balance_avail: 
            send(msg.data[0], msg.data[1])
            contract.storage[3] = balance_avail - msg.data[1]
            contract.storage[2] = block.timestamp

    # Unlimited account has no restrictions
    elif msg.sender == contact.storage[1]:
        send(msg.data[0], msg.data[1])
</code></pre>

<p>This contract is pretty straightforward. It is an account with two access keys, where the first key has a withdrawal limit and the second key does not. You can think of it as a cold/hot wallet setup, except that you do not need to periodically go to the cold wallet to refill unless you want to withdraw a large amount of ether all at once. If a message is sent with data <code>[DEST, VALUE]</code>, then if the sender is the first account it can send up to a certain limit of ether, and the limit refills at the rate of 1 finney per second (ie. 86.4 ether per day). If the sender is the second account, then the account contract sends the desired amount of ether to the desired destination with no restrictions. Now, let's see what expensive operations are required to execute here, specifically for a withdrawal with the limited key:</p>

<ol>
<li>An elliptic curve verification to verify the transaction</li>
<li>2 storage database reads to get the last access time and last withdrawable balance</li>
<li>1 storage database write to record the balance changes that result from the sending transaction</li>
<li>2 storage database writes to write the new last access time and withdrawable balance</li>
</ol>

<p>There are also a couple dozen stack operations and memory reads/writes, but these are much faster than database and cryptography ops so we will not count them. The storage database reads can be made efficient with caching, although the writes will require a few hashes each to rewrite the Patricia tree so they are not as easy; that's why <code>SLOAD</code> has a gas cost of 20 but <code>SSTORE</code> has a cost of up to 200. Additionally, the entire transaction should take about 160 bytes, the Serpent code takes up 180 bytes, and the four storage slots take up 100-150 bytes - hence, 350 bytes one-time cost and 160 bytes bandwitdh per transaction.</p>

<p>Now, consider this contract with a multisig oracle. The same operations will need to be done, but only on a few servers so the cost is negligible. However, when the multisig transaction is sent to Bitcoin, if the multisig is a 3-of-5 then three elliptic curve verifications will be required, and the transaction will require 65 bytes per signature plus 20 bytes per public key so it will take about 350-400 bytes altogether (including also metadata and inputs). The blockchain storage cost will be around 50 bytes per UTXO (as opposed to a static 350 in Ethereum). Hence, assuming that an elliptic curve verification takes longer than a few hashes (it does), the blockchain-based approach is actually easier. The reason why this example is so favorable is because it is a perfect example of how Ethereum is about state and not Turing-completeness: no loops were used, but the magic of the contract came from the fact that a running record of the withdrawal limit could be maintained inside the contract.</p>

<p>(Note: advanced cryptographers may note that there is a specialized type of threshold signature that actually requires only one verification operation even if a large number of oracles are used to produce it. However, if we use a currency with such a feature built-in, then we are already abandoning Bitcoin's existing infrastructure and network effect; in that case, why not just use the Ethereum contract?)</p>

<h3>But Sometimes They Are</h3>

<p>At other times, however, oracles do make sense. The most common case that will appear in reality is the case of external data; sometimes, you want a financial contract that uses the price of the US dollar, and you can't cryptographically determine that just by doing a few hashes and measuring ratios. In this case, oracles are absolutely necessary. Another important case is smart contracts that actually are very hard to evaluate. For example, if you are purchasing computational resources from a decentralized cloud computing application, verifying that computations were done legitimately is not a task that the Ethereum blockchain can cheaply handle. For most classes of computation, verifying that they were done correctly takes exactly as long as doing them in the first place, so the only way to practically do such a thing is through occasional spot-checking using, well, oracles. Another cloud-computing use case for oracles, although in this context we do not think of them as such, is file storage - you absolutely do not want to back up your 1GB hard drive onto the blockchain.</p>

<p>An additional use-case, already mentioned above, is privacy. Sometimes, you may not want the details of your financial contracts public, so doing everything on-chain may not be the best idea. Sure, you can use standard-form contracts, and people won't know that it's <em>you</em> who is making a contract for difference between ETH and USD at 5:1 leverage, but the information leakage is still high. In those cases, you may want to limit what is done on-chain and do most things off-chain.</p>

<h3>So How Can They Work Together</h3>

<p>So we have these two paradigms of total on-chain and partial on-chain, and they both have their relative strengths and weaknesses. However, the question is, are the two really purely competitive? The answer is, as it turns out, no. To further this point, here are a few particular examples:</p>

<ol>
<li><strong>SchellingCoin</strong> - incentivized decentralized oracles. The <a href="https://blog.ethereum.org/2014/03/28/schellingcoin-a-minimal-trust-universal-data-feed/">SchellingCoin protocol</a> is a proof-of-concept that shows how we can create a decentralized oracle protocol that is incentive-compatible: have a two-step commitment protocol so that oracles do not initially know what each other's answers are, and then at the end have an Ethereum contract reward those oracles that are closest to the median. This incentivizes everyone to respond with the truth, since it is very difficult to coordinate on a lie. An independently conceived alternative, <a href="https://news.ycombinator.com/item?id=7691289">TruthCoin</a>, does a similar thing for prediction markets with binary outcomes (eg. did the Toronto Maple Leafs win the World Cup?).</li>
<li><strong>Verifiable computation oracles</strong> - when the oracles in question are executing moderately computationally intensive code, then we can actually go beyond the admittedly flaky and untested economics of the SchellingCoin/TruthCoin protocols. The idea is as follows. By default, we have M of N oracles running the code and providing their votes on the answers. However, when an oracle is perceived to vote incorrectly, that oracles can be "challenged". At that point, the oracle must provide the code to the blockchain, the blockchain checks the code against a pre-provided hash and runs the code itself, and sees if the result matches. If the result does not match, or if the oracle never replies to the challenge, then it loses its security deposit. The game-theoretic equilibrium here is for there to be no cheating at all, since any attempt at cheating necessarily harms some other party and so that party has the incentive to perform a check.</li>
<li><strong>Signature batching</strong> - one of the problems that I pointed out with the multisig oracle approach above is signature bloat: if you have three oracles signing everything, then that's 195 extra bytes in the blockchain and three expensive verification operations per transaction. However, with Ethereum we can be somewhat more clever - we can come up with a specialized "oracle contract", to which oracles can submit a single transaction with a single signature with a large number of votes batched together: <code>[addr1, vote1, addr2, vote2 ... ]</code>. The oracle contract then processes the entire list of votes and updates all of the multisig voting pools contained inside it simultaneously. Thus, one signature could be used to back an arbitrarily large number of votes, reducing the scalability concerns substantially.</li>
<li><p><strong>Blockchain-based auditing</strong> - the concept of oracle-based computation can actually go much further than the "Bitcoin multisig oracle" (or, for that matter, Ethereum multisig oracle) idea. The extreme is an approach where oracles also decide the one thing that the Bitcoin-based schemes still leave the blockchain to decide: the order of transactions. If we abandon this requirement, then it is possible to achieve much higher degrees of efficiency by having an oracle maintain a centralized database of transactions and state as they come, providing a signed record of each new balance sheet as a transaction is applied, allowing for applications like microtransactions and high-frequency trading. However, this has obvious trust-problems; particularly, what if the oracle double-spends? </p><p>Fortunately, we can set up an Ethereum contract to solve the problem. Much like the verifiable computation example above, the idea is that by default everything would run entirely on the oracle, but if the oracle chooses to sign two different balance sheets that are the result of incompatible transactions then those two signatures can be imported into Ethereum, and the contract will verify that those two signatures are valid, and if they are the contract will take away the oracle's security deposit. More complicated schemes to deal with other attack vectors are also possible.</p></li>
<li><strong>Verifiable secure multiparty computation</strong> - in the case where you are using oracles specifically for the purpose of maintaining private data, you can set up a protocol where the oracles securely choose a new secret key using multiparty random number generation every 24 hours, sign a message with the old key to prove to the world that the new key has authority, and then have to submit all of the computations that they made using the old key to the Ethereum blockchain for verification. The old key would be revealed, but it would be useless since a message transferring ownership rights to the new key is already in the blockchain several blocks before. Any malfeasance or nonfeasance revealed in the audit would lead to the loss of a security deposit.</li>
</ol>

<p>The larger overarching point of all this is that the primary raison d'être of Ethereum is not just to serve as a smart contract engine; it is more generally to serve as a world-wide trust-free decentralized computer, albeit with the disadvantages that it can hold no secrets and it is about ten thousand times slower than a traditional machine. The work in developing cryptoeconomic protocols to ensure that ordinary people have access to reliable, trustworthy and efficient markets and institutions is not nearly done, and the most exciting end-user-centric innovation is likely what will be built on top. It is entirely possible to have systems which use Ethereum for one thing, an M-of-N oracle setup for another thing, and some alternative network like Maidsafe for something else; base-level protocols are your servant, not your master.</p>

<i>Special thanks to Vlad Zamfir for some of the ideas behind combining oracles and Ethereum</i>