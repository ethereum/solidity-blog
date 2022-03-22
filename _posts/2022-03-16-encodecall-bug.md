---
layout: post
published: true
title: 'abi.encodeCall Literals Bug'
date: '2022-03-16'
author: Solidity Team
category: Security Alerts
---

On March 10th, 2022, the Solidity team discovered a bug in the implementation of
``abi.encodeCall`` when used together with fixed-length bytes literals.

It was introduced together with ``abi.encodeCall`` in Solidity 0.8.11 and is fixed in 0.8.13.

We assigned the bug a severity of "very low".

## Which Contracts are Affected?

You might be affected if you use ``abi.encodeCall(f, (...))`` where ``f`` takes a
``bytesNN`` parameter and you provide the value for that parameter either as a
hex literal (``0x1234`` or ``hex"abcd"``) or
as a string literal (``"abcd"``).

If you only pass variables to ``abi.encodeCall``, your code is not affected.

## Technical Details

The compiler did check that the types of the values are all implicitly convertible
to the types of the parameters of the function (this was the main advantage of
``abi.encodeCall`` over ``abi.encode`` and the reason for adding the feature),
but it did not take the types properly into account when generating the code for
this function call. The generated code was still the same as the one for
``abi.encode``, which just encodes according to the types of the arguments.

The problem is that literals like ``0x1234`` and ``"abcd"`` can be implicitly converted to
different types: The first can be converted to both ``bytes2`` and ``uint16`` and the
second can be converted to ``bytes4`` and ``bytes memory``.

In the first case, the compiler chose to encode ``0x1234`` as a ``uint16``. This means it
right-aligned it instead of left-aligning it for ``bytes2``. In the second case, it
was encoded as ``bytes memory``, which is a dynamic type, and thus at the place of
the ``bytes4`` value, an offset pointer was stored and additional data was added at the end.

Since the bug is only present for literals (actual variables have proper types and
implicit conversions are always no-ops), it should be easy to detect with simple testing.
