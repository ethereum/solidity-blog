---
layout: post
published: true
title: Saving Gas with Simple Inlining
date: '2021-03-02'
author: Christian Reitwiessner
category: Explainers
---

[Solidity v0.8.2](https://github.com/ethereum/solidity/releases/tag/v0.8.2) adds a simple inliner to the [low-level optimizer](https://docs.soliditylang.org/en/latest/internals/optimiser.html) of Solidity.  In this post, we examine how it works and take a look at synergies with other steps of the optimizer.

### Low-Level Inliner

The Low-Level Inliner is a component of the low-level optimizer of the Solidity compiler.
To save gas, it can inline short functions that do not contain control-flow branches or
opcodes with side-effects.

The decision to inline or not is based on the trade-off parameter "runs":
The combined code deposit cost and execution cost is computed with the
assumption that the code is executed "runs" times. If the inlined version
is estimated to be cheaper overall than the non-inlined version, the function call is inlined.

In particular, the optimizer analyzes code of the form

```
PUSH <tag>
JUMP
...
<tag>:
[ROUTINE]
JUMP
```

If the sequence of opcodes in ``[ROUTINE]`` is short and simple enough, the first ``JUMP``
is replaced by a copy of this code to result in the following:

```
[ROUTINE]
JUMP
...
<tag>:
[ROUTINE]
JUMP
```

If all references to the tag are eliminated in that way, then also the tag itself
and the original routine can be removed, saving even more gas.

### Step-by-step Walkthrough and Synergies with Other Optimizer Steps

You might ask what it means for ``[ROUTINE]`` to be "simple" and why it has to terminate
in a ``JUMP`` opcode. The idea behind this
restriction is that we want the inliner to just be the first step towards further optimizations.

Take a look at the following code:

```solidity
function unsafeAdd(uint x, uint y) pure returns (uint) {
    unchecked { return x + y; }
}
function doSomthing(uint x) pure returns (uint) {
    ...
    uint z = unsafeAdd(x, 7);
    ...
}
```

We of course want the call to ``unsafeAdd`` to be inlined. Internally, Solidity translates
the function call roughly to the following assembly code:

```
...
PUSH <returnTag>
PUSH 7
DUP3 // fetch x
PUSH <unsafeAdd>
JUMP
returnTag:
...

unsafeAdd:
ADD
SWAP1
JUMP
```

For the call, the stack looks as follows (top of the stack on the right):

```
<return address> <y> <x>
```

So the last ``JUMP`` in the code jumps back to the call site.

After inlining the code looks like this:

```
...
PUSH <returnTag>
PUSH 7
DUP3 // fetch x
ADD
SWAP1
JUMP
returnTag:
...

unsafeAdd:
ADD
SWAP1
JUMP
```

There is another optimizer stage in the Solidity compiler called the
"Common Subexpression Eliminator". Despite its name, it is actually a symbolic reasoning engine
that transforms code into an internal representation, simplifies it and tries to generate
code with the same semantics but fewer instructions. This stage notices that the
``PUSH <returnTag>`` lies unused on the stack until the very end (it is consumed by the
``JUMP``) and re-arranges the code in the following way:

```
...
PUSH 7
DUP2 // fetch x
ADD
PUSH <returnTag>
JUMP
returnTag:
...

unsafeAdd:
ADD
SWAP1
JUMP
```

Now the code is in a form where the target of the ``JUMP`` opcode
can be determined without a potentially costly stack analysis, since it is pushed
right above the opcode. Furthermore, it is a jump to a tag that is just the
next opcode. This is a perfect opportunity for another stage in the optimizer called the
"Peephole Optimizer": It tries to find simple patterns of sequences of opcodes without
doing a full semantic or symbolic analysis. It will remove the "jump to next"
opcode triple and turn the code into this:

```
...
PUSH 7
DUP2 // fetch x
ADD
...

unsafeAdd:
ADD
SWAP1
JUMP
```

And of course finally, there is an unreachable code remover that can eliminate the "unsafeAdd" routine
(unless it is referenced from somewhere else):

```
...
PUSH 7
DUP2 // fetch x
ADD
...
```

Now to get back to the point why we require the routine to be simple:
As soon as you do more complicated things like for example branching, calling external contracts,
the Common Subexpression Eliminator cannot re-construct the code anymore or does not
do full symbolic evaluation of the expressions. Furthermore, it can only fully inline the
function if there is a ``JUMP`` at the end.

### Conclusion and Future Outlook

In our repository, this simple routine was able to reduce the gas costs of many tests.
In order to prevent bugs, we always strive to keep individual optimizer stages as simple as possible
so that their full potential is mainly realized in combination with other stages.
Because it is situated in the low-level optimizer, this new inliner can realize optimization
opportunities that the high-level inliner cannot, because it does not operate on the level
of individual jumps and cannot split functions.

There is one downside in this routine that we carefully considered before implementing it
the way it is now: Since it can split functions, it might lead to debuggers being confused
about where a function starts and where the actual call is - to a point where a function
is removed altogether, which actually is the main point here.

With more and more radical optimizations, the debuggability of Smart Contracts deteriorates.
This can be helped by the compiler maintaining debugging information that is transformed
together with the optimizer step and exporting it to help analysis. This is one task we
want to tackle in the future.
