---
layout: post
published: true
title: 'Signed Immutables Bug'
date: '2021-09-29'
author: Solidity Team
category: Security Alerts
---

On September 28th, 2021, the Solidity team discovered that
for immutable variables of a signed integer type shorter than 256 bits,
sign extension (cleanup) of its value is not always properly performed.

To our knowledge, the value can only be accessed in its unclean state
when using inline assembly.
The bug is present since the introduction of the
`immutable` feature in Solidity 0.6.5 and is fixed in 0.8.9.

We assigned the bug a severity of "very low".

## Technical Details

When immutable variables are assigned in Solidity during the construction phase, the value
is stored in memory until the creation concludes. For types shorter than 32 bytes,
these values were stored left-aligned within the 32 byte word and when the values were
read again, they were shifted back accordingly, but no cleanup was performed on the values.
In particular, the signextend operation was not performed on signed values.

As an example, if you store ``-2`` in an ``int8``, it is stored in two's complement encoding
as a single-byte value of ``0xfe``. Since the words in the EVM use 32 bytes, the value
needs to be properly sign-extended before an operation like signed division is performed,
otherwise the value would be interpreted as a positive value.

Sign-extension means setting the higher order bits to the same value as the
sign bit, which is the most significant bit of the actual type.

The value ``-2`` represented as a two's complement in 32 bits (``int8``) is
``0xfe``, however, in EVM, it is sign extended to
``0xfffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffe``,
which also represents the value ``-2`` when interpreted as ``int256``.
Note that ``0xfe`` when interpreted as ``int256`` is ``+254``.

The missing sign extension was not discovered earlier, since Solidity always performs
additional cleanup (which can be removed by the optimizer if done twice)
right before an operation where it is important to have clean higher order bits
like copying to memory, comparing or dividing values.

So the only way
(to our knowledge) to obtain the "unclean" value of the immutable is to first
assign it to a local variable and then read from that local variable
inside inline assembly.

## Affected Example

In the example below, the call to ``f()`` returns ``0x000000...0000fe`` instead of
``0xffffff...fffffe``.

```solidity
contract C {
    int8 immutable x = -2;
    function f() public view returns (bytes32 r) {
        int8 y = x;
        assembly { r := y }
    }
}
```

If you use something like ``r = bytes32(int(y));`` to return the value without
inline assembly, it would return the correct value. The same is true when using
a getter - ``int8 public immutable x = -2``.

## Justification for Inline Assembly to be Required

We believe that inline assembly is required for this bug to manifest itself, not only
because we believe that the Solidity compiler always properly cleans the values
whenever it matters, but also for another reason:

The bug is only present in what we call the "legacy" code generator. Cleanup when
reading immutable values has been correctly implemented in the new Yul-IR-based
code generator (which is still considered experimental).

In order to find bugs in the new implementation, we regularly fuzz-test
the two implementations against each other using a mix of generational
(Solidity programs generated based on language grammar) and mutational
(mutated variants of existing Solidity programs) test creation engines.

This mechanism can of course not only find bugs in the new implementation,
but also in the old one.

Inline assembly is currently not used by the generational engine because
we know that the two implementations diverge on the assembly level
and it would lead to too many false positives.

Since the fuzzer has not found the bug in the almost 18 months it has been
part of the compiler, we are pretty sure that there is no other way
than inline assembly to access the unclean values.