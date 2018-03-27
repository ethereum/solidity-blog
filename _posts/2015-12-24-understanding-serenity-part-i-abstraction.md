---
id: 2356
title: 'Understanding Serenity, Part I: Abstraction'
date: 2015-12-24T02:23:48+00:00
author: Vitalik Buterin
layout: post
guid: https://blog.ethereum.org/?p=2356
permalink: /2015/12/24/understanding-serenity-part-i-abstraction/
dsq_thread_id:
  - "4429419462"
---
<em>Special thanks to Gavin Wood for prompting my interest into abstraction improvements, and Martin Becze, Vlad Zamfir and Dominic Williams for ongoing discussions.</em>

<p>For a long time we have been public about our plans to continue improving the Ethereum protocol over time and our long development roadmap, learning from our mistakes that we either did not have the opportunity to fix in time for 1.0 or only realized after the fact. However, the Ethereum protocol development cycle has started up once again, with a Homestead release coming very soon, and us quietly starting to develop proof-of-concepts for the largest milestone that we had placed for ourselves in our <a href="https://blog.ethereum.org/2015/03/03/ethereum-launch-process/">development roadmap</a>: Serenity.</p>

<p>Serenity is intended to have two major feature sets: abstraction, a concept that I initially expanded on in <a href="https://blog.ethereum.org/2015/07/05/on-abstraction/">this blog post here</a>, and Casper, our security-deposit-based proof of stake algorithm. Additionally, we are exploring the idea of adding at least the scaffolding that will allow for the smooth deployment over time of <a href="https://www.youtube.com/watch?v=-QIt3mKLIYU">our scalability</a> <a href="https://blog.ethereum.org/2015/04/05/blockchain-scalability-chain-fibers-redux/">proposals</a>, and at the same time completely resolve parallelizability concerns <a href="http://www.multichain.com/blog/2015/11/smart-contracts-slow-blockchains/">brought up here</a> - an instant very large gain for private blockchain instances of Ethereum with nodes being run in massively multi-core dedicated servers, and even the public chain may see a 2-5x improvement in scalability. Over the past few months, research on Casper and formalization of scalability and abstraction (eg. with <a href="https://github.com/ethereum/EIPs/issues/28">EIP 101</a>) have been progressing at a rapid pace between myself, Vlad Zamfir, Lucius Greg Meredith and a few others, and now I am happy to announce that the first proof of concept release for Serenity, albeit in a very limited form suitable only for testing, <a href="https://github.com/ethereum/pyethereum/tree/serenity/">is now available</a>.</p>

<p>The PoC can be run by going into the <code>ethereum</code> directory and running <code>python test.py</code> (<b>make sure to download and install the latest Serpent from <a href="https://github.com/ethereum/serpent">https://github.com/ethereum/serpent</a>, develop branch</b>); if the output looks something like this then you are fine:</p>

<pre>
vub@vub-ThinkPad-X250 15:01:03 serenity/ethereum: python test.py
REVERTING 940534 gas from account 0x0000000000000000000000000000000000000000 to account 0x98c78be58d729dcdc3de9efb3428820990e4e3bf with data 0x
Warning (file "casper.se.py", line 74, char 0): Warning: function return type inconsistent!
Running with 13 maximum nodes
Warning (file "casper.se.py", line 74, char 0): Warning: function return type inconsistent!
Warning (file "casper.se.py", line 74, char 0): Warning: function return type inconsistent!
Length of validation code: 57
Length of account code: 0
Joined with index 0
Length of validation code: 57
Length of account code: 0
Joined with index 1
Length of validation code: 57
</pre>

<p>This is a simulation of 13 nodes running the Casper+Serenity protocol at a 5-second block time; this is fairly close to the upper limit of what the client can handle at the moment, though note that (i) this is python, and C++ and Go will likely show much higher performance, and (ii) this is all nodes running on one computer at the same time, so in a more "normal" environment it means you can expect python Casper to be able to handle at least ~169 nodes (though, on the other hand, we want consensus overhead to be much less than 100% of CPU time, so these two caveats combined do NOT mean that you should expect to see Casper running with thousands of nodes!). If your computer is too slow to handle the 13 nodes, try <code>python test.py 10</code> to run the simulation with 10 nodes instead (or <code>python test.py 7</code> for 7 nodes, etc). Of course, research on improving Casper's efficiency, though likely at the cost of somewhat slower convergence to finality, is still continuing, and these problems should reduce over time. The <code>network.py</code> file simulates a basic P2P network interface; future work will involve swapping this out for actual computers running on a real network.</p>

<p>The code is split up into several main files as follows:</p>

<ul>
<li><code>serenity_blocks.py</code> - the code that describes the block class, the state class and the block and transaction-level transition functions (about 2x simpler than before)</li>
<li><code>serenity_transactions.py</code> - the code that describes transactions (about 2x simpler than before)</li>
<li><code>casper.se.py</code> - the serpent code for the Casper contract, which incentivizes correct betting</li>
<li><code>bet.py</code> - Casper betting strategy and full client implementation</li>
<li><code>ecdsa_accounts.py</code> - account code that allows you to replicate the account validation functionality available today in a Serenity context</li>
<li><code>test.py</code> - the testing script</li>
<li><code>config.py</code> - config parameters</li>
<li><code>vm.py</code> - the virtual machine (faster implementation at <code>fastvm.py</code>)</li>
<li><code>network.py</code> - the network simulator</li>
</ul>

<p>For this article, we will focus on the abstraction features and so <code>serenity_blocks.py</code>, <code>ecdsa_accounts.py</code> and <code>serenity_transactions.py</code> are most critical; for the next article discussing Casper in Serenity, <code>casper.se.py</code> and <code>bet.py</code> will be a primary focus.</p>

<h3>Abstraction and Accounts</h3>

<p>Currently, there are two types of accounts in Ethereum: externally owned accounts, controlled by a private key, and contracts, controlled by code. For externally owned accounts, we specify a particular digital signature algorithm (secp256k1 ECDSA) and a particular sequence number (aka. nonce) scheme, where every transaction must include a sequence number one higher than the previous, in order to prevent replay attacks. The primary change that we will make in order to increase abstraction is this: rather than having these two distinct types of accounts, we will now have only one - contracts. There is also a special "entry point" account, <code>0x0000000000000000000000000000000000000000</code>, that <em>anyone can send from</em> by sending a transaction. Hence, instead of the signature+nonce verification logic of accounts being in the protocol, it is now up to the user to put this into a contract that will be securing their own account.</p>

<p>The simplest kind of contract that is useful is probably the ECDSA verification contract, which simply provides the exact same functionality that is available right now: transactions pass through only if they have valid signatures and sequence numbers, and the sequence number is incremented by 1 if a transaction succeeds. The code for the contract looks as follows:</p>

<pre>
# We assume that data takes the following schema:
# bytes 0-31: v (ECDSA sig)
# bytes 32-63: r (ECDSA sig)
# bytes 64-95: s (ECDSA sig)
# bytes 96-127: sequence number (formerly called "nonce")
# bytes 128-159: gasprice
# bytes 172-191: to
# bytes 192-223: value
# bytes 224+: data

# Get the hash for transaction signing
~mstore(0, ~txexecgas())
~calldatacopy(32, 96, ~calldatasize() - 96)
~mstore(0, ~sha3(0, ~calldatasize() - 64))
~calldatacopy(32, 0, 96)
# Call ECRECOVER contract to get the sender
~call(5000, 1, 0, 0, 128, 0, 32)
# Check sender correctness; exception if not
if ~mload(0) != 0x82a978b3f5962a5b0957d9ee9eef472ee55b42f1:
    ~invalid()
# Sequence number operations
with minusone = ~sub(0, 1):
    with curseq = self.storage[minusone]:
        # Check sequence number correctness, exception if not
        if ~calldataload(96) != curseq:
            ~invalid()
        # Increment sequence number
        self.storage[minusone] = curseq + 1
# Make the sub-call and discard output
with x = ~msize():
    ~call(msg.gas - 50000, ~calldataload(160), ~calldataload(192), 160, ~calldatasize() - 224, x, 1000)
    # Pay for gas
    ~mstore(0, ~calldataload(128))
    ~mstore(32, (~txexecgas() - msg.gas + 50000))
    ~call(12000, ETHER, 0, 0, 64, 0, 0)
    ~return(x, ~msize() - x)
</pre>

<p>This code would sit as the contract code of the user's account; if the user wants to send a transaction, they would send a transaction (from the zero address) to this account, encoding the ECDSA signature, the sequence number, the gasprice, destination address, ether value and the actual transaction data using the encoding specified above in the code. The code checks the signature against the transaction gas limit and the data provided, and then checks the sequence number, and if both are correct it then increments the sequence number, sends the desired message, and then at the end sends a second message to pay for gas (note that miners can statically analyze accounts and refuse to process transactions sending to accounts that do not have gas payment code at the end).</p>

<p>An important consequence of this is that Serenity introduces a model where <em>all transactions (that satisfy basic formatting checks) are valid</em>; transactions that are currently "invalid" will in Serenity simply have no effect (the <code>invalid</code> opcode in the code above simply points to an unused opcode, immediately triggering an exit from code execution). This does mean that transaction inclusion in a block is no longer a guarantee that the transaction was actually executed; to substitute for this, every transaction now gets a receipt entry that specifies whether or not it was successfully executed, providing one of three return codes: <strong>0</strong> (transaction not executed due to block gas limit), <strong>1</strong> (transaction executed but led to error), <strong>2</strong> (transaction executed successfully); more detailed information can be provided if the transaction returns data (which is now auto-logged) or creates its own logs.</p>

<p>The main very large benefit of this is that it gives users much more freedom to innovate in the area of account policy; possible directions include:</p>

<ul>
<li><strong>Bitcoin-style multisig</strong>, where an account expects signatures from multiple public keys at the same time before sending a transaction, rather than accepting signatures one at a time and saving intermediate results in storage</li>
<li><strong>Other elliptic curves</strong>, including ed25519</li>
<li><strong>Better integration for more advanced crypto</strong>, eg. ring signatures, threshold signatures, ZKPs</li>
<li><strong>More advanced sequence number schemes</strong> that allow for higher degrees of parallelization, so that users can send many transactions from one account and have them included more quickly; think a combination of a traditional sequence number and a bitmask. One can also include timestamps or block hashes into the validity check in various clever ways.</li>
<li><strong>UTXO-based token management</strong> - some people dislike the fact that Ethereum uses accounts instead of Bitcoin's "unspent transaction output" (UTXO) model for managing token ownership, in part for privacy reasons. Now, you can create a system inside Ethereum that actually is UTXO-based, and Serenity no longer explicitly "privileges" one over the other.</li>
<li><strong>Innovation in payment schemes</strong> - for some dapps, "contract pays" is a better model than "sender pays" as senders may not have any ether; now, individual dapps can implement such models, and if they are written in a way that miners can statically analyze and determine that they actually will get paid, then they can immediately accept them (essentially, this provides <a href="http://www.rootstock.io/">what Rootstock is trying to do</a> with optional author-pays, but in a much more abstract and flexible way).</li>
<li><strong>Stronger integration for "ethereum alarm clock"-style applications</strong> - the verification code for an account doesn't have to check for signatures, it could also check for Merkle proofs of receipts, state of other accounts, etc</li>
</ul>

<p>In all of these cases, the primary point is that through abstraction all of these other mechanisms become much easier to code as there is no longer a need to create a "pass-through layer" to feed the information in through Ethereum's default signature scheme; when no application is special, every application is.</p>

<p>One particular interesting consequence is that with the current plan for Serenity, Ethereum will be optionally quantum-safe; if you are scared of the NSA having access to a quantum computer, and want to protect your account more securely, you can personally <a href="https://bitcoinmagazine.com/articles/bitcoin-is-not-quantum-safe-and-how-we-can-fix-1375242150">switch to Lamport signatures</a> at any time. Proof of stake further bolsters this, as even if the NSA had a quantum computer and no one else they would not be able to exploit that to implement a 51% attack. The only cryptographic security assumption that will exist at protocol level in Ethereum is collision-resistance of SHA3.</p>

<p>As a result of these changes, transactions are also going to become much simpler. Instead of having nine fields, as is the case right now, transactions will only have four fields: destination address, data, start gas and init code. Destination address, data and start gas are the same as they are now; "init code" is a field that can optionally contain contract creation code for the address that you are sending to.</p>

<p>The reason for the latter mechanic is as follows. One important property that Ethereum currently provides is the ability to send to an account before it exists; you do not need to already have ether in order to create a contract on the blockchain before you can receive ether. To allow this in Serenity, an account's address can be determined from the desired initialization code for the account in advance, by using the formula <code>sha3(creator + initcode) % 2**160</code> where <code>creator</code> is the account that created the contract (the zero account by default), and <code>initcode</code> is the initialization code for the contract (the output of running the initcode will become the contract code, just as is the case for CREATEs right now). You can thus generate the initialization code for your contract locally, compute the address, and let others send to that address. Then, once you want to send your first transaction, you include the init code in the transaction, and the init code will be executed automatically and the account created before proceeding to run the actual transaction (you can find this logic implemented <a href="https://github.com/ethereum/pyethereum/blob/42a448be003e07db18cd94159a61f3a361aff57e/ethereum/serenity_blocks.py#L253">here</a>).</p>

<h3>Abstraction and Blocks</h3>

Another clean separation that will be implemented in Serenity is the complete separation of blocks (which are now simply packages of transactions), state (ie. current contract storage, code and account balances) and the consensus layer. Consensus incentivization is done inside a contract, and consensus-level objects (eg. PoW, bets) should be included as transactions sent to a "consensus incentive manager contract" if one wishes to incentivize them.

This should make it much easier to take the Serenity codebase and swap out Casper for any consensus algorithm - Tendermint, <a href="https://github.com/amiller/HoneyBadgerBFT">HoneyBadgerBFT</a>, <a href="http://arxiv.org/abs/1501.06238">subjective consensus</a> or even plain old proof of work; we welcome research in this direction and aim for maximum flexibility.

<h3>Abstraction and Storage</h3>

<p>Currently, the "state" of the Ethereum system is actually quite complex and includes many parts:</p>

<ul>
<li>Balance, code, nonce and storage of accounts</li>
<li>Gas limit, difficulty, block number, timestamp</li>
<li>The last 256 block hashes</li>
<li>During block execution, the transaction index, receipt tree and the current gas used</li>
</ul>

<p>These data structures exist in various places, including the block state transition function, the state tree, the block header and previous block headers. In Serenity, this will be simplified greatly: although many of these variables will still exist, they will all be moved to specialized contracts in storage; hence, the ONLY concept of "state" that will continue to exist is a tree, which can mathematically be viewed as a mapping <code>{address: {key: value} }</code>. Accounts will simply be trees; account code will be stored at key <code>""</code> for each account (not mutable by <code>SSTORE</code>), balances will be stored in a specialized "ether contract" and sequence numbers will be left up to each account to determine how to store. Receipts will also be moved to storage; they will be stored in a "log contract" where the contents get overwritten every block.</p>

<p>This allows the State object in implementations to be simplified greatly; all that remains is a two-level map of tries. The scalability upgrade may increase this to three levels of tries (shard ID, address, key) but this is not yet determined, and even then the complexity will be substantially smaller than today.</p>

<p>Note that the move of ether into a contract does NOT constitute total ether abstraction; in fact, it is arguably not that large a change from the status quo, as opcodes that deal with ether (the <code>value</code> parameter in <code>CALL</code>, <code>BALANCE</code>, etc) still remain for backward-compatibility purposes. Rather, this is simply a reorganization of how data is stored.</p>

<h3>Future Plans</h3>

<p>For POC2, the plan is to take abstraction even further. Currently, substantial complexity still remains in the block and transaction-level state transition function (eg. updating receipts, gas limits, the transaction index, block number, stateroots); the goal will be to create an "entry point" object for transactions which handles all of this extra "boilerplate logic" that needs to be done per transaction, as well as a "block begins" and "block ends" entry point. A theoretical ultimate goal is to come up with a protocol where there is only one entry point, and the state transition function consists of simply sending a message from the zero address to the entry point containing the block contents as data. The objective here is to reduce the size of the actual <em>consensus-critical client implementation</em> as much as possible, pushing a maximum possible amount of logic directly into Ethereum code itself; this ensures that Ethereum's multi-client model can continue even with an aggressive development regime that is willing to accept hard forks and some degree of new complexity in order to achieve our goals of transaction speed and scalability without requiring an extremely large amount of ongoing development effort and security auditing.</p>

<p>In the longer term, I intend to continue producing proof-of-concepts in python, while the Casper team works together on improving the efficiency and proving the safety and correctness of the protocol; at some point, the protocol will be mature enough to handle a public testnet of some form, possibly (but not certainly) with real value on-chain in order to provide stronger incentives for people to try to "hack" Casper they way that we inevitably expect that they will once the main chain goes live. This is only an initial step, although a very important one as it marks the first time when the research behind proof of stake and abstraction is finally moving from words, math on whiteboards and blog posts into a working implementation written in code.</p>

<p>The next part of this series will discuss the other flagship feature of Serenity, the Casper consensus algorithm.</p>