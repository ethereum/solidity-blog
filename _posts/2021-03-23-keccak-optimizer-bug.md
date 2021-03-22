---
layout: post
published: true
title:  "Solidity Optimizer Keccak Caching Bug"
date:   2021-03-23
author: Solidity Team
category: Security Alerts
---

On March 20, 2021, a bug in Solidity's bytecode optimizer was found by differential fuzzing. The bug
is fixed with [version 0.8.3](https://github.com/ethereum/solidity/releases/tag/v0.8.3) released on
March 23, 2021. **The bug is present in all prior versions of Solidity.**

We assigned the bug a severity level of "medium".

## Technical Details

**Summary**: The bytecode optimizer incorrectly re-used previously evaluated Keccak-256 hashes. You
are unlikely to be affected if you do not compute Keccak-256 hashes in inline assembly.

Solidity's bytecode optimizer has a step that can compute Keccak-256 hashes, if the contents of the
memory, over which the Keccak-256 built-in function is invoked, are known during compilation time.
This step also has a mechanism to determine that two Keccak-256 hashes are equal even if the values
in memory are not known during compile time. This step had a bug where Keccak-256 hashes of the
same memory content, but of different sizes were considered equal. The following is a simple example
that illustrates the bug:

```solidity
contract C {
  function bug() public returns (uint a, uint b) {
    assembly {
      mstore(0, 0)
      // The optimizer computes the value at compile time:
      // 0x290decd9548b62a8d60345a988386fc84ba6bc95484008f6362f93160ef3e563
      a := keccak256(0, 32)
      // The optimizer incorrectly uses the cached value
      // and transforms the next line to
      // b := 0x290decd9548b62a8d60345a988386fc84ba6bc95484008f6362f93160ef3e563
      // instead of 
      // b := 0xe2b9f9f9430b05bfa9a3abd3bac9a181434d23a707ef1cde8bd25d30203538d8
      b := keccak256(0, 23)
    }
  }
}
```

In the above example, both the Keccak-256 hashes can be computed at compile time. Here are two other
cases where the hashes cannot be computed at compile time, yet the optimizer considered them equal
and thus modified the semantics:

```solidity
contract C {
  function bug(string memory s) public returns (bool ret) {
    assembly {
      let a := keccak256(s, 32)
      let b := keccak256(s, 8)
      // Here `a` and `b` were considered equal,
      // leading to `ret` being incorrectly set to true.
      ret := eq(a, b)
    }
  }
}
```

```solidity
contract C {
  function bug() public view returns (bool ret) {
    assembly {
      let x := calldataload(0)
      mstore(0, x)
      mstore(0x20, x)
      let a := keccak256(0, 4)
      // even though the memory location is different,
      // the 32-byte content is the same.
      let b := keccak256(0x20, 8)
      // Here `a` and `b` were considered equal,
      // leading to `ret` being incorrectly set to true.
      ret := eq(a, b)
    }
  }
}
```

Specifically, `keccak256(mpos1, length1)` and `keccak256(mpos2, length2)` in some cases were
considered equal if `length1` and `length2`, when rounded up to nearest multiple of 32 were the
same, and when the memory contents at `mpos1` and `mpos2` can be deduced to be equal.

You may be **affected** if you compute multiple Keccak-256 hashes of the same content, but with
different lengths inside inline assembly, and enabled the optimizer.

### Unaffected Cases

You are unaffected if your code:

- uses `keccak256` with a length that is not known at compile time,
- only uses `keccak256` with a length that is always a multiple of 32. In particular,
  for storage slot computation, the Solidity compiler always computes Keccak-256 hash over memory
  regions whose lengths and offsets are multiples of 32,
- contains pairs of `keccak256` hashes that are separated by certain instructions such as:
  - an instruction that breaks the control flow, e.g., `jumpi`, `jump` etc. In high level code, this
     would be function calls, if statements, loops, etc,
  - an instruction that writes to memory other than a very simple `mstore`, e.g., `call`,
    `returndatacopy`, etc.

Note that, apart from the builtin function `keccak256`, the compiler internally generates code that
makes use of the Keccak-256 hash. For example,
- in certain indexed event arguments,
- in the function `abi.encodeWithSignature`,
- when using `string` or `bytes` as keys in mappings,
- when accessing the data location of `mapping` and `array` types.

### Severity

Most usages of `keccak256` fall under one of the unaffected cases mentioned in the previous section.
In particular, this can only affect pairs of `keccak256` opcodes without an intermediate jump or an
external call, which makes the likelyhood of the bug being present in existing code very low. This
is also be validated by the fact that the bug has been present in the optimizer for so long without
being detected. Therefore, we assigned the bug a "medium" severity level.

### Fuzzing

The bug was discovered by differentially testing the current code-generation and the upcoming Yul
based code-generation with fuzzer generated input. Essentially, the example in
[solidity#11131](https://github.com/ethereum/solidity/issues/11131) produced different results, when
compiled using different code-generation paths; this is because the bytecode optimizer, in which the
bug was present, is only activated in the old code-generation, and the Yul based codegen currently
only uses the Yul-optimizer that does not contain this bug.

This bug discovery is part of ongoing fuzzing related work. You can read more about Solidity's
approach to fuzzing in this introductory [blog
post.](https://blog.soliditylang.org/2021/02/10/an-introduction-to-soliditys-fuzz-testing-approach/)
