---
id: 2716
title: Go Ethereum’s JIT-EVM
date: 2016-06-02T16:21:36+00:00
author: Jeffrey Wilcke
layout: post
guid: https://blog.ethereum.org/?p=2716
permalink: /2016/06/02/go-ethereums-jit-evm/
dsq_thread_id:
  - "4877933833"
---
The Ethereum Virtual machine is kind of different than most other Virtual Machines out there. In my <a class="markup--anchor markup--p-anchor" href="https://medium.com/@jeff.ethereum/optimising-the-ethereum-virtual-machine-58457e61ca15" data-href="https://medium.com/@jeff.ethereum/optimising-the-ethereum-virtual-machine-58457e61ca15">previous post</a> I already explained how it’s used and described some of its characteristics.
<blockquote>The Ethereum Virtual Machine (EVM) is a simple but powerful, Turing complete 256bit Virtual Machine that allows anyone to execute arbitrary <a class="markup--anchor markup--blockquote-anchor" href="https://github.com/ethereum/go-ethereum/blob/master/core/vm/opcodes.go" rel="nofollow" data-href="https://github.com/ethereum/go-ethereum/blob/master/core/vm/opcodes.go">EVM Byte Code</a>.</blockquote>
The go-ethereum project contains two implementations of the EVM. A simple and straightforward <a class="markup--anchor markup--p-anchor" href="https://en.wikipedia.org/wiki/Bytecode" rel="nofollow" data-href="https://en.wikipedia.org/wiki/Bytecode">byte-code VM</a> and a more sophisticated <a class="markup--anchor markup--p-anchor" href="https://en.wikipedia.org/wiki/Just-in-time_compilation" rel="nofollow" data-href="https://en.wikipedia.org/wiki/Just-in-time_compilation">JIT-VM</a>. In this post I’m going to explain some of the differences between the two implementations and describe some of the characteristics of the JIT EVM and why it can be so much faster than the byte-code EVM.

<!--more-->
<h3 id="c1d8" class="graf--h3 graf--hasDropCapModel graf--leading">Go-ethereum’s Byte Code Virtual Machine</h3>
The EVM’s internals are pretty simple; it has a single run loop which will attempt to execute the instruction at the current <em class="markup--em markup--p-em">Program Counter (PC</em> in short). Within this loop the <em class="markup--em markup--p-em">Gas</em> is calculated for each instruction, memory is expanded if necessary and executes the instruction if the preamble succeeds. This will continue on until the VM either finishes gracefully or returns with an error by throwing an exception (e.g. <em class="markup--em markup--p-em">out-of-gas</em>).
<pre id="281b" class="graf--pre graf-after--p"><code>for op = contract[pc] {</code>
<code>    if !sufficientGas(op) {</code>
<code>        return error("insufficient gas for op:", or)</code>
<code>    }
</code><code>    switch op {</code>
<code>    case ...:</code>
<code>        /* execute */</code>
<code>    case RETURN:</code>
<code>        return memory[stack[-1], stack[-2]]</code>
<code>    }</code>
<code>    pc++</code>
<code>}
</code></pre>
At the end of the execution loop the program-counter gets increment to run the next instruction and continues to do so until it has finished.

The EVM has another way to <em class="markup--em markup--p-em">change</em> the program-counter through something called <em class="markup--em markup--p-em">jump</em>-instructions (<em class="markup--em markup--p-em">JUMP</em> &amp; <em class="markup--em markup--p-em">JUMPI). </em>Instead of letting the program-counter increment (pc++) the EVM can also jump to arbitrary positions in the contract code. The EVM knows two jump instructions, a normal jump that reads as “<em class="markup--em markup--p-em">jump to position X</em>” and a conditional jump that read as “<em class="markup--em markup--p-em">jump to position X if condition Y is true</em>”. When either such a jump occurs it must always land on a <em class="markup--em markup--p-em">jump-destination</em>. If the program lands on an instruction other than a jump destination the program fails — in other words, for a jump to be valid it must always be followed by a jump-destination instruction if the condition yielded true.

Prior to running any Ethereum program the EVM iterates over the code and finds all possible jump-destinations, it then puts them in a map that can be referenced by the program-counter to find them. Each and every time the EVM encounters a jump-instructions the jump validity is checked.

As you can see the executing code is relatively easy and simply interpreted by the byte-code VM, we may conclude even that through its sheer simplicity it’s actually pretty dumb.
<h3 id="0544" class="graf--h3 graf-after--p">Welcome JIT VM</h3>
The JIT-EVM takes a different approach to running EVM byte-code and is by definition <em class="markup--em markup--p-em">initially</em> slower than the byte-code VM. Before the VM can run any code it must first <em class="markup--em markup--p-em">compile</em> the byte-code in to components that can be understood by the JIT VM.
<p id="9d2b" class="graf--p graf-after--p">The initialisation- and execution procedure is done in 3-steps:</p>

<ol class="postList">
 	<li id="72fd" class="graf--li graf-after--p">We check whether there’s a JIT program ready to be run using the hash of the code — <em class="markup--em markup--li-em">H(C)</em> is used as an identifier to identify the program;</li>
 	<li id="6298" class="graf--li graf-after--li">if a program was found we run the program and return the result;</li>
 	<li id="1234" class="graf--li graf-after--li">if no program was found we run the byte-code <em class="markup--em markup--li-em">and</em> we compile a JIT program in the background.</li>
</ol>
<p id="3eac" class="graf--p graf-after--li">Initially I tried to check whether the JIT program had finished compiling and move the execution over to the JIT — this all happened during runtime in the same loop using Go’s <em class="markup--em markup--p-em">atomic</em> package — unfortunately it turned out to be slower than letting the byte-code VM run and use the JIT program for every sequential call after the compilation of the program had finished.</p>
<p id="2b52" class="graf--p graf-after--p">By compiling the byte-code in to logical pieces the JIT has the ability to analyse the code more precisely and optimise where and whenever necessary.</p>
<p id="bf79" class="graf--p graf-after--p">For example an incredible simple optimisation that I did was compiling several <em class="markup--em markup--p-em">push</em> operation in to a single instruction. Let’s take the <em class="markup--em markup--p-em">CALL </em>instruction; call requires 7 push instructions — i.e. gas, address, value, input-offset, input-size, return-offset and return-size — prior to executing it, and what I did instead of looping through these 7 instructions, executing them one by one, I’ve optimised this away by taking the 7 instructions and append the 7 values in to a single slice. Now, whenever the <em class="markup--em markup--p-em">start</em> of the 7 push instructions is executed, it instead executes the one optimised instruction by immediately appending the static slice to the VM stack. Now of course this only works for static values (i.e. <em class="markup--em markup--p-em">push 0x10),</em> but these are present in the code quite a lot.</p>
<p id="77ce" class="graf--p graf-after--p">I’ve also optimised the <em class="markup--em markup--p-em">static jump </em>instructions. Static jumps are jumps who always jump to the same position (i.e. <em class="markup--em markup--p-em">push 0x1, jump</em>) and never change under any circumstance. By determining which jumps are static we can pre-check whether a jump is valid and lies within the bounds of the contract and if so we create a new instructions that replaces both the <em class="markup--em markup--p-em">push</em> and <em class="markup--em markup--p-em">jump</em>instruction and is flagged as <em class="markup--em markup--p-em">valid</em>. This prevents the VM from having to do two instructions and it prevents it from having to check whether the jump is valid and doing an expensive hash-map lookup for valid jump position.</p>

<h4 id="20a3" class="graf--h4 graf-after--p">Next steps</h4>
<p id="4189" class="graf--p graf-after--h4">Full stack and memory analysis would also fit nicely in this model where large chunks of code could fit in to single instructions. Further I’d like to add <a class="markup--anchor markup--p-anchor" href="https://en.wikipedia.org/wiki/Symbolic_execution" rel="nofollow" data-href="https://en.wikipedia.org/wiki/Symbolic_execution">symbolic-execution</a> and turn the JIT in to a proper JIT-VM. I think this would be a logical next step once programs get large enough to take advantage of these optimisations.</p>

<h3 id="c1b6" class="graf--h3 graf-after--p">Conclusion</h3>
<p id="2f9e" class="graf--p graf--hasDropCapModel graf--hasDropCap graf-after--h3"><span class="graf-dropCap">O</span>ur JIT-VM is a whole lot smarter than the byte-code VM, but is far from being completely done (if ever). There are many more clever tricks we could add with this structure, but simply aren’t realistic for the moment. The runtime is within the bounds of being “reasonable” speedy. May the need arise to further optimise the VM we have the tools to do so.</p>

<h4 id="06ef" class="graf--h4 graf-after--p">Further code-reading</h4>
<ul class="postList">
 	<li id="e56b" class="graf--li graf-after--h4"><a class="markup--anchor markup--li-anchor" href="https://github.com/ethereum/go-ethereum/blob/master/core/vm/vm.go#L164" rel="nofollow" data-href="https://github.com/ethereum/go-ethereum/blob/master/core/vm/vm.go#L164">Byte code VM execution</a></li>
 	<li id="87da" class="graf--li graf-after--li"><a class="markup--anchor markup--li-anchor" href="https://github.com/ethereum/go-ethereum/blob/master/core/vm/jit.go#L148" rel="nofollow" data-href="https://github.com/ethereum/go-ethereum/blob/master/core/vm/jit.go#L148">JIT VM Compilation</a></li>
 	<li id="4588" class="graf--li graf-after--li"><a class="markup--anchor markup--li-anchor" href="https://github.com/ethereum/go-ethereum/blob/master/core/vm/jit.go#L323" rel="nofollow" data-href="https://github.com/ethereum/go-ethereum/blob/master/core/vm/jit.go#L323">JIT VM Program execution</a></li>
 	<li id="cb0a" class="graf--li graf-after--li graf--last"><a class="markup--anchor markup--li-anchor" href="https://github.com/ethereum/go-ethereum/blob/master/core/vm/jit_optimiser.go#L29" rel="nofollow" data-href="https://github.com/ethereum/go-ethereum/blob/master/core/vm/jit_optimiser.go#L29">JIT VM Optimiser</a></li>
</ul>
Cross posted from - https://medium.com/@jeff.ethereum/go-ethereums-jit-evm-27ef88277520#.1ed9lj7dz