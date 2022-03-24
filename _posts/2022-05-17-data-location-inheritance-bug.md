---
layout: post
published: true
title: 'Bug Concerning Data Location during Inheritance'
date: '2022-05-17'
author: Solidity Team
category: Security Alerts
---

On February 5th 2021, Nicolas Venturo reported a bug that allows
overriding functions to change the data location of parameters from
``memory`` to ``calldata``.

The bug was introduced in Solidity 0.6.9 together with the ability to use ``calldata``
data location for all variables (and not just parameters of external functions).

We assigned the bug a severity of "very low".

## Which Contracts are Affected?

The effect of the bug is that a memory pointer is interpreted as a calldata pointer
or vice-versa. It can only happen if you change the data location of a function
during inheritance and perform an internal call at a location that is only aware of
the original function signature from the base contract.

One case is that you have a base contract where you perform an internal call
to a virtual function of the same base contract and there is a derived contract
that overrides the base contract's function and changes the data location.

```solidity
abstract contract I {
    // The base contract uses "calldata"
    function f(uint[] calldata x) virtual internal;
}
contract C is I {
    // The derived contract uses "memory" and the compiler
    // does not complain - this is the bug in the compiler.
    function f(uint[] memory x)  override internal {
        // If you use `x`, it will access memory
        // even if `D.g` passed us a calldata pointer.
        // This leads to the wrong data being accessed.
    }
}
abstract contract D is I {
    function g(uint[] calldata x)  external {
        // Since D only "knows" `I`, the signature of `f`
        // uses calldata, while the virtual lookup ends
        // up with `C.f`, which uses memory. This results
        // in the calldata pointer `x` being passed and
        // interpreted as a memory pointer.
        f(x);
    }
}
contract X is C, D { }
```

In the example above, the bug will only manifest itself in
``X``, which combines the faulty override in ``C`` with the
virtual function call in ``D``.

It is likely that any test covering the affected function call
will be able to detect the problem.

External calls are not affected, and it is actually not a problem
to change the data location between ``calldata`` and ``memory``
for ``external`` functions. This is also how the bug was introduced:
The compiler did not distinguish between ``calldata`` and ``memory``
in inheritance because it does not matter for ``external`` functions
and ``calldata`` was only allowed inside ``external`` functions.
