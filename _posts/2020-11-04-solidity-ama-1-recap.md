---
layout: post
published: true
title:  "Ask the Solidity Team Anything #1 Recap"
date:   2020-11-04
author: Solidity Team
category: Announcements
---

We hosted our very first Solidity team AMA on Reddit last week! We would like to take the opportunity to summarize the most interesting and most upvoted questions & answers in this post.

If you are interested in going through the full AMA thread you can do so [here](https://www.reddit.com/r/ethdev/comments/jigz5o/ask_the_solidity_team_anything_1/).

### General Questions

#### Roadmap Outlook: What does the Solidity team see as its most important feature goals in the medium-long term and what are the biggest blockers to achieving those goals?

As far as the compiler is concerned, currently the biggest task is getting the Yul IR to 100% coverage, something we should be able to complete towards the end of the year. Consequently, we should have two equal compiler pipelines towards mid-2021.

As far as language features are concerned we can list a few prominent topics and their current implementation hurdles: We would like to support templates (blocker: complexity), make copies and references more explicit (blocker: acceptance), move more towards functional programming (immutable by default, range-based loops, algebraic data-types, ...), make more efficient use of memory (eliminate "stack too deep", de-allocate memory), provide better data about internal structures for debuggers, and use SMT solvers and other powerful tools in the optimizer (blocker: ensuring correctness).

In general, the biggest difficulty is often getting proper feedback about language features and coordinating all these changes.

#### Users: What things/features do you wish people would stop doing/using with Solidity?

Regarding the usage of language features, people are free to do whatever they want as long as they know what they are doing. What is unfortunate is that source flattening for source verification is so popular. The flattened source is less structured and it prevents you from using the modularity of imports and other features. The [sourcify validation script](https://www.npmjs.com/package/@ethereum-sourcify/validation) can be used to extract all you need from a truffle or buidler/hardhat project directory to re-compile your smart contracts in a non-flattened form.

#### Stack too deep: Is it possible to ever change the way the stack is accessed in order to get rid of the "Stack too deep" issues?

Yes! In fact, we are currently working on it. 

You can check the progress [here](https://github.com/ethereum/solidity/pull/10015). This will only go into the new code generator and if you access memory from inline assembly, you will have to make some changes, but it should generally not be a problem anymore in the long term.

#### Yul/Inline Assembly: Is there a document that explains in detail how each instruction in assembly works?

The [table of builtins](https://solidity.readthedocs.io/en/develop/yul.html#evm-dialect) should provide a good summary. If you need more details about the EVM, maybe try the [section on the EVM](https://solidity.readthedocs.io/en/develop/introduction-to-smart-contracts.html#index-6). If you are missing something, please feel free to open an issue in the [Solidity repo](https://github.com/ethereum/solidity/issues)!

#### Rollups: What do you think of optimism's solc customisations? Is this a viable way to implement containerisation?

I hope they will switch to a yul-based approach once the code generator is finished. The code generated through the IR does not contain any dynamic jumps and it can be easily rewritten by a very simple tool to do what optimism needs.

### Solidity Optimizer

#### What is the Solidity Optimizer optimizing for (size or cost or something else?) and how does it achieve that?

The Solidity compiler uses two different optimizer modules: The "old" optimizer that operates at opcode level and the "new" optimizer that operates on Yul IR code. The opcode-based optimizer applies simplification rules from the [list](https://github.com/ethereum/solidity/blob/develop/libevmasm/RuleList.h) to opcodes next to each other. It also combines equal code sets, removes unused code and some other things. The Yul-based optimizer is much more powerful, because it can work across function calls: In Yul it is not possible to perform arbitrary jumps, so it is for example possible to compute the side-effects of each function: If a function does not modify storage, a call to it can be swapped with a function that does. If a function is side-effect free and its result is multiplied by zero, you can remove the function call completely.

The optimizer tries to simplify complicated expressions (which reduces both size and execution cost), but it also specializes or inlines functions. Especially function inlining is an operation that can cause much bigger code, but it is often done because it results in opportunities for more simplifications.

In the opcode-based optimizer there is only one stage (the "constant optimizer") where the trade-offs between deploy-time and run-time costs are taken into account. This stage tries to find a "better" representation of each number in the source, like ``0x10000000000`` can be encoded as ``PUSH6 0x10000000000`` (7 bytes and almost zero run-time costs), but it can also be encoded as `PUSH1 1 PUSH1 40 SHL` (5 bytes and a bit more expensive at run-time). Most of the time, the difference is not too relevant.

One of the big advantages of the Yul-based optimizer is that each step can be seen in isolation: Each step receives Yul code as input and produces Yul code as output, without any tight dependency on other steps or analysis code. Furthermore, we try to keep each step as simple as possible so that bugs in those steps are very unlikely. As long as each simple step is bug-free, so is the whole Yul optimizer.

#### How does it work?

There are several steps that the optimizer performs. A simple example would be evaluating expressions whose value is known at runtime, e.g., `x + 0` is evaluated to `x`, where `x` may be a parameter known only during runtime. A more complex example would be to identify expressions that remains invariant inside a loop and moving them outside the loop, thereby saving gas. Another interesting example would be avoiding accessing the same value from storage multiple times, i.e., multiple `sload` to the same slot can be reduced to a single `sload`, in certain situations.

References:
- [High level description of steps in Yul optimizer](https://github.com/ethereum/solidity/blob/develop/libyul/optimiser/README.md)
- [List of steps in Yul optimizer](https://solidity.readthedocs.io/en/v0.7.4/yul.html?highlight=ir%20optimize#yul-optimizer)
- [Assembly based optimizer](https://solidity.readthedocs.io/en/v0.7.4/internals/optimiser.html)
<br><br>

#### What are the typical differences between optimized and non-optimized code?

Generally, the most visible difference would be constant expressions getting evaluated. When it comes to the ASM output, one can also notice reduction of equivalent/duplicate "code blocks" (compare the output of the flags `--asm` and `--asm --optimize`). However, when it comes to the Yul/intermediate-representation, there can be significant differences, for example, functions can get inlined, combined, rewritten to eliminate redundancies, etc. (compare the output between the flags `--ir` and `--optimize --ir-optimized`).

#### How is optimization affected by the number of runs (`--optimize-runs`) and is there a maximum number above which it stops mattering, or is `--optimize-runs=20000` less efficient than `--optimize-runs=500000`?

The parameter specifies roughly how often each opcode of the deployed code will be executed across the life-time of the contract. A "runs" parameter of "1" will produce short but expensive code. The largest value is ``2**64-1``.

#### When multiple small variables are packed into a single storage slot, how can we be sure accessing them both is done with a single SSTORE/SLOAD? How "nearby" do the accesses have to be each-other, and is there any better way to ensure this than inspecting the assembly?

As long as this still uses the non-yul code generator, it is actually rather limiting. It is best to not have any branches in between the accesses, so assigning a memory struct to storage should work best.

#### Does it make sense to cache ``arr.length`` in a stack variable when looping over an array from front to back, or will it SLOAD/MLOAD that only once if it sees it won't change in the loop body?

In the Yul optimizer (for the new code generator), there is an optimization step called LoopInvariantCodeMotion which is designed to detect expressions that remain invariant in the loop and move them outside the loop. Take the following Solidity example that finds the sum of a dynamic integer array in storage.

```solidity
uint sum = 0;
for (uint i = 0; i < arr.length; ++i)
{
    sum += arr[i];
}
```

The optimization step can correctly identify that the `arr.length` remains invariant and will move it outside the loop. So there is no need for manually caching the length for this example.

To understand if you need to manually cache length, or any other storage/memory value inside a loop, we'll describe how the step works.

1. The step only deals with expressions that remain the same or invariant, so in the above example, `arr[i]` will not even be considered for moving.
2. If such expressions are movable, i.e., the expression does not have any side effects, they are moved outside right away.  Examples of such instructions would be arithmetic operations such as `add` or instructions that do not read/modify memory, storage or blockchain state, e.g., `address`. Non-examples would be `keccak256` (reads from memory), `sload` (reads from storage), `call` (can modify blockchain state and contract storage.)
3. If such expressions have side effects, but only the *read* kind, i.e., reading from storage or memory, e.g., `sload`, `mload`, `extcodesize`, etc., then they can be moved out of the loop if the loop does not write to the corresponding location. In the above example, even though `arr.length` reads from storage, since no other expression in the loop can write to storage, we can move `arr.length` outside the loop. Note that the step cannot reason about fine-grained storage or memory locations. i.e., writing to storage slot, say `0`, will mean that `sload(1)` cannot be moved outside. This may be improved in the future.

In short, for the new code generator, one does not need to cache reads from a storage (or memory) if there are no writes to storage (or memory). Manual caching will only be beneficial in the following situation: if the loop contains a write, but if the contract author can reason that the write does not modify a variable that was read. An example of this situation would be the following:

```solidity
// Copying storage arrays arr1 into arr2, assuming arr2 is big enough.
// Example where caching is helpful:
// uint len = arr1.length
// and replacing arr1.length with len will save gas
for (uint i = 0; i < arr1.length; ++i)
{
    arr2[i] = arr1[i];
}
````	

Please note that the new code generator is not active yet. For the old generator, there are some inefficiencies and it is hence probably cheaper to cache the length on the stack.

#### Does the compiler ever inline functions? Does it make sense for the user to be able to request the compiler to do this? Even if it's not worth it to reduce call overhead, this may allow new opportunities for constant folding.

The current code generator does not inline function, but the new one will, exactly for that purpose. We do not plan to make those user-controllable. It might also make sense to inline the function depending on how it is called, but in general, small functions are very likely to be inlined.

#### Why do you think people are generally suspicious of the optimizer, and are they right to be?

The optimizer used to be very complicated some years ago. In the meantime, we disabled most of the complicated routines and fixed several bugs. While bugs can be present in the optimizer as they can be in any code, they often manifest themselves in a way that is easily detected. New compiler code like **ABIEncoderV2** focuses more on correctness instead of efficiency and is written with the assumption in mind, that the optimizer will be used. So for recent versions of Solidity, we would recommend to always use the optimizer unless you really do not care about gas costs.

### Security 

#### What are your plans for making Solidity a safer language? (Especially with regards to arguments raised in [this article](https://blog.blockstack.org/bringing-clarity-to-8-dangerous-smart-contract-vulnerabilities/).)

Most of the issues raised in the linked article have been fixed months if not years ago. In our design decisions, we have always focused on safety. Most of the work Solidity does is evening out restrictions and weirdnesses that the EVM has. One example is that a call to a non-existing contract is considered successful. Because of that, Solidity always checks that the contract to be called exists. Furthermore, the built-in SMT checker (``pragma experimental SMTChecker``) is improving on a weekly basis and can detect many problems while you are writing your code.
       
Let's tackle the mentioned issues step-by-step:

1. **Reentrancy**: The Ethereum community has no clear consensus over whether reentrancy is a feature or not. Over the years, many tools have evolved to flag code that has issues with reentrancy and blocking reentrant calls altogether is not only expensive, it also creates a new class of bugs.
     
2. **Access Control**: Visibility has been solved years ago by making it explicit. In recent Solidity versions, you can even move functions to the file level (outside of contracts) where it is obvious that they cannot be called from outside and that they cannot access storage variables (unless explicitly provided as arguments).

3. **Overflow and Underflow**: Safe math as a languge feature is already done and is going to become the default in the next breaking release (see [Solidity 0.8.x Preview Release](https://solidity.ethereum.org/2020/10/28/solidity-0.8.x-preview/)).
   
4. **Unchecked Return Values For Low Level Calls**: The compiler has been flagging unchecked low-level calls for years.

5. **Denial of Service**: With each breaking release, we are limiting more and more what can be done with objects of unlimited size. The biggest change in that direction is [making the semantics of copying more visible](https://github.com/ethereum/solidity/issues/2435) but we have not yet received much feedback whether this feature would be more annoying that helpful.

6. **Bad Randomness**: This is more of a general blockchain issue rather than something to be solved in Solidity. How can you be sure that any number you see on the blockchain is actually random[?](https://xkcd.com/221/) More importantly, how do you ensure that the one who generated it does not have an unfair advantage in knowing the number ahead of time? Well, there are ways to achieve that, e.g. Verifiable Random Functions, but there are two big problems with baking it into the language. First, this is all new crypto and none of this is standardized. There are new methods being developed all the time and new vulnerabilities being found. Compared to the usual pace of cryptographic research it's all developing at a breakneck speed and there's a lot of risk in committing to any particular scheme. Second, you can't really do it on-chain. And even if you could it would be very expensive (proof generation is infamously intensive in terms of computation). You're better off relying a solution where the randomness is provided by an oracle and only doing the proof verification on-chain.

7. **Time manipulation**: There's not much you can do to prevent these kinds of attacks at Solidity level. The timestamp comes from the underlying blockchain and is set by the miner. [Clock synchronization](https://en.wikipedia.org/wiki/Clock_synchronization) is hard even before you start taking malicious actors into account. The clients do have validation rules that prevent these timestamps from drifting too far apart from the clocks on the client machines but that's not enough to prevent the miner from introducing minuscule differences that are exploited in this attack. And Bitcoin is affected by it as much as Ethereum is. The additional complication in the GovernMental attack was that the contract author was the attacker. So preventing it would not just be a matter of providing better features and ergonomics but actively restricting the programmer. As a general rule we try to provide as much safety as possible at Solidity level but still let you go down to the inline assembly level and do pretty much anything you want. Block timestamp has a lot more legitimate uses than malicious ones so going so far as to stop you from using it at all would be simply unacceptable.

8. **Short address attack**: Since Solidity 5.0 there's a built-in protection that will revert the transaction if the calldata is too short. If you use ABIEncoderV2 you are even protected against inputs that do not fit the provided type. Also, please note that Solidity ABI **is** strongly typed and that this alone cannot protect you if the ABI definition used by the client does not match the actual ABI of the contract or if the client has a bug and is not actually following it.

We hope you enjoyed the first AMA and are looking forward to many questions in the next edition! In the meantime, feel free to join the [language design mailing list](https://groups.google.com/g/solidity-users) or check out our newly launched [Solidity language portal](https://soliditylang.org/).
