---
id: 44
title: 'The Latest EVM: “Ethereum Is A Trust-Free Closure System”'
date: 2014-03-20T10:04:28+00:00
author: Vitalik Buterin
layout: post
guid: http://blog.ethereum.org/?p=44
permalink: /2014/03/20/the-latest-evm-ethereum-is-a-trust-free-closure-system/
dsq_thread_id:
  - "2790050075"
---
In the past two weeks our lead C++ developer, Gavin Wood, and myself have been spending a lot of time meeting the local Ethereum community in San Francisco and Silicon Valley. We were very excited to see such a large amount of interest in our project, and the fact that after only two months we have a meetup group that comes together every week, just like the Bitcoin meetup, with over thirty people attending each time. People in the community are taking it upon themselves to make educational videos, organize events and experiment with contracts, and one person is even independently starting to write an implementation of Ethereum in node.js. At the same time, however, we had the chance to take another look at the Ethereum protocols, see where things are still imperfect, and agree on a large array of changes that will be integrated, likely with only minimal modification, into the PoC 3.5 clients.
<h3>Transactions as Closures</h3>
In ES1 and ES2, the MKTX opcode, which allowed contracts to send transactions triggering other contracts, had one very non-intuitive feature: although one would naturally expect MKTX to be like a function call, processing the entire transaction immediately and then continuing on with the rest of the code, in reality MKTX did not work this way. Instead, the execution of the call is deferred toward the end – when MKTX was called, a new transaction would be pushed to the front of the transaction stack of the block, and when the execution of the first transaction ends the execution of the second transaction begins. For example, this is something that you might expect to work:

x = array()
x[0] = "george"
x[1] = MYPUBKEY

mktx(NAMECOIN,10^20,x,2)

if contract.storage(NAMECOIN)["george"] == MYPUBKEY:
registration_successful = 1
else:
registration_successful = 0

// do more stuff...

Use the namecoin contract to try to register “george”, then use the EXTRO opcode to see if the registration is successful. This seems like it should work. However, of course, it doesn’t.

In EVM3 (no longer ES3), we fix this problem. We do this by taking an idea from ES2 – creating a concept of reusable code, functions and software libraries, and an idea from ES1 – keeping it simple by keeping code as a sequential set of instructions in the state, and merging the two together into a concept of “message calls”. A message call is an operation executed from inside a contract which takes a destination address, an ether value, and some data as input and calls the contract with that ether value and data, but which also, unlike a transaction, returns data as an output. There is thus also a new RETURN opcode which allows contract execution to return data.

With this system, contracts can now be much more powerful. Contracts of the traditional sort, performing certain data upon receiving message calls, can still exist. But now, however, two other design patterns also become possible. First, one can now create a proprietary data feed contract; for example, Bloomberg can publish a contract into which they push various asset prices and other market data, and include in its contract an API that returns the internal data as long as the incoming message call sends at least 1 finney along with it. The fee can’t go too high; otherwise contracts that fetch data from the Bloomberg contract once per block and then provide a cheaper passthrough will be profitable. However, even with fees equal to the value of perhaps a quarter of a transaction fee, such a data-feeding business may end up being very viable. The EXTRO opcode is removed to facilitate this functionality, ie. contracts are now opaque from inside the system, although from the outside one can obviously simply look at the Merkle tree.

Second, it is possible to create contracts that represent functions; for example, one can have a SHA256 contract or an ECMUL contract to compute those respective functions. There is one problem with this: twenty bytes to store the address to call a particular function might be a bit much. However, this can be solved by creating one “stdlib” contract which contains a few hundred clauses for common functions, and contracts can store the address of this contract once as a variable and then access it many times simply as “x” (technically, “PUSH 0 MLOAD”). This is the EVM3 way of integrating the other major idea from ES2, the concept of standard libraries.
<h3>Ether and Gas</h3>
Another important change is this: contracts no longer pay for contract execution, transactions do. When you send a transaction, you now need to include a BASEFEE and a maximum number of steps that you’re willing to pay for. At the start of transaction execution, the BASEFEE multiplied by the maxsteps is immediately subtracted from your balance. A new counter is then instantiated, called GAS, that starts off with the number of steps that you have left. Then, transaction execution starts as before. Every step costs 1 GAS, and execution continues until either it naturally halts, at which point all remaining gas times the provided BASEFEE is returned to the sender, or the execution runs out of GAS; in that case, all execution is reverted but the entire fee is still paid.

This approach has two important benefits. First, it allows miners to know ahead of time the maximum quantity of GAS that a transaction will consume. Second, and much more importantly, it allows contract writers to spend much less time focusing on making the contract “defensible” against dummy transactions that try to sabotage the contract by forcing it to pay fees. For example, consider the old 5-line Namecoin:

if tx.value &lt; block.basefee * 200:
stop
if !contract.storage[tx.data[0]] or tx.data[0] = 100:
contract.storage[tx.data[0]] = tx.data[1]

Two lines, no checks. Much simpler. Focus on the logic, not the protocol details. The main weakness of the approach is that it means that, if you send a transaction to a contract, you need to precalculate how long the execution will take (or at least set a reasonable upper bound you’re willing to pay), and the contract has the power to get into an infinite loop, use up all the gas, and force you to pay your fee with no effect. However, this is arguably a non-issue; when you send a transaction to someone, you are already implicitly trusting them not to throw the money into a ditch (or at least not complain if they do), and it’s up to the contract to be reasonable. Contracts may even choose to include a flag stating how much gas they expect to require (I hereby nominate prepending “PUSH 4 JMP ” to execution code as a voluntary standard)

There is one important extension to this idea, which applies to the concept of message calls: when a contract makes a message call, the contract also specifies the amount of gas that the contract on the other end of the call has to use. Just as at the top level, the receiving contract can either finish execution in time or it can run out of gas, at which point execution reverts to the start of the call but the gas is still consumed. Alternatively, contracts can put a zero in the gas fields; in that case, they are trusting the sub-contract with all remaining gas. The main reason why this is necessary is to allow automatic contracts and human-controlled contracts to interact with each other; if only the option of calling a contract with all remaining gas was available, then automatic contracts would not be able to use any human-controlled contracts without absolutely trusting their owners. This would make m-of-n data feed applications essentially nonviable. On the other hand, this does introduce the weakness that the execution engine will need to include the ability to revert to certain previous points (specifically, the start of a message call).
<h3>The New Terminology Guide</h3>
With all of the new concepts that we have introduced, we have standardized on a few new terms that we will use; hopefully, this will help clear up discussion on the various topics.
<ul>
	<li><b>External Actor</b>: A person or other entity able to interface to an Ethereum node, but external to the world of Ethereum. It can interact with Ethereum through depositing signed Transactions and inspecting the block-chain and associated state. Has one (or more) intrinsic Accounts.</li>
	<li><b>Address</b>: A 160-bit code used for identifying Accounts.</li>
	<li><b>Account</b>: Accounts have an intrinsic balance and transaction count maintained as part of the Ethereum state. They are owned either by External Actors or intrinsically (as an indentity) an Autonomous Object within Ethereum. If an Account identifies an Autonomous Object, then Ethereum will also maintain a Storage State particular to that Account. Each Account has a single Address that identifies it.</li>
	<li><b>Transaction</b>: A piece of data, signed by an External Actor. It represents either a Message or a new Autonomous Object. Transactions are recorded into each block of the block-chain.</li>
	<li><b>Autonomous Object</b>: A virtual object existant only within the hypothetical state of Ethereum. Has an intrinsic address. Incorporated only as the state of the storage component of the VM.</li>
	<li><b>Storage State</b>: The information particular to a given Autonomous Object that is maintained between the times that it runs.</li>
	<li><b>Message</b>: Data (as a set of bytes) and Value (specified as Ether) that is passed between two Accounts in a perfectly trusted way, either through the deterministic operation of an Autonomous Object or the cryptographically secure signature of the Transaction.</li>
	<li><b>Message Call</b>: The act of passing a message from one Account to another. If the destination account is an Autonomous Object, then the VM will be started with the state of said Object and the Message acted upon. If the message sender is an Autonomous Object, then the Call passes any data returned from the VM operation.</li>
	<li><b>Gas</b>: The fundamental network cost unit. Paid for exclusively by Ether (as of PoC-3.5), which is converted freely to and from Gas as required. Gas does not exist outside of the internal Ethereum computation engine; its price is set by the Transaction and miners are free to ignore Transactions whose Gas price is too low.</li>
</ul>
<h3>Long Term View</h3>
Soon, we will release a full formal spec of the above changes, including a new version of the whitepaper that takes into account all of these modifications, as well as a new version of the client that implements it. Later on, further changes to the EVM will likely be made, but the ETH-HLL will be changed as little as possible; thus, it is perfectly safe to write contracts in ETH-HLL now and they will continue to work even if the language changes.

We still do not have a final idea of how we will deal with mandatory fees; the current stop-gap approach is now to have a block limit of 1000000 operations (ie. GAS spent) per block. Economically, a mandatory fee and a mandatory block limit are essentially equivalent; however, the block limit is somewhat more generic and theoretically allows a limited number of transactions to get in for free. There will be a blog post covering our latest thoughts on the fee issue shortly. The other idea that I had, stack traces, may also be implemented later.

In the long term, maybe even beyond Ethereum 1.0, perhaps the holy grail is attack the last two “intrinsic” parts of the system, and see if we can turn them too into contracts: ether and ECDSA. In such a system, ether would still be the privileged currency in the system; the current thinking is that we will premine the ether contract into the index “1″ so it takes nineteen fewer bytes to use it. However, the execution engine would become simpler since there would no longer be any concept of a currency – instead, it would all be about contracts and message calls. Another interesting benefit is that this would allow ether and ECDSA to be decoupled, making ether optionally quantum-proof; if you want, you could make an ether account using an NTRU or Lamport contract instead. A detriment, however, is that proof of stake would not be possible without a currency that is intrinsic at the protocol level; that may be a good reason not to go in this direction.