---
id: 1707
title: 'Olympic: Frontier Pre-Release'
date: 2015-05-09T15:11:09+00:00
author: Vitalik Buterin
layout: post
guid: https://blog.ethereum.org/?p=1707
permalink: /2015/05/09/olympic-frontier-pre-release/
dsq_thread_id:
  - "3749698755"
---
<p>What began all the way back on midnight of February 1st 2014, is coming to a close: we are happy to announce the release of the ninth and last in the Ethereum Proof-of-Concept series.  We invite the community to participate in the ongoing Proof-of-Concept IX testnet in our present release, Olympic, <a href="https://github.com/ethereum/go-ethereum/releases/tag/v0.9.18">made available now.</a></p>

<p>The purpose of Olympic is to reward people who try to test the limits of the Ethereum blockchain during the pre-release period, spamming the network with transactions and doing crazy things with the state, so that we can see how the network holds up under high levels of load. At the same time, application developers, data providers, exchanges, and users are encouraged to develop and deploy on the testnet and run nodes - and if you have multiple virtual private servers, spin up as many nodes as you can.</p>

<p>Olympic will feature a total prize fund of up to 25,000 ether. There will be four categories of prize as well as a grand prize for the first person to create a substantial fork on the testnet.</p>

<p>The four categories of prize will be <b>Transaction Activity</b>, <b>Virtual Machine Usage</b>, <b>Mining Prowess</b> and <b>General Punishment</b>. Each category will include a main prize of 2,500 ether, as well as one or more smaller prizes at 100 - 1,000 ether and possibly tiny rewards of 0.1-5 ether simply for participating. Each of the categories will be judged by Vitalik, Gavin and Jeff, likely with substantial help from automated blockchain analysis tools. In addition to the Ether, like the bounty programme, all prize-winners will be entitled to have their name immortalised in the Ethereum Genesis block. Please note that to be considered for a prize, send an e-mail describing a claim to olympic@ethereum.org.</p>

<p>This is also the last phase of the Ethereum development process before the Frontier release, as the network is currently proving to be highly stable at its current size of 20-100 nodes, all major clients have been staying in consensus and we are approaching code freeze pending testing and auditing inputs. <b>It is expected to last close to 14 days, though we reserve the right to shorten or lengthen it based on technical considerations. When we deem that we are ready, we will provide a 48 hour countdown for the Frontier 1.0 launch.</b></p>

<p>Binaries and source are available here: <a href="https://github.com/ethereum/go-ethereum/releases/tag/v0.9.18">https://github.com/ethereum/go-ethereum/releases/tag/v0.9.18</a></p>

<p>A work-in-progress guide to Frontier is available here: <a href="http://ethereum.gitbooks.io/frontier-guide/">http://ethereum.gitbooks.io/frontier-guide/</a></p>

<h2>Transaction Activity</h2>

<p>This includes activity related to sending and receiving transactions. Examples of things we might reward include:</p>

<ul>
<li>The account that sends the highest total number of transactions</li>
<li>The account that contributes the highest total of gas usage</li>
<li>The account that receives the highest total number of transactions</li>
<li>The account that contributes the highest bloat (as measured in bytes) to the blockchain in terms of transaction trie contents</li>
<li>The account with the lowest address (in terms of lexicographic order or numerical representation; the two orders are equivalent) that sends at least one transaction</li>
<li>The two accounts that send at least one transaction whose addresses are closest to each other</li>
</ul>

<h2>Virtual Machine Usage</h2>

<p>This includes activity relating to using the virtual machine. Examples of things we might reward include:</p>

<ul>
<li>The account that makes the highest number of calls of any opcode</li>
<li>The account that makes the highest number of calls of any opcode within a single transaction</li>
<li>The account that sends the single transaction that takes the longest for a particular client to execute</li>
<li>The account that sends the single transaction with the highest ratio of time to execute per unit gas consumed</li>
<li>The account that receives the highest total number of messages</li>
<li>The account that reverts the highest total amount of gas usage</li>
<li>The account that creates the contract with the lowest address (in terms of lexicographic order or numerical representation; the two orders are equivalent)</li>
<li>The account that sends the transaction with the largest (as measured in bytes) encoded receipt RLP</li>
<li>The account that produces the largest number of transactions whose bloom is a (distinct) prime number</li>
<li>A creator of a contract that contains, for example, a sudoku solver, taking an array of 81 values as input and outputting the array of 81 values corresponding to a solved sudoku</li>
</ul>

<h2>Mining Prowess</h2>

<p>This includes activity relating to mining; miners will be identified by coinbase address. Examples of things we might reward include:</p>

<ul>
<li>The miner that produces the largest block in the main chain (as measured in bytes)</li>
<li>The miner that contributes the largest total number of bytes of block data to the main chain</li>
<li>The miner that mines the largest number of blocks in a row</li>
<li>The miner that includes the most transactions</li>
<li>The miner that produces the block that takes the longest amount of time for a particular client to process</li>
</ul>

<h2>General Punishment</h2>

<p>This includes mucking around with the State in crazy ways, getting contracts to do crazy things. Examples of things we might reward include:</p>

<ul>
<li>The sender/miner of the transaction/block that adds the largest number of items to contract storage</li>
<li>The creator of the contract with the largest code (externally-owned accounts only)</li>
<li>The creator of the contract with the most filled items in storage</li>
<li>The creator of the contract that contains a key/value pair with the highest merkle tree depth (ie. the largest number of hash lookups before getting to the value)</li>
<li>The creator of the contract that suicides while containing the most filled items in storage</li>
<li>The creator of the contract that makes the largest number of consecutive storage updates such that the storage root at each step is a prime number.</li>
</ul>

<p>Note that the above categories are by no means an exhaustive list of things that are eligible for prizes. If you do anything outside of the above that you feel merits consideration, please email us.</p>

<h2>The Grand Prize</h2>

<p>A grand prize of at least 5,000 ether will be shared between the miner(s) who manage to create a substantial fork between the Go and C++ clients. The fork must be on the main chain; one client must accept the block with the other client rejecting it. Smaller prizes for forks between Go/C++ and Python may also be available.</p>

<p>Good luck, and we look forward to seeing and hearing what you all come up with!</p>

<p>- The ethereum core dev team</p>
