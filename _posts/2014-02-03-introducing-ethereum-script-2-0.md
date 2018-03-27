---
id: 64
title: Introducing Ethereum Script 2.0
date: 2014-02-03T10:00:19+00:00
author: Vitalik Buterin
layout: post
guid: http://blog.ethereum.org/?p=64
permalink: /2014/02/03/introducing-ethereum-script-2-0/
dsq_thread_id:
  - "2790049990"
---
This post will provide the groundwork for a major rework of the Ethereum scripting language, which will substantially modify the way ES works although still keeping many of the core components working in the exact same way. The rework is necessary as a result of multiple concerns which have been raised about the way the language is currently designed, primarily in the areas of simplicity, optimization, efficiency and future-compatibility, although it does also have some side-benefits such as improved function support. This is not the last iteration of ES2; there will likely be many incremental structural improvements that can be made to the spec, but it does serve as a strong starting point.

As an important clarification, this rework will have little effect on the Ethereum CLL, the stripped-down-Python-like language in which you can write Namecoin in five lines of code. The CLL will still stay the same as it is now. We will need to make updates to the compiler (an alpha version of which is now available in Python at <a href="http://github.com/ethereum/compiler">http://github.com/ethereum/compiler</a> or as a friendly web interface at <a href="http://162.218.208.138:3000/">http://162.218.208.138:3000</a>) in order to make sure the CLL continues to compile to new versions of ES, but you as an Ethereum contract developer working in E-CLL should not need to see any changes at all.

<h3>Problems with ES1</h3>

Over the last month of working with ES1, several problems with the language’s design have become apparent. In no particular order, they are as follows:

<ul>
    <li><b>Too many opcodes</b> – looking at the specification <a href="http://web.archive.org/web/20140203145850/http://wiki.ethereum.org/index.php/Ethereum_Script">as it appears today</a>, ES1 now has exactly 50 opcodes – less than the 80 opcodes found in <a href="https://en.bitcoin.it/wiki/Script">Bitcoin Script</a>, but still far more than the theoretically minimal 4-7 opcodes needed to have a functional Turing-complete scripting language. Some of those opcodes are necessary because we want the scripting language to have access to a lot of data – for example, the transaction value, the transaction source, the transaction data, the previous block hash, etc; like it or not, there needs to be a certain degree of complexity in the language definition to provide all of these hooks. Other opcodes, however, are excessive, and complex; as an example, consider the current definition of SHA256 or ECVERIFY. With the way the language is designed right now, that is necessary for efficiency; otherwise, one would have to write SHA256 in Ethereum script by hand, which might take many thousands of BASEFEEs. But ideally, there should be some way of eliminating much of the bloat.</li>
    <li><b>Not future-compatible</b> – the existence of the special crypto opcodes does make ES1 much more efficient for certain specialized applications; thanks to them, computing SHA3 takes only 40x BASEFEE instead of the many thousands of basefees that it would take if SHA3 was implemented in ES directly; same with SHA256, RIPEMD160 and secp256k1 elliptic curve operations. However, it is absolutely not future-compatible. Even though these existing crypto operations will only take 40x BASEFEE, SHA4 will take several thousand BASEFEEs, as will ed25519 signatures, the quantum-proof<a href="http://en.wikipedia.org/wiki/NTRU">NTRU</a>, SCIP and Zerocoin math, and any other constructs that will appear over the coming years. There should be some natural mechanism for folding such innovations in over time.</li>
    <li><b>Not deduplication-friendly</b> – the Ethereum blockchain is likely to become extremely bloated over time, especially with every contract writing its own code even when the bulk of the code will likely be thousands of people trying to do the exact same thing. Ideally, all instances where code is written twice should pass through some process of deduplication, where the code is only stored once and only a pointer to the code is stored twice. In theory, Ethereum’s Patricia trees do this already. In practice, however, code needs to be in exactly the same place in order for this to happen, and the existence of jumps means that it is often difficult to abitrarily copy/paste code without making appropriate modifications. Furthermore, there is no incentivization mechanism to convince people to reuse existing code.</li>
    <li><b>Not optimization-friendly</b> – this is a very similar criterion to future-compatibility and deduplication-friendliness in some ways. However, here optimization refers to a more automatic process of detecting bits of code that are reused many times, and replacing them with <a href="http://en.wikipedia.org/wiki/Memoization">memoized</a> or compiled machine code versions.</li>
</ul>

<h3>Beginnings of a Solution: Deduplication</h3>

The first issue that we can handle is that of deduplication. As described above, Ethereum Patricia trees provide deduplication already, but the problem is that achieving the full benefits of the deduplication requires the code to be formatted in a very special way. For example, if the code in contract A from index 0 to index 15 is the same as the code in contract B from index 48 to index 63, then deduplication happens. However, if the code in contract B is offset at all modulo 16 (eg. from index 49 to index 64), then no deduplication takes place at all. In order to remedy this, there is one relatively simple solution: move from a dumb hexary Patricia tree to a more semantically oriented data structure. That is, the tree represented in the database should mirror the abstract syntax tree of the code.

To understand what I am saying here, consider some existing ES1 code:

TXVALUE PUSH 25 PUSH 10 PUSH 18 EXP MUL LT NOT PUSH 14 JMPI STOP PUSH 0 TXDATA SLOAD NOT PUSH 0 TXDATA PUSH 1000 LT NOT MUL NOT NOT PUSH 32 JMPI STOP PUSH 1 TXDATA PUSH 0 TXDATA SSTORE

In the Patricia tree, it looks like this:

(
(TXVALUE PUSH 25 PUSH 10 PUSH 18 EXP MUL LT NOT PUSH 14 JMPI STOP PUSH)
(0 TXDATA SLOAD NOT PUSH 0 TXDATA PUSH 1000 LT NOT MUL NOT NOT PUSH 32)
(JMPI STOP PUSH 1 TXDATA PUSH 0 TXDATA SSTORE)
)

And here is what the code looks like structurally. This is easiest to show by simply giving the E-CLL it was compiled from:

if tx.value &lt; 25 * 10^18:
stop
if contract.storage[tx.data[0]] or tx.data[0] &lt; 1000:
stop
contract.storage[tx.data[0]] = tx.data[1]

No relation at all. Thus, if another contract wanted to use some semantic sub-component of this code, it would almost certainly have to re-implement the whole thing. However, if the tree structure looked somewhat more like this:

(
(
IF
(TXVALUE PUSH 25 PUSH 10 PUSH 18 EXP MUL LT NOT)
(STOP)
)
(
IF
(PUSH 0 TXDATA SLOAD NOT PUSH 0 TXDATA PUSH 1000 LT NOT MUL NOT)
(STOP)
)
( PUSH 1 TXDATA PUSH 0 TXDATA SSTORE )
)

Then if someone wanted to reuse some particular piece of code they easily could. Note that this is just an illustrative example; in this particular case it probably does not make sense to deduplicate since pointers need to be at least 20 bytes long to be cryptographically secure, but in the case of larger scripts where an inner clause might contain a few thousand opcodes it makes perfect sense.

<h3>Immutability and Purely Functional Code</h3>

Another modification is that code should be immutable, and thus separate from data; if multiple contracts rely on the same code, the contract that originally controls that code should not have the ability to sneak in changes later on. The pointer to which code a running contract should start with, however, should be mutable.

A third common optimization-friendly technique is the make a programming language purely functional, so functions cannot have any side effects outside of themselves with the exception of return values. For example, the following is a pure function:

def factorial(n):
prod = 1
for i in range(1,n+1):
prod *= i
return prod

However, this is not:

x = 0
def next_integer():
x += 1
return x

And this most certainly is not:

import os
def happy_fluffy_function():
bal = float(os.popen('bitcoind getbalance').read())
os.popen('bitcoind sendtoaddress 1JwSSubhmg6iPtRjtyqhUYYH7bZg3Lfy1T %.8f' % (bal - 0.0001))
os.popen('rm -rf ~')

Ethereum cannot be purely functional, since Ethereum contracts do necessarily have state – a contract can modify its long-term storage and it can send transactions. However, Ethereum script is a unique situation because Ethereum is not just a scripting environment – it is an incentivized scripting environment. Thus, we can allow applications like modifying storage and sending transactions, but discourage them with fees, and thus ensure that most script components are purely functional simply to cut costs, even while allowing non-purity in those situations where it makes sense.

What is interesting is that these two changes work together. The immutability of code also makes it easier to construct a restricted subset of the scripting language which is functional, and then such functional code could be deduplicated and optimized at will.

<h3>Ethereum Script 2.0</h3>

So, what’s going to change? First of all, the basic stack-machine concept is going to roughly stay the same. The main data structure of the system will continue to be the stack, and most of your beloved opcodes will not change significantly. The only differences in the stack machine are the following:

<ol>
    <li>Crypto opcodes are removed. Instead, we will have to have someone write SHA256, RIPEMD160, SHA3 and ECC in ES as a formality, and we can have our interpreters include an optimization replacing it with good old-fashioned machine-code hashes and sigs right from the start.</li>
    <li>Memory is removed. Instead, we are bringing back DUPN (grabs the next value in the code, say N, and pushes a copy of the item N items down the stack to the top of the stack) and SWAPN (swaps the top item and the nth item).</li>
    <li>JMP and JMPI are removed.</li>
    <li>RUN, IF, WHILE and SETROOT are added (see below for further definition)</li>
</ol>

Another change is in how transactions are serialized. Now, transactions appear as follows:

<ul>
    <li><b>SEND</b>: [ 0, nonce, to, value, [ data0 ... datan ], v, r, s ]</li>
    <li><b>MKCODE</b>: [ 1, nonce, [ data0 ... datan ], v, r, s ]</li>
    <li><b>MKCONTRACT</b>: [ 2, nonce, coderoot, v, r, s ]</li>
</ul>

The address of a contract is defined by the last 20 bytes of the hash of the transaction that produced it, as before. Additionally, the nonce no longer needs to be equal to the nonce stored in the account balance representation; it only needs to be equal to or greater than that value.

Now, suppose that you wanted to make a simple contract that just keeps track of how much ether it received from various addresses. In E-CLL that’s:

contract.storage[tx.sender] = tx.value

In ES2, instantiating this contract now takes two transactions:

[ 1, 0, [ TXVALUE TXSENDER SSTORE ], v, r, s]

[ 2, 1, 761fd7f977e42780e893ea44484c4b64492d8383, v, r, s ]

What happens here is that the first transaction instantiates a code node in the Patricia tree. The hash sha3(rlp.encode([ TXVALUE TXSENDER SSTORE ]))[12:] is 761fd7f977e42780e893ea44484c4b64492d8383, so that is the “address” where the code node is stored. The second transaction basically says to initialize a contract whose code is located at that code node. Thus, when a transaction gets sent to the contract, that is the code that will run.

Now, we come to the interesting part: the definitions of IF and RUN. The explanation is simple: IF loads the next two values in the code, then pops the top item from the stack. If the top item is nonzero, then it runs the code item at the first code value. Otherwise, it runs the code item at the second code value. WHILE is similar, but instead loads only one code value and keeps running the code while the top item on the stack is nonzero. Finally, RUN just takes one code value and runs the code without asking for anything. And that’s all you need to know. Here is one way to do a Namecoin contract in new Ethereum script:

A: [ TXVALUE PUSH 25 PUSH 10 PUSH 18 EXP MUL LT ]
B: [ PUSH 0 TXDATA SLOAD NOT PUSH 0 TXDATA PUSH 100 LT NOT MUL NOT ]
Z: [ STOP ]
Y: [ ]
C: [ PUSH 1 TXDATA PUSH 0 TXDATA SSTORE ]
M: [ RUN A IF Z Y RUN B IF Z Y RUN C ]

The contract would then have its root be M. But wait, you might say, this makes the interpreter recursive. As it turns out, however, it does not – you can simulate the recursion using a data structure called a “continuation stack”. Here’s what the full stack trace of that code might look like, assuming the transaction is [ X, Y ] sending V where X &gt; 100, V &gt; 10^18 * 25and contract.storage[X] is not set:

{ stack: [], cstack: [[M, 0]], op: RUN }
{ stack: [], cstack: [[M, 2], [A, 0]], op: TXVALUE }
{ stack: [V], cstack: [[M, 2], [A, 1]], op: PUSH }
{ stack: [V, 25], cstack: [[M, 2], [A, 3]], op: PUSH }
{ stack: [V, 25, 10], cstack: [[M, 2], [A, 5]], op: PUSH }
{ stack: [V, 25, 10, 18], cstack: [[M, 2], [A, 7]], op: EXP }
{ stack: [V, 25, 10^18], cstack: [[M, 2], [A, 8]], op: MUL }
{ stack: [V, 25*10^18], cstack: [[M, 2], [A, 9]], op: LT }
{ stack: [0], cstack: [[M, 2], [A, 10]], op: NULL }
{ stack: [0], cstack: [[M, 2]], op: IF }
{ stack: [0], cstack: [[M, 5], [Y, 0]], op: NULL }

{ stack: [0], cstack: [[M, 5]], op: RUN }
{ stack: [], cstack: [[M, 7], [B, 0]], op: PUSH }
{ stack: [0], cstack: [[M, 7], [B, 2]], op: TXDATA }
{ stack: [X], cstack: [[M, 7], [B, 3]], op: SLOAD }
{ stack: [0], cstack: [[M, 7], [B, 4]], op: NOT }
{ stack: [1], cstack: [[M, 7], [B, 5]], op: PUSH }
{ stack: [1, 0], cstack: [[M, 7], [B, 7]], op: TXDATA }
{ stack: [1, X], cstack: [[M, 7], [B, 8]], op: PUSH }
{ stack: [1, X, 100], cstack: [[M, 7], [B, 10]], op: LT }
{ stack: [1, 0], cstack: [[M, 7], [B, 11]], op: NOT }
{ stack: [1, 1], cstack: [[M, 7], [B, 12]], op: MUL }
{ stack: [1], cstack: [[M, 7], [B, 13]], op: NOT }
{ stack: [1], cstack: [[M, 7], [B, 14]], op: NULL }
{ stack: [0], cstack: [[M, 7]], op: IF }
{ stack: [0], cstack: [[M, 9], [Y, 0]], op: NULL }

{ stack: [], cstack: [[M, 10]], op: RUN }
{ stack: [], cstack: [[M, 12], [C, 0]], op: PUSH }
{ stack: [1], cstack: [[M, 12], [C, 2]], op: TXDATA }
{ stack: [Y], cstack: [[M, 12], [C, 3]], op: PUSH }
{ stack: [Y,0], cstack: [[M, 12], [C, 5]], op: TXDATA }
{ stack: [Y,X], cstack: [[M, 12], [C, 6]], op: SSTORE }
{ stack: [], cstack: [[M, 12], [C, 7]], op: NULL }
{ stack: [], cstack: [[M, 12]], op: NULL }
{ stack: [], cstack: [], op: NULL }

And that’s all there is to it. Cumbersome to read, but actually quite easy to implement in any statically or dynamically types programming language or perhaps even ultimately in an ASIC.

<h3>Optimizations</h3>

In the above design, there is still one major area where optimizations can be made: making the references compact. What the clear and simple style of the above contract hid is that those pointers to A, B, C, M and Z aren’t just compact single letters; they are 20-byte hashes. From an efficiency standpoint, what we just did is thus actually substantially worse than what we had before, at least from the point of view of special cases where code is not nearly-duplicated millions of times. Also, there is still no incentive for people writing contracts to write their code in such a way that other programmers later on can optimize; if I wanted to code the above in a way that would minimize fees, I would just put A, B and C into the contract directly rather than separating them out into functions. There are two possible solutions:

<ol>
    <li>Instead of using H(x) = SHA3(rlp.encode(x))[12:], use H(x) = SHA3(rlp.encode(x))[12:] if len(rlp.encode(x)) &gt;= 20 else x. To summarize, if something is less than 20 bytes long, we include it directly.</li>
    <li>A concept of “libraries”. The idea behind libraries is that a group of a few scripts can be published together, in a format [ [ ... code ... ], [ ... code ... ], ... ], and these scripts can internally refer to each other with their indices in the list alone. This completely alleviates the problem, but at some cost of harming deduplication, since sub-codes may need to be stored twice. Some intelligent thought into exactly how to improve on this concept to provide both deduplication and reference efficiency will be required; perhaps one solution would be for the library to store a list of hashes, and then for the continuation stack to store [ lib, libIndex, codeIndex ] instead of [ hash, index ].</li>
</ol>

Other optimizations are likely possible. For example, one important weakness of the design described above is that it does not support recursion, offering only while loops to provide Turing-completeness. It might seem to, since you can call any function, but if you try to actually try to implement recursion in ES2 as described above you soon notice that implementing recursion would require finding the fixed point of an iterated hash (ie. finding x such that H(a + H( c + ... H(x) ... + d) + b) = x), a problem which is generally assumed to be cryptographically impossible. The “library” concept described above does actually fix this at least internally to one library; ideally, a more perfect solution would exist, although it is not necessary. Finally, some research should go into the question of making functions first-class; this basically means changing the IF and RUNopcode to pull the destination from the stack rather than from fixed code. This may be a major usability improvement, since you can then code higher-order functions that take functions as arguments like map, but it may also be harmful from an optimization standpoint since code becomes harder to analyze and determine whether or not a given computation is purely functional.

<h3>Fees</h3>

Finally, there is one last question to be resolved. The primary purposes of ES2 as described above are twofold: deduplication and optimization. However, optimizations by themselves are not enough; in order for people to actually benefit from the optimizations, and to be incentivized to code in patterns that are optimization-friendly, we need to have a fee structure that supports this. From a deduplication perspective, we already have this; if you are the second person to create a Namecoin-like contract, and you want to use A, you can just link to A without paying the fee to instantiate it yourself. However, from an optimization perspective, we are far from done. If we create SHA3 in ES, and then have the interpreter intelligently replace it with a contract, then the interpreter does get much faster, but the person using SHA3 still needs to pay thousands of BASEFEEs. Thus, we need a mechanism for reducing the fee of specific computations that have been heavily optimized.

Our current <a href="http://blog.ethereum.org/?p=119/on-transaction-fees-market-based-solutions">strategy with fees</a> is to have miners or ether holders vote on the basefee, and in theory this system can easily be expanded to include the option to vote on reduced fees for specific scripts. However, this does need to be done intelligently. For example, EXP can be replaced with a contract of the following form:

PUSH 1 SWAPN 3 SWAP WHILE ( DUP PUSH 2 MOD IF ( DUPN 2 ) ( PUSH 1 ) DUPN 4 MUL SWAPN 4 POP 2 DIV SWAP DUP MUL SWAP ) POP

However, the runtime of this contract depends on the exponent – with an exponent in the range [4,7] the while loop runs three times, in the range [1024, 2047] the while loop runs eleven times, and in the range [2^255, 2^256-1] it runs 256 times. Thus, it would be highly dangerous to have a mechanism which can be used to simply set a fixed fee for any contract, since that can be exploited to, say, impose a fixed fee for a contract computing the <a href="http://en.wikipedia.org/wiki/Ackermann_function">Ackermann function</a> (a function notorious in the world of mathematics because the cost of computing or writing down its output grows so fast that with inputs as low as 5 it becomes larger than the size of the universe). Thus, a percentage discount system, where some contracts can enjoy half as large a basefee, may make more sense. Ultimately, however, a contract cannot be optimized down to below the cost of calling the optimized code, so we may want to have a fixed fee component. A compromise approach might be to have a discount system, but combined with a rule that no contract can have its fee reduced below 20x the BASEFEE.

So how would fee voting work? One approach would be to store the discount of a code item along side that code item’s code, as a number from 1 to 232, where 232 represents no discount at all and 1 represents the highest discounting level of 4294967296x (it may be prudent to set the maximum at 65536x instead for safety). Miners would be authorized to make special “discount transactions” changing the discounting number of any code item by a maximum of 1/65536x of its previous value. With such a system, it would take about 40000 blocks or about one month to halve the fee of any given script, a sufficient level of friction to prevent mining attacks and give everyone a chance to upgrade to new clients with more advanced optimizers while still making it possible to update fees as required to ensure future-compatibility.

Note that the above description is not clean, and is still very much not fleshed out; a lot of care will need to be made in making it maximally elegant and easy to implement. An important point is that optimizers will likely end up replacing entire swaths of ES2 code blocks with more efficient machine code, but under the system described above will still need to pay attention to ES2 code blocks in order to determine what the fee is. One solution is to have a miner policy offering discounts only to contracts which maintain exactly the same fee when run regardless of their input; perhaps other solutions exist as well. However, one thing is clear: the problem is not an easy one.