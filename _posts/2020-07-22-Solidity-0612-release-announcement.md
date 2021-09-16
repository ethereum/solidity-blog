---
layout: post
published: true
title: 'Solidity 0.6.12 Release Announcement'
date: '2020-07-22'
author: Solidity Team
category: Releases
---

[Solidity v0.6.12](https://github.com/ethereum/solidity/releases/tag/v0.6.12) adds more flexibility
when using inheritance with NatSpec comments and introduces many small improvements to the optimizer.


## Notable New Features 

### NatSpec Inheritance

As explained in the [Solidity 0.6.11 release announcement](https://blog.soliditylang.org/2020/07/07/solidity-0611-release-announcement/),
NatSpec comments are now inherited automatically if you do not provide any
NatSpec in the derived function. Starting from Solidity 0.6.12, you can
explicitly inherit comments from a base contract using the tag ``@inheritdoc``
even if you specify some of the tags.
The tags that are not given will then be taken from the specified base class'
function.

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.6.11;
interface Gathering {
  /// The address `participant` just registered for the gathering.
  event Registered(address participant);

  /// Registers `msg.sender` to take part in the gathering.
  /// @param name The name of the participant.
  function register(string calldata name) external;
}

contract MyGathering is Gathering {
  mapping(address => string) public participants;
  uint public participantCount;

  /// Registers `msg.sender` to take part in the gathering
  /// unless there are already more than 10 participants.
  /// @inheritdoc Gathering
  function register(string calldata name) public override {
    require(participantCount <= 10);
    participants[msg.sender] = name;
    participantCount++;
    emit Registered(msg.sender);
  }
}
```

The derived contract ``MyGathering`` in the example code above will have
the documentation for the parameter "name" as part of its developer documentation.


## Full Changelog

### Language Features:
* NatSpec: Implement tag ``@inheritdoc`` to copy documentation from a specific base contract.
* Wasm backend: Add ``i32.ctz``, ``i64.ctz``, ``i32.popcnt``, and ``i64.popcnt``.

### Compiler Features:
* Code Generator: Avoid double cleanup when copying to memory.
* Code Generator: Evaluate ``keccak256`` of string literals at compile-time.
* Optimizer: Add rule to remove shifts inside the byte opcode.
* Peephole Optimizer: Add rule to remove swap after dup.
* Peephole Optimizer: Remove unnecessary masking of tags.
* Yul EVM Code Transform: Free stack slots directly after visiting the right-hand-side of variable declarations instead of at the end of the statement only.

### Bugfixes:
* SMTChecker: Fix error in events with indices of type static array.
* SMTChecker: Fix internal error in sequential storage array pushes (``push().push()``).
* SMTChecker: Fix internal error when using bitwise operators on fixed bytes type.
* SMTChecker: Fix internal error when using compound bitwise operator assignments on array indices inside branches.
* Type Checker: Fix internal compiler error related to oversized types.
* Type Checker: Fix overload resolution in combination with ``{value: ...}``.

### Build System:
 * Update internal dependency of jsoncpp to 1.9.3.

A big thank you to all contributors who helped make this release possible!

Download the new version of Solidity [here](https://github.com/ethereum/solidity/releases/tag/v0.6.12).
