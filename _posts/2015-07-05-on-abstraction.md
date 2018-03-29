---
id: 1793
title: On Abstraction
date: 2015-07-05T11:06:58+00:00
author: Vitalik Buterin
layout: post
guid: https://blog.ethereum.org/?p=1793
permalink: /2015/07/05/on-abstraction/
dsq_thread_id:
  - "3906068021"
---
<p><i>Special thanks to Gavin Wood, Vlad Zamfir, our security auditors and others for some of the thoughts that led to the conclusions described in this post</i></p>

<p>One of Ethereum's goals from the start, and arguably its entire raison d'être, is the high degree of abstraction that the platform offers. Rather than limiting users to a specific set of transaction types and applications, the platform allows anyone to create any kind of blockchain application by writing a script and uploading it to the Ethereum blockchain. This gives an Ethereum a degree of future-proof-ness and neutrality much greater than that of other blockchain protocols: even if society decides that blockchains aren't really all that useful for finance at all, and are only really interesting for supply chain tracking, self-owning cars and self-refilling dishwashers and playing chess for money in a trust-free form, Ethereum will still be useful. However, there still are a substantial number of ways in which Ethereum is not nearly as abstract as it could be.</p>

<h3>Cryptography</h3>

<p>Currently, Ethereum transactions are all signed using the <a href="https://en.wikipedia.org/wiki/Elliptic_Curve_Digital_Signature_Algorithm">ECDSA algorithm</a>, and specifically Bitcoin's secp256k1 curve. Elliptic curve signatures are a popular kind of signature today, particularly because of the smaller signature and key sizes compared to RSA: an elliptic curve signature takes only 65 bytes, compared to several hundred bytes for an RSA signature. However, it is becoming increasingly understood that the specific kind of signature used by Bitcoin is far from optimal; <a href="http://ed25519.cr.yp.to/ed25519-20110926.pdf">ed25519</a> is increasingly recognized as a superior alternative particularly because of its <a href="https://github.com/vbuterin/ed25519/blob/master/ed25519.py">simpler implementation</a>, greater hardness against side-channel attacks and faster verification. And if quantum computers come around, we will likely have to <a href="https://bitcoinmagazine.com/6021/bitcoin-is-not-quantum-safe-and-how-we-can-fix/">move to Lamport signatures.</a></p>

<p>One suggestion that some of our security auditors, and others, have given us is to allow ed25519 signatures as an option in 1.1. But what if we can stay true to our spirit of abstraction and go a bit further: let people use <i>whatever</i> cryptographic verification algorithm that they want? Is that even possible to do securely? Well, we have the ethereum virtual machine, so we have a way of letting people implement arbitrary cryptographic verification algorithms, but we still need to figure out <em>how</em> it can fit in.</p>

<p>Here is a possible approach:</p>

<ol>
<li>Every account <em>that is not a contract</em> has a piece of "verification code" attached to it.</li>
<li>When a transaction is sent, it must now explicitly specify both sender and recipient.</li>
<li>The first step in processing a transaction is to call the verification code, using the transaction's signature (now a plain byte array) as input. If the verification code outputs anything nonempty within 50000 gas, the transaction is valid. If it outputs an empty array (ie. exactly zero bytes; a single <code>\x00</code> byte does not count) or exits with an exception condition, then it is not valid.</li>
<li>To allow people without ETH to create accounts, we implement a protocol such that one can generate verification code offline and use the hash of the verification code as an address. People can send funds to that address. The first time you send a transaction from that account, you need to provide the verification code in a separate field (we can perhaps overload the nonce for this, since in all cases where this happens the nonce would be zero in any case) and the protocol (i) checks that the verification code is correct, and (ii) swaps it in (this is roughly equivalent to "<a href="https://en.bitcoin.it/wiki/Pay_to_script_hash">pay-to-script-hash</a>" in Bitcoin).</li>
</ol>

<p>This approach has a few benefits. First, it does not specify <em>anything</em> about the cryptographic algorithm used or the signature format, except that it must take up at most 50000 gas (this value can be adjusted up or down over time). Second, it still keeps the property of the existing system that no pre-registration is required. Third, and quite importantly, it allows people to add higher-level validity conditions that depend on state: for example, making transactions that spend more GavCoin than you currently have actually fail instead of just going into the blockchain and having no effect.</p>

<p>However, there are substantial changes to the virtual machine that would have to be made for this to work well. The current virtual machine is designed well for dealing with 256-bit numbers, capturing the hashes and elliptic curve signatures that are used right now, but is suboptimal for algorithms that have different sizes. Additionally, no matter how well-designed the VM is right now, it fundamentally adds a layer of abstraction between the code and the machine. Hence, if this will be one of the uses of the VM going forward, an architecture that maps VM code directly to machine code, applying transformations in the middle to translate specialized opcodes and ensure security, will likely be optimal - particularly for expensive and exotic cryptographic algorithms like zk-SNARKs. And even then, one must take care to minimize any "startup costs" of the virtual machine in order to further increase efficiency as well as denial-of-service vulnerability; together with this, a gas cost rule that encourages re-using existing code and heavily penalizes using different code for every account, allowing just-in-time-compiling virtual machines to maintain a cache, may also be a further improvement.</p>

<h3>The Trie</h3>

<p>Perhaps the most important data structure in Ethereum is the <a href="https://github.com/ethereum/wiki/wiki/Patricia-Tree">Patricia tree</a>. The Patricia tree is a data structure that, like the standard binary Merkle tree, allows any piece of data inside the trie to be securely authenticated against a root hash using a logarithmically sized (ie. relatively short) hash chain, but also has the important property that data can be added, removed or modified in the tree extremely quickly, only making a small number of changes to the entire structure. The trie is used in Ethereum to store transactions, receipts, accounts and particularly importantly the storage of each account.</p>

<p>One of the often cited weaknesses of this approach is that the trie is one particular data structure, optimized for a particular set of use cases, but in many cases accounts will do better with a different model. The most common request is a heap: a data structure to which elements can quickly be added with a priority value, and from which the lowest-priority element can always be quickly removed - particularly useful in implementations of markets with bid/ask offers.</p>

<p>Right now, the only way to do this is a rather inefficient workaround: write an implementation of a heap in Solidity or Serpent <a href="https://github.com/ethereum/serpent/blob/develop/examples/cyberdyne/heap.se">on top of the trie</a>. This essentially means that every update to the heap requires a logarithmic number of updates (eg. at 1000 elements, ten updates, at 1000000 elements, twenty updates) to the trie, and each update to the trie requires changes to a logarithmic number (once again ten at 1000 elements and twenty at 1000000 elements) of items, and each one of those requires a change to the leveldb database which uses a logarithmic-time-updateable trie internally. If contracts had the option to have a heap instead, as a direct protocol feature, then this overhead could be cut down substantially.</p>

<p>One option to solve this problem is the direct one: just have an option for contracts to have either a regular trie or a heap, and be done with it. A seemingly nicer solution, however, is to generalize even further. The solution here is as follows. Rather than having a trie or a treap, we simply have an abstract hash tree: there is a root node, which may be empty or which may be the hash of one or more children, and each child in turn may either be a terminal value or the hash of some set of children of its own. An extension may be to allow nodes to have both a value <em>and</em> children. This would all be encoded in RLP; for example, we may stipulate that all nodes must be of the form:</p>

<pre><code>[val, child1, child2, child3....]
</code></pre>

<p>Where <code>val</code> must be a string of bytes (we can restrict it to 32 if desired), and each child (of which there can be zero or more) must be the 32 byte SHA3 hash of some other node. Now, we have the virtual machine's execution environment keep track of a "current node" pointer, and add a few opcodes:</p>

<ul>
<li><code>GETVAL</code>: pushes the value of the node at the current pointer onto the stack</li>
<li><code>SETVAL</code>: sets the value at the of the node at the current pointer to the value at the top of the stack</li>
<li><code>GETCHILDCOUNT</code>: gets the number of children of the node</li>
<li><code>ADDCHILD</code>: adds a new child node (starting with zero children of its own)</li>
<li><code>REMOVECHILD</code>: pops off a child node</li>
<li><code>DESCEND</code>: descend to the kth child of the current node (taking <code>k</code> as an argument from the stack)</li>
<li><code>ASCEND</code>: ascend to the parent</li>
<li><code>ASCENDROOT</code>: ascend to the root node</li>
</ul>

<p>Accessing a Merkle tree with 128 elements would thus look like this:</p>

<pre><code>def access(i):
    ~ascendroot()
    return _access(i, 7)

def _access(i, depth):
    while depth &gt; 0:
        ~descend(i % 2)   
        i /= 2
        depth -= 1
    return ~getval()
</code></pre>

<p>Creating the tree would look like this:</p>

<pre><code>def create(vals):
    ~ascendroot()
    while ~getchildcount() &gt; 0:
        ~removechild()
    _create(vals, 7)

def _create(vals:arr, depth):
    if depth &gt; 0:
        # Recursively create left child
        ~addchild()
        ~descend(0)
        _create(slice(vals, 0, 2**(depth - 1)), depth - 1)
        ~ascend()
        # Recursively create right child
        ~addchild()
        ~descend(1)
        _create(slice(vals, 2**(depth - 1), 2**depth), depth - 1)
        ~ascend()
    else:
        ~setval(vals[0])
</code></pre>

<p>Clearly, the trie, the treap and in fact <em>any</em> other tree-like data structure could thus be implemented as a library on top of these methods. What is particularly interesting is that each individual opcode is constant-time: theoretically, each node can keep track of the pointers to its children and parent on the database level, requiring only one level of overhead.</p>

<p>However, this approach also comes with flaws. Particularly, note that if we lose control of the structure of the tree, then we lose the ability to make optimizations. Right now, most Ethereum clients, including C++, Go and Python, have a higher-level cache that allows updates to and reads from storage to happen in constant time if there are multiple reads and writes within one transaction execution. If tries become de-standardized, then optimizations like these become impossible. Additionally, each individual trie structure would need to come up with its own gas costs and its own mechanisms for ensuring that the tree cannot be exploited: quite a hard problem, given that even our own trie had a medium level of vulnerability until recently when we replaced the trie keys with the SHA3 hash of the key rather than the actual key. Hence, it's unclear whether going this far is worth it.</p>

<h3>Currency</h3>

<p>It's well-known and established that an open blockchain requires some kind of cryptocurrency in order to incentivize people to participate in the consensus process; this is the kernel of truth behind this otherwise rather silly meme:</p>

<center><img src="https://pbs.twimg.com/media/CCiUG8OWAAABAN-.jpg" /></center>

<p>However, can we create a blockchain that does not rely on any <em>specific</em> currency, instead allowing people to transact using whatever currency they wish? In a proof of work context, particularly a fees-only one, this is actually relatively easy to do for a simple currency blockchain; just have a block size limit and leave it to miners and transaction senders themselves to come to some equilibrium over the transaction price (the transaction fees may well be done as a batch payment via credit card). For Ethereum, however, it is slightly more complicated. The reason is that Ethereum 1.0, as it stands, comes with a built-in gas mechanism which allows miners to safely accept transactions without fear of being hit by denial-of-service attacks; the mechanism works as follows:</p>

<ol>
<li>Every transaction specifies a max gas count and a fee to pay per unit gas.</li>
<li>Suppose that the transaction allows itself a gas limit of N. If the transaction is valid, and takes less than N computational steps (say, M computational steps), then it pays M steps worth of the fee. If the transaction consumes all N computational steps before finishing, the execution is reverted but it still pays N steps worth of the fee.</li>
</ol>

<p>This mechanism relies on the existence of a specific currency, ETH, which is controlled by the protocol. Can we replicate it without relying on any one particular currency? As it turns out, the answer is yes, at least if we combine it with the "use any cryptography you want" scheme above. The approach is as follows. First, we extend the above cryptography-neutrality scheme a bit further: rather than having a separate concept of "verification code" to decide whether or not a particular transaction is valid, simply state that there is only one type of account - a contract, and a transaction is simply a message coming in from the zero address. If the transaction exits with an exceptional condition within 50000 gas, the transaction is invalid; otherwise it is valid and accepted. Within this model, we then set up accounts to have the following code:</p>

<ol>
<li>Check if the transaction is correct. If not, exit. If it is, send some payment for gas to a master contract that will later pay the miner.</li>
<li>Send the actual message.</li>
<li>Send a message to ping the master contract. The master contract then checks how much gas is left, and refunds a fee corresponding to the remaining amount to the sender and sends the rest to the miner.</li>
</ol>

<p>Step 1 can be crafted in a standardized form, so that it clearly consumes less than 50000 gas. Step 3 can similarly be constructed. Step 2 can then have the message provide a gas limit equal to the transaction's specified gas limit minus 100000. Miners can then pattern-match to only accept transactions that are of this standard form (new standard forms can of course be introduced over time), and they can be sure that no single transaction will cheat them out of more than 50000 steps of computational energy. Hence, everything becomes enforced entirely by the gas limit, and miners and transaction senders can use whatever currency they want.</p>

<p>One challenge that arises is: how do you pay contracts? Currently, contracts have the ability to "charge" for services, using code like this registry example:</p>

<pre><code>def reserve(_name:bytes32):
    if msg.value &gt; 100 * 10**18:
        if not self.domains[_name].owner:
            self.domains[_name].owner = msg.sender
</code></pre>

<p>With a sub-currency, there is no such clear mechanism of tying together a message and a payment for that message. However, there are two general patterns that can act as a substitute. The first is a kind of "receipt" interface: when you send a currency payment to someone, you have the ability to ask the contract to store the sender and value of the transaction. Something like <code>registrar.reserve("blahblahblah.eth")</code> would thus be replaced by:</p>

<pre><code>gavcoin.sendWithReceipt(registrar, 100 * 10**18)
registrar.reserve("blahblahblah.eth")
</code></pre>

<p>The currency would have code that looks something like this:</p>

<pre><code>def sendWithReceipt(to, value):
    if self.balances[msg.sender] &gt;= value:
        self.balances[msg.sender] -= value
        self.balances[to] += value
        self.last_sender = msg.sender
        self.last_recipient = to
        self.last_value = value

def getLastReceipt():
    return([self.last_sender, self.last_recipient, self.value]:arr)
</code></pre>

<p>And the registrar would work like this:</p>

<pre><code>def reserve(_name:bytes32):
    r = gavcoin.getLastReceipt(outitems=3)
    if r[0] == msg.sender and r[1] == self and r[2] &gt;= 100 * 10**18:
        if not self.domains[_name].owner:
            self.domains[_name].owner = msg.sender
</code></pre>

<p>Essentially, the registrar would check the last payment made in that currency contract, and make sure that it is a payment to itself. In order to prevent double-use of a payment, it may make sense to have the <code>get_last_receipt</code> method destroy the receipt in the process of reading it.</p>

<p>The other pattern is to have a currency have an interface for allowing another address to make withdrawals from your account. The code would then look as follows on the caller side: first, approve a one-time withdrawal of some number of currency units, then reserve, and the reservation contract attempts to make the withdrawal and only goes forward if the withdrawal succeeds:</p>

<pre><code>gavcoin.approveOnce(registrar, 100)
registrar.reserve("blahblahblah.eth")
</code></pre>

<p>And the registrar would be:</p>

<pre><code>def reserve(_name:bytes32):
    if gavcoin.sendCoinFrom(msg.sender, 100, self) == SUCCESS:
        if not self.domains[_name].owner:
            self.domains[_name].owner = msg.sender
</code></pre>

The second pattern has been standardized at the <a href="https://github.com/ethereum/wiki/wiki/Standardized_Contract_APIs">Standardized Contract APIs wiki page</a>.

<h3>Currency-agnostic Proof of Stake</h3>

<p>The above allows us to create a completely currency-agnostic proof-of-work blockchain. However, to what extent can currency-agnosticism be added to proof of stake? Currency-agnostic proof of stake is useful for two reasons. First, it creates a stronger impression of economic neutrality, which makes it more likely to be accepted by existing established groups as it would not be seen as favoring a particular specialized elite (bitcoin holders, ether holders, etc). Second, it increases the amount that will be deposited, as individuals holding digital assets other than ether would have a very low personal cost in putting some of those assets into a deposit contract. At first glance, it seems like a hard problem: unlike proof of work, which is fundamentally based on an external and neutral resource, proof of stake is intrinsically based on some kind of currency. So how far can we go?</p>

<p>The first step is to try to create a proof of stake system that works using any currency, using some kind of standardized currency interface. The idea is simple: anyone would be able to participate in the system by putting up any currency as a security deposit. Some market mechanism would then be used in order to determine the value of each currency, so as to estimate the amount of each currency that would need to be put up in order to obtain a stake depositing slot. A simple first approximation would be to maintain an on-chain decentralized exchange and read price feeds; however, this ignores liquidity and sockpuppet issues (eg. it's easy to create a currency and spread it across a small group of accounts and pretend that it has a value of $1 trillion per unit); hence, a more coarse-grained and direct mechanism is required.</p>

<p>To get an idea of what we are looking for, consider David Friedman's <a href="http://daviddfriedman.com/Academic/Course_Pages/Legal_Systems_Very_Different_13/Book_Draft/Systems/AthenianChapter.html">description of one particular aspect</a> of the ancient Athenian legal system:</p>

<blockquote>
  <p>The Athenians had a straightforward solution to the problem of producing public goods such as the maintainance of a warship or the organizing of a public festival. If you were one of the richest Athenians, every two years you were obligated to produce a public good; the relevant magistrate would tell you which one.
"As you doubtless know, we are sending a team to the Olympics this year. Congratulations, you are the sponsor."
Or
"Look at that lovely trireme down at the dock. This year guess who gets to be captain and paymaster."
Such an obligation was called a liturgy. There were two ways to get out of it. One was to show that you were already doing another liturgy this year or had done one last year. The other was to prove that there was another Athenian, richer than you, who had not done one last year and was not doing one this year.
This raises an obvious puzzle. How, in a world without accountants, income tax, public records of what people owned and what it was worth, do I prove that you are richer than I am? The answer is not an accountant’s answer but an economist’s—feel free to spend a few minutes trying to figure it out before you turn the page.
The solution was simple. I offer to exchange everything I own for everything you own. If you refuse, you have admitted that you are richer than I am, and so you get to do the liturgy that was to be imposed on me.</p>
</blockquote>

<p>Here, we have a rather nifty scheme for preventing people that are rich from pretending that they are poor. Now, however, what we are looking for is a scheme for preventing people that are poor from pretending that they are rich (or more precisely, preventing people that are releasing small amounts of value into the proof of stake security deposit scheme from pretending that they are staking a much larger amount).</p>

<p>A simple approach would be a swapping scheme like that, but done in reverse via a voting mechanic: in order to join the stakeholder pool, you would need to be approved by 33% of the existing stakeholders, but every stakeholder that approves you would have to face the condition that you can exchange your stake for theirs: a condition that they would not be willing to meet if they thought it likely that the value of your stake actually would drop. Stakeholders would then charge an insurance fee for signing stake that is likely to strongly drop against the existing currencies that are used in the stake pool.</p>

<p>This scheme as described above has two substantial flaws. First, it naturally leads to currency centralization, as if one currency is dominant it will be most convenient and safe to also stake in that currency. If there are two assets, A and B, the process of joining using currency A, in this scheme, implies receiving an option (in the <a href="https://en.wikipedia.org/wiki/Option_%28finance%29">financial sense</a> of the term) to purchase B at the exchange rate of A:B at the price at the time of joining, and this option would thus naturally have a cost (which can be estimated via the <a href="http://en.wikipedia.org/wiki/Black%E2%80%93Scholes_model">Black-Scholes model</a>). Just joining with currency A would be simpler. However, this can be remedied by asking stakeholders to continually vote on the price of all currencies and assets used in the stake pool - an incentivized vote, as the vote reflects both the weight of the asset from the point of view of the system and the exchange rate at which the assets can be forcibly exchanged.</p>

<p>A second, more serious flaw, however, is the possibility of pathological metacoins. For example, one can imagine a currency which is backed by gold, but which has the additional rule, imposd by the institution backing it, that forcible transfers initiated by the protocol "do not count"; that is, if such a transfer takes place, the allocation <em>before</em> the transfer is frozen and a new currency is created using that allocation as its starting point. The old currency is no longer backed by gold, and the new one is. Athenian forcible-exchange protocols can get you far when you can actually forcibly exchange property, but when one can deliberately create pathological assets that arbitrarily circumvent specific transaction types it gets quite a bit harder.</p>

<p>Theoretically, the voting mechanism can of course get around this problem: nodes can simply refuse to induct currencies that they know are suspicious, and the default strategy can tend toward conservatism, accepting a very small number of currencies and assets only. Altogether, we leave currency-agnostic proof of stake as an open problem; it remains to be seen exactly how far it can go, and the end result may well be some quasi-subjective combination of TrustDavis and Ripple consensus.</p>

<h3>SHA3 and RLP</h3>

<p>Now, we get to the last few parts of the protocol that we have not yet taken apart: the hash algorithm and the serialization algorithm. Here, unfortunately, abstracting things away is much harder, and it is also much harder to tell what the value is. First of all, it is important to note that even though we have shows how we could conceivably abstract away the trees that are used for account storage, it is much harder to see how we could abstract away the trie on the top level that keeps track of the accounts themselves. This tree is necessarily system-wide, and so one can't simply say that different users will have different versions of it. The top-level trie relies on SHA3, so some kind of specific hashing algorithm there must stay. Even the bottom-level data structures will likely have to stay SHA3, since otherwise there would be a risk of a hash function being used that is not collision-resistant, making the whole thing no longer strongly cryptographically authenticated and perhaps leading to forks between full clients and light clients.</p>

<p>RLP is similarly unavoiable; at the very least, each account needs to have code and storage, and the two need to be stored together some how, and that is already a serialization format. Fortunately, however, SHA3 and RLP are perhaps the most well-tested, future-proof and robust parts of the protocol, so the benefit from switching to something else is quite small.</p>
