---
id: 3844
title: Roundup Round III
date: 2017-05-24T17:04:21+00:00
author: Vitalik Buterin
layout: post
guid: https://blog.ethereum.org/?p=3844
permalink: /2017/05/24/roundup-round-iii/
---
Over the last month and a half we saw vigorous ongoing research and development on all sides of the Ethereum roadmap, and progress is rapidly starting to translate into real results that can be run and verified inside of an Ethereum client.

On Metropolis:

<ul>
 	<li style="font-weight: 400;"><span style="font-weight: 400;">Agendas for core dev meetings 15 and 16 here: </span><a href="https://github.com/ethereum/pm/issues/13"><span style="font-weight: 400;">https://github.com/ethereum/pm/issues/13</span></a><span style="font-weight: 400;"> and </span><a href="https://github.com/ethereum/pm/issues/14"><span style="font-weight: 400;">https://github.com/ethereum/pm/issues/14</span></a></li>
 	<li style="font-weight: 400;"><span style="font-weight: 400;">List of accepted EIPs here: </span><a href="https://github.com/ethereum/EIPs/blob/master/README.md"><span style="font-weight: 400;">https://github.com/ethereum/EIPs/blob/master/README.md</span></a><span style="font-weight: 400;"> (mostly agreed on, though we are still going back and forth on details such as gas costs)</span></li>
 	<li style="font-weight: 400;"><span style="font-weight: 400;">Most of the EIPs have been implemented in C++ and Python and are being implemented in other clients; tests are being actively written and added here </span><a href="https://github.com/ethereum/tests/tree/develop/GeneralStateTests"><span style="font-weight: 400;">https://github.com/ethereum/tests/tree/develop/GeneralStateTests</span></a><span style="font-weight: 400;"> (see the various branches for different tests)</span></li>
 	<li style="font-weight: 400;"><span style="font-weight: 400;">Clients that have not implemented GeneralStateTests could provide a docker image for   Hive testing (contact Martin Swende). All GeneralStateTests are being converted into BlockChainTests and run on Hive.</span></li>
 	<li style="font-weight: 400;"><span style="font-weight: 400;">A release date has still not been finalized; the general consensus is to wait until all tests are passing on major clients before setting one. Because of the rapid increases in block difficulty, the ice age continues to be delayed; current prognosis assuming no further increases (arguably a very pessimistic estimate) is that block times will not exceed 20s until July 12, and will not exceed 30s until Sep 12.</span></li>
</ul>

The Ethereum blockchain has hit several new all-time highs:

<ul>
 	<li style="font-weight: 400;"><a href="https://etherscan.io/chart/difficulty"><span style="font-weight: 400;">Difficulty</span></a><span style="font-weight: 400;"> (450 TH) and </span><a href="https://etherscan.io/chart/hashrate"><span style="font-weight: 400;">hashrate</span></a><span style="font-weight: 400;"> (28.5 TH)</span></li>
 	<li style="font-weight: 400;"><a href="https://etherscan.io/chart/tx"><span style="font-weight: 400;">Transactions per day</span></a><span style="font-weight: 400;"> (187115, or ~2.16 per sec)</span></li>
 	<li style="font-weight: 400;"><span style="font-weight: 400;">Gas usage per day has not yet reached the all-time high of Jun 18, when the blockchain was heavily spammed as part of the DAO attack and various counterattacks, but is nearing it with an </span><a href="https://etherscan.io/chart/tx"><span style="font-weight: 400;">11-month high</span></a><span style="font-weight: 400;"> of 10.7 billion per day. That’s 1991878 gas per block, or ~45% full blocks for the day (reminder: gas limits are </span><a href="https://github.com/ethereum/pyethereum/blob/state_revamp/ethereum/common.py#L14"><span style="font-weight: 400;">dynamically adjusting</span></a><span style="font-weight: 400;">, so congestion with rapidly increasing fees is not likely) . </span><a href="https://etherscan.io/chart/uncles"><span style="font-weight: 400;">Uncle rate</span></a><span style="font-weight: 400;"> on that day was only ~7.4%.</span></li>
</ul>

On various side projects:

<ul>
 	<li style="font-weight: 400;"><span style="font-weight: 400;">ENS has been deployed, and </span><a href="https://ens.codetract.io/"><span style="font-weight: 400;">auctions are ongoing</span></a><span style="font-weight: 400;">.</span></li>
 	<li style="font-weight: 400;"><span style="font-weight: 400;">Whisper is getting a proper API, which is in alignment with our general RPC, the API should be ready soon and a workable whisper version will be released.</span></li>
        <li style="font-weight: 400;"><span style="font-weight: 400;">Swarm has made a number of significant improvements, including (i) supporting directory upload and download via the http interface, (ii) full FUSE support, (iii) a new protocol pss for node-to-node messaging, (iv) replacing the chunk hash with a Merkle tree hash to enable more efficient data inclusion proofs. Progress toward POC3 is going at full steam.</span></li>
</ul>

Pyethereum development has picked up quickly:

<ul>
 	<li style="font-weight: 400;"><span style="font-weight: 400;">Jan Xie and his team have successfully synced a pyethapp node to the most recent block on the mainnet.</span></li>
 	<li style="font-weight: 400;"><span style="font-weight: 400;">Several bugs in the implementation have been fixed, and the client is now passing all state tests and most pre-Metropolis blockchain tests. Work to find the remaining issues is ongoing.</span></li>
 	<li style="font-weight: 400;"><span style="font-weight: 400;">Most Metropolis EIPs have been implemented, including the </span><a href="https://github.com/ethereum/pyethereum/blob/state_revamp/ethereum/specials.py#L74"><span style="font-weight: 400;">four new precompiles</span></a><span style="font-weight: 400;">.</span></li>
 	<li style="font-weight: 400;"><span style="font-weight: 400;">The tester module has been revamped so that it is fully based on the Chain module, and a new and more convenient interface has been added, including functionality such as creating state tests.</span></li>
</ul>

Casper research is now in the process of fine-tuning the incentives for liveness, and implementing the logic inside of pyethereum. This includes:

<ul>
 	<li style="font-weight: 400;"><span style="font-weight: 400;">The </span><a href="https://github.com/ethereum/casper/tree/master/casper/daemon"><span style="font-weight: 400;">validator daemon</span></a></li>
 	<li style="font-weight: 400;"><span style="font-weight: 400;">The hybrid Casper fork choice rule</span></li>
 	<li style="font-weight: 400;"><a href="https://github.com/ethereum/casper/tree/master/casper/contracts"><span style="font-weight: 400;">Casper contracts including tests</span></a></li>
 	<li style="font-weight: 400;"><span style="font-weight: 400;">Signature validation code, including for ECDSA and </span><a href="https://github.com/ethereum/casper/tree/master/casper/validation_codes"><span style="font-weight: 400;">quantum-resistant hash-ladder signatures</span></a></li>
</ul>

Other research stuff:

<li>Substantial work has been done on sharding, and specifically the dreaded “data availability problem”; see here: <a href="https://github.com/ethereum/research/wiki/A-note-on-data-availability-and-erasure-coding"><span style="font-weight: 400;">https://github.com/ethereum/research/wiki/A-note-on-data-availability-and-erasure-coding</li>
<li>The Sharding FAQ <a href="https://github.com/ethereum/wiki/wiki/Sharding-FAQ">has been updated</a></li>
<li>Greg Colvin's "EVM 1.5" proposals are now EIP drafts <a href="https://github.com/ethereum/EIPs/issues/615">615</a> and <a href="https://github.com/ethereum/EIPs/issues/616">616</a></li>

Geth development:

<ul>
 	<li style="font-weight: 400;"><a href="https://github.com/ethereum/go-ethereum/pull/14442"><span style="font-weight: 400;">Removed the artificial 20 shannon minimum gas limit from the transaction pool</span></a><span style="font-weight: 400;">, enabling Geth nodes to accept and relay arbitrarily priced transactions. This should help enable a real gas market.</span></li>
 	<li style="font-weight: 400;"><span style="font-weight: 400;">Identified a sync regression for HDD users, resulting in a </span><a href="https://github.com/ethereum/go-ethereum/pull/14460"><span style="font-weight: 400;">heavy rewrite of fast sync code</span></a><span style="font-weight: 400;">, making it much more robust and stable.</span></li>
 	<li style="font-weight: 400;"><a href="https://github.com/ethereum/go-ethereum/pull/14336"><span style="font-weight: 400;">Identified and fixed a few EVM bottlenecks</span></a><span style="font-weight: 400;">, causing complex contracts to execute up to 60% faster doing up to 95% less memory allocations.</span></li>
 	<li style="font-weight: 400;"><span style="font-weight: 400;">Polishing the light client and its </span><a href="https://github.com/ethereum/go-ethereum/pulls?q=is%3Apr+mobile+milestone%3A1.6.2"><span style="font-weight: 400;">mobile bindings</span></a><span style="font-weight: 400;"> in collaboration with </span><a href="https://status.im/"><span style="font-weight: 400;">Status</span></a><span style="font-weight: 400;">, </span><a href="https://www.reddit.com/r/walleth/"><span style="font-weight: 400;">Walleth</span></a><span style="font-weight: 400;"> and others. Preliminary proof-of-concept done for light client event retrievals.</span></li>
 	<li style="font-weight: 400;"><a href="https://github.com/ethereum/go-ethereum/pull/3749"><span style="font-weight: 400;">Working on a new filtering mechanism</span></a><span style="font-weight: 400;">, with very promising results, reducing the time required to filter the entire blockchain for contract events from minutes to seconds.</span></li>
 	<li style="font-weight: 400;"><span style="font-weight: 400;">Investigating a new mining strategy, which should move transaction processing for miners completely parallel to mining, hence reducing the overhead of transaction inclusion to 0. This should permit miners to lower fees without transactions impacting the probability of finding blocks and/or having them included.</span></li>
 	<li style="font-weight: 400;"><span style="font-weight: 400;">Slowly adding minor polishes to Puppeth, Clique and Rinkeby, such as a </span><a href="https://github.com/ethereum/go-ethereum/pull/14402"><span style="font-weight: 400;">tiered faucet</span></a><span style="font-weight: 400;"> for Ether withdrawals, </span><a href="https://github.com/ethereum/go-ethereum/pull/14453"><span style="font-weight: 400;">configurable gas dynamics</span></a><span style="font-weight: 400;"> for private network miners and automatic ENS integration.</span></li>
 	<li style="font-weight: 400;"><span style="font-weight: 400;">An external security-audit has been performed by Truesec AB. No critical issues were found.</span></li>
</ul>

C++ development:

<ul>
 	<li style="font-weight: 400;"><span style="font-weight: 400;">the monitoring node achieved a full sync to the mainnet</span></li>
 	<li style="font-weight: 400;"><a href="https://github.com/ethereum/cpp-ethereum/issues/4050"><span style="font-weight: 400;">metropolis EIPs</span></a><span style="font-weight: 400;"> are almost complete</span></li>
 	<li style="font-weight: 400;"><span style="font-weight: 400;">work on </span><a href="https://ethereum.stackexchange.com/questions/1161/what-is-geths-fast-sync-and-why-is-it-faster"><span style="font-weight: 400;">fast sync</span></a><span style="font-weight: 400;"> and snapshot sync has begun</span></li>
 	<li style="font-weight: 400;"><span style="font-weight: 400;">Testeth tool has new option --statediff for debug information on a praticular test case as well as new test format  GeneralStateTests (enhancement of the old statetests) [</span><a href="https://github.com/ethereum/cpp-ethereum/pull/4074"><span style="font-weight: 400;">4074</span></a><span style="font-weight: 400;">]</span></li>
</ul>

Remix:

<ul>
 	<li style="font-weight: 400;"><span style="font-weight: 400;">almost finished a new static analysis module that can detect reentrancy bugs (externally contributed by “soad003”) [</span><a href="https://github.com/ethereum/browser-solidity/pull/508"><span style="font-weight: 400;">508</span></a><span style="font-weight: 400;">]</span></li>
</ul>
<ul>
 	<li style="font-weight: 400;"><span style="font-weight: 400;">added a folder view to show open files inside folders - files imported via e.g. import "github.com/ethereum/solidity/std/StandardToken.sol"; are now visible in the folder view [</span><a href="https://github.com/ethereum/browser-solidity/pull/449"><span style="font-weight: 400;">449</span></a><span style="font-weight: 400;">]</span></li>
 	<li style="font-weight: 400;"><span style="font-weight: 400;">debugger can now display mappings [</span><a href="https://github.com/ethereum/remix/pull/498"><span style="font-weight: 400;">498</span></a><span style="font-weight: 400;">]</span></li>
 	<li style="font-weight: 400;"><span style="font-weight: 400;">rearranged tabs to provide better overview [</span><a href="https://github.com/ethereum/browser-solidity/pull/496"><span style="font-weight: 400;">496</span></a><span style="font-weight: 400;">]</span></li>
</ul>

Solidity:

<ul>
 	<li style="font-weight: 400;"><span style="font-weight: 400;">we released the unified standard json interface to interact with the compiler [</span><a href="https://github.com/ethereum/solidity/pull/1639"><span style="font-weight: 400;">1639</span></a><span style="font-weight: 400;">]</span></li>
 	<li style="font-weight: 400;"><span style="font-weight: 400;">added “interface contract” feature[</span><a href="https://github.com/ethereum/solidity/pull/1688"><span style="font-weight: 400;">1688</span></a><span style="font-weight: 400;">]</span></li>
 	<li style="font-weight: 400;"><span style="font-weight: 400;">added some more safety checks: statements without effect, and unary plus and unused variables [</span><a href="https://github.com/ethereum/solidity/pull/2139"><span style="font-weight: 400;">2139</span></a><span style="font-weight: 400;">, </span><a href="https://github.com/ethereum/solidity/pull/2152"><span style="font-weight: 400;">2152</span></a><span style="font-weight: 400;">, </span><a href="https://github.com/ethereum/solidity/pull/2199"><span style="font-weight: 400;">2199</span></a><span style="font-weight: 400;">]</span></li>
</ul>
<ul>
 	<li style="font-weight: 400;"><span style="font-weight: 400;">further work on the new intermediate language including web assembly and EVM1.5 backends [</span><a href="https://github.com/ethereum/solidity/pull/2129"><span style="font-weight: 400;">2129</span></a><span style="font-weight: 400;"> and lots more]</span></li>
 	<li style="font-weight: 400;"><span style="font-weight: 400;">almost finished work on exporting and especially also importing the AST (this will allow mutation testing and lots of other extensions) [</span><a href="https://github.com/ethereum/solidity/pull/1810"><span style="font-weight: 400;">1810</span></a><span style="font-weight: 400;"> and others]</span></li>
</ul>

Mist:

<ul>
 	<li style="font-weight: 400;"><span style="font-weight: 400;">Decouple geth specific hard-coded node and network handling, allow for arbitrary nodes and networks defined by local or remote clientBinaries.json</span></li>
 	<li style="font-weight: 400;"><span style="font-weight: 400;">Refactor of the settings backend in preparation for a settings UI</span></li>
 	<li style="font-weight: 400;"><span style="font-weight: 400;">Added macOS and Windows Code-Signing, preparing github/swarm based auto-updater</span></li>
 	<li style="font-weight: 400;"><span style="font-weight: 400;">Many of the audit findings will incorporated in the coming releases</span></li>
</ul>
<span style="font-weight: 400;">Changes to the provider will make Mist and other Projects like MetaMask and status.im more future proof, as developers don’t need to depend on high level libraries not changing over time. </span><b>There will be a few announcements concerning the new provider</b><span style="font-weight: 400;">. Discussion about that is happening at <a href="https://github.com/ethereum/interfaces/issues/16">https://github.com/ethereum/interfaces/issues/16</a></span>
<br><br>
Web3.js:

<ul>
 	<li style="font-weight: 400;"><span style="font-weight: 400;">We are currently adding generation and signing with private keys right into web3.js. So that you can easily create wallets and sign messages in your dapps. You can find the current experimental docs here: </span><a href="http://web3js.readthedocs.io/en/1.0/web3-eth-accounts.html"><span style="font-weight: 400;">http://web3js.readthedocs.io/en/1.0/web3-eth-accounts.html</span></a></li>
 	<li style="font-weight: 400;"><span style="font-weight: 400;">The next steps will be adding @maiavictor’s swarm library and the new whisper API and the new web3.js should be ready for test drive by the community.</span></li>
</ul>