---
layout: post
published: true
title: 'Bug when Copying Dirty Bytes Arrays to Storage'
date: '2022-06-15'
author: Solidity Team
category: Security Alerts
---

On July 1, 2021, a bug in the Solidity code generator was found by differential fuzzing.
The bug causes the legacy code generation pipeline to generate code that
may write dirty values to storage when copying ``bytes`` arrays from ``calldata`` or ``memory``.

Initially, it was assumed that the dirty values in storage are only observable using inline
assembly. However, resizing a ``bytes`` array using an empty ``.push()`` without actually
writing values to it, can expose the dirty bytes without any use of inline assembly.

The bug was already present in the initial implementation of ``bytes`` arrays in very early versions of the compiler.
Despite this, it was never exploited or reported externally, and therefore we assigned it a severity of "low".

## Which Contracts are Affected?

Most instances of copying ``bytes`` arrays from ``memory`` or ``calldata`` to ``storage`` can be affected
in the sense that they may write dirty values. Those dirty values in storage only become visible,
if there are empty ``.push()``es to the ``bytes`` array (in older version also assignments to its
``.length`` field) and the resulting elements are assumed to be zero without actually writing to them.

For example, the following used to result in the newly ``.push()``ed array element in ``h()`` not being zero:

```solidity
contract C {
    event ev(uint[], uint);
    bytes s;
    constructor() {
        // The following event emission involves writing to temporary memory at the current location
        // of the free memory pointer. Several other operations (e.g. certain keccak256 calls) will
        // use temporary memory in a similar manner.
        // In this particular case, the length of the passed array will be written to temporary memory
        // exactly such that the byte after the 63 bytes allocated below will be 0x02. This dirty byte
        // will then be written to storage during the assignment and become visible with the push in ``h``.
        emit ev(new uint[](2), 0);
        bytes memory m = new bytes(63);
        s = m;
    }
    function h() external returns (bytes memory) {
        s.push();
        return s;
      }
}
```

Similarly, dirty values from ``calldata`` may end up being copied to storage:

```solidity
contract C {
    bytes public s;
    function f(bytes calldata c) external returns (bytes memory) {
        s = c;
        s.push();
        return s;
    }
}
```

The function ``f`` here can be called with dirty bytes in calldata past the length of ``c``, which
will be copied to ``s`` and become visible after ``s.push()``.

However, the fact that this bug remained undiscovered until now indicates that real projects do not seem to depend
on the fact that empty ``.push()``es to ``bytes`` arrays are supposed to add a zeroed new element.
