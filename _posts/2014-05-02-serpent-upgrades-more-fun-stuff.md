---
id: 33
title: 'Serpent upgrades: More Fun Stuff'
date: 2014-05-02T09:49:42+00:00
author: Vitalik Buterin
layout: post
guid: http://blog.ethereum.org/?p=33
permalink: /2014/05/02/serpent-upgrades-more-fun-stuff/
dsq_thread_id:
  - "2784358952"
---
Over the past two weeks our primary focus has been getting all of the clients updated to PoC5 compatibility, and it definitely has been a long road. Among the changes to the VM include:
<ul>
	<li><b>The new init/code mechanism</b>: basically, when you create a contract, the code provided will execute immediately, and then the return value of that code will be what becomes the contract’s code. This allows us to have contract initialization code, but still keep to the same format of [nonce, price, gas, to, value, data] for both transactions and contract creation, also making it easier to create new contracts via forwarding contracts</li>
	<li><b>Reordering transaction and contract data</b>: the order is now [nonce, price, gas, to, value, data] in transactions and [gas, to, value, datain, datainsz, dataout, dataoutsz] in messages. Note that Serpent retains the send(to, value, gas), o = msg(to, value, gas, datain, datainsz) and o = msg(to, value, gas, datain, datainsz, dataoutsz) parameters.</li>
	<li><b>Fee adjustments</b>: transaction creation now has a fee of 500 gas, and several other fees were updated.</li>
	<li><b>The CODECOPY and CALLDATACOPY opcodes</b>: CODECOPY takes code_index, mem_index, len as arguments, and copies the code from code_index ... code_index+len-1 to memory mem_index ... mem_index+len-1. These are very useful when combined with init/code. There is also now CODESIZE.</li>
</ul>
The largest changes, however, have been to the architecture surrounding the protocol. On the GUI side, the C++ and Go clients are evolving rapidly, and we will see more updates from that side coming very shortly. If you have been following Ethereum closely, you have likely seen <a href="https://www.youtube.com/watch?v=ldJcw7JPHz0">Denny’s Lotto</a>, a full implementation of a lottery, plus GUI, written and executed inside the C++ client. From here on, the C++ client will shift toward being a more developer-oriented tool, whereas the Go client will start to focus on being a user-facing application (or rather, meta-application). On the compiler side, Serpent has undergone a number of substantial improvements.

First, the code. You can peek into the Serpent compiler under the hood and you will be able to see <a href="https://github.com/ethereum/serpent/blob/master/serpent/compiler.py#L44">all of the functions</a>available, together with their precise translations into EVM code. For example, we have:

72:     ['access', 2, 1,
73:         ['', '', 32, 'MUL', 'ADD', 'MLOAD']],

This means that what access(x,y) is actually doing under the hood is it’s recursively compiling whatever x and y actually are, and then loading the memory at index x + y * 32; hence, x is the pointer to the start of the array and y is the index. This code structure has been around since PoC4, but now I have upgraded the meta-language used to describe translations even further, so as to include even if, while and init/code in this construction (before they were special cases); now, only set and seq remain as special cases, and if I wanted to I could even remove seq by reimplementing it as a <a href="https://github.com/ethereum/serpent/blob/master/serpent/compiler.py#L203">rewrite rule</a>.

The largest changes so far have been for PoC5 compatibility. For example, if you run serpent compile_to_assembly 'return(msg.data[0]*2)', you will see:

["$begincode_0.endcode_0", "DUP", "MSIZE", "SWAP", "MSIZE", "$begincode_0", "CALLDATACOPY", "RETURN", "~begincode_0", "#CODE_BEGIN", 2, 0, "CALLDATALOAD", "MUL", "MSIZE", "SWAP", "MSIZE", "MSTORE", 32, "SWAP", "RETURN", "#CODE_END", "~endcode_0"]

The actual code there is just:

[2, 0, "CALLDATALOAD", "MUL", "MSIZE", "SWAP", "MSIZE", "MSTORE", 32, "SWAP", "RETURN"]

If you want to see what’s going on here, suppose that a message is coming in with its first datum being 5. We thus have:

2 -&gt; Stack: [2]
0 -&gt; Stack: [2, 0]
CALLDATALOAD -&gt; Stack: [2,5]
MUL -&gt; Stack: [10]
MSIZE -&gt; Stack: [10, 0]
SWAP -&gt; Stack: [0, 10]
MSIZE -&gt; Stack: [0, 10, 0]
MSTORE -&gt; Stack: [0], Memory: [0, 0, 0 ... 10]
32 -&gt; Stack: [0, 32], Memory: [0, 0, 0 ... 10]
SWAP -&gt; Stack: [32, 0], Memory: [0, 0, 0 ... 10]
RETURN

The last RETURN returns the 32 memory bytes starting from 0, or [0, 0, 0 ... 10], or the number 10.

Now, let’s analyze the wrapper code.

["$begincode_0.endcode_0", "DUP", "MSIZE", "SWAP", "MSIZE", "$begincode_0", "CALLDATACOPY", "RETURN", "~begincode_0", "#CODE_BEGIN", ..... , "#CODE_END", "~endcode_0"]

I elided the inner code explained above to make things clearer. The first thing we see are two labels, ~begincode_0 and~endcode_0, and the #CODE_BEGIN and #CODE_END guards. The labels mark the beginning and end of the inner code, and the guards are there for the later stages of the compiler, which understands that everything between the guards should be compiled as if it is a separate program. Now, let’s look at the first parts of the code. In this case, we have ~begincode_0 at position 10 and ~endcode_0 at position 24 in the final code. $begincode_0 and $endcode_0 are used to refer to these positions, and $begincode_0.endcode_0 refers to the length of the interval between them, 14. Now, remember that during contract initialization the call data is the code that you’re feeding in. Thus, we have:

14 -&gt; Stack: [14]
DUP -&gt; Stack: [14, 14]
MSIZE -&gt; Stack: [14, 14, 0]
SWAP -&gt; Stack: [14, 0, 14]
MSIZE -&gt; Stack: [14, 0, 14, 0]
10 -&gt; Stack: [14, 0, 14, 0, 10]
CALLDATACOPY -&gt; Stack: [14, 0] Memory: [ ... ]
RETURN

Notice how the first half of the code cleverly set up the stack so that it would push the inner code into memory indices 0…13, and then immediately return that chunk of memory. In the final compiled code,600e515b525b600a37f26002600035025b525b54602052f2, the inner code sits nicely to the right of the initializer code that simply returns it. In more complex contracts, initializers can also serve functions like setting certain storage slots to values, or even calling or creating other contracts.

Now, let us introduce the latest and most fun feature of Serpent: imports. One common use case in contract land is that you want to give a contract the ability to spawn off new contracts. Problem is, how to you put the code for the spawned contracts into the spawner contracts? Before, the only solution was the uncomfortable approach of compiling the newer contracts first, and then putting the compiled code into an array. Now, we have a better solution: import.

Put the following into returnten.se:

x = create(tx.gas - 100, 0, import(mul2.se))
return(msg(x,0,tx.gas-100,[5],1))

Now, put the following into mul2.se:

return(msg.data[0]*2)

Now, if you serpent compile returnten.se and <a href="http://blog.ethereum.org/2014/04/10/pyethereum-and-serpent-programming-guide-2/">run the contract</a>, you notice that, voila, it returns ten. The reason why is obvious. The returnten.se contract creates an instance of the mul2.se contract, and then calls it with the value 5. mul2.se, as the name suggests, is a doubler, and so it returns 5*2 = 10. Note that import is not a function in the standard sense; x = import('123.se') will fail, and import only works in the very specific context of create.

Now, suppose you are creating a 1000-line monster contract and want to split it up into files. To do that, we use inset. Intoouter.se, put:

if msg.data[0] == 1:
inset(inner.se)

And into inner.se, put:

return(3)

Running serpent compile outer.se gives you a nice piece of compiled code that returns 3 if the msg.data[0] argument is equal to one. And that’s all there is to it.

Upcoming updates to Serpent include:
<ul>
	<li>An improvement of this mechanism so it doesn’t load the inner code twice if you try to use import twice with the same filename</li>
	<li>String literals</li>
	<li>Space and code-efficiency improvements for array literals</li>
	<li>A debugging decorator (ie. a compiling function which tells you what lines of Serpent correspond to what bytes of compiled code)</li>
</ul>
In the short term, though, my own effort will focus on bugfixes, a cross-client test suite, and continued work on <a href="https://github.com/ethereum/ethereumjs-lib">ethereumjs-lib</a>.