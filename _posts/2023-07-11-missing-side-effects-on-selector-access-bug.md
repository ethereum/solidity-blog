---
layout: post
published: true
title: 'Bug in legacy code generation when accessing the .selector member on expressions with side-effects'
date: '2023-07-19'
author: Solidity Team
category: Security Alerts
---

On June 26, 2023, a bug in the legacy code generation pipeline of the Solidity compiler was found during
investigation of a security report related to the use of ``abi.decode`` with a ternary
expression as the type argument.

The legacy code generation was not evaluating expressions when accessing the ``.selector``
member on some complex expressions, like assignments and conditionals, while the ``via-ir`` code
generation behave as expected and evaluates such expressions.
This leads to a wrong compiler behavior in the legacy code generation.

The bug was already present since the compiler version ``0.6.2``.
Despite this, as far as we know, it was never exploited or reported externally, and
therefore we assigned it a severity of "low".

## Which Contracts are Affected?

If you still using the legacy code generation pipeline and your contract uses the 
``.selector`` member access on some complex expressions,
like the return of a function call, such expressions will not be evaluated in the 
legacy code generation, leading to a different behavior when compiling with and without ``via-ir``.

For example, the code below, if compiled with ``via-ir`` will result in ``x`` being 
assigned ``42``, evaluating the function call ``h()`` expression.
On the other hand, if compiled without ``via-ir`` it will result in ``x`` being ``0``.

```solidity
contract D {
    function f() external {}
}
contract C {
    uint256 public x;
    function f() public {
        h().f.selector;
    }
    function h() public returns (D) {
        x = 42;
        return new D();
    }
}
```

However, the fact that this bug remained undiscovered until now indicates that real
projects do not seem to be using such a pattern.
Since the expected usage of the ``.selector`` is a constant expression there is a very low chance
of this occurring by accident or without raising immediate suspicions when reading real-world code.
