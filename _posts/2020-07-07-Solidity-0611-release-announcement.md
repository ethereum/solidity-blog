---
layout: post
published: true
title: 'Solidity 0.6.11 Release Announcement'
date: '2020-07-07'
author: Solidity Team
category: Releases
---

[Solidity v0.6.11](https://github.com/ethereum/solidity/releases/tag/v0.6.11) adds inheritance to NatSpec comments,
improves debugging data output and fixes some minor issues with opening up ``calldata`` for non-external functions.


## Notable New Features 

### NatSpec Inheritance and on Events

NatSpec comments are a way to describe the behaviour of a function to end-users. It also allows
to provide more detailed information to developers.

One regular use-case is that you document the behaviour of an interface and then implement
the interface in a derived contract. Previously, you had to repeat the documentation in the derived
contract. This is now obsolete: The compiler will automatically inherit the documentation of
the base function if the derived function does not provide any NatSpec tag.

If you provide any of the tags (``@param``, ``@dev``, ...), then nothing will be inherited.
The next release will provide a feature to explicitly inherit from a certain base also in that case,
so stay tuned!


Furhermore, NatSpec is now supported on events.

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.6.11;
interface Gathering {
  /// The address `participant` just registered for the gathering.
  event Registered(address participant);

  /// Registers `msg.sender` to take part in the gathering.
  function register() external;
}

contract MyGathering is Gathering {
  mapping(address => bool) public participants;

  function register() public override {
    participants[msg.sender] = true;
    emit Registered(msg.sender);
  }
}
```

The derived contract ``MyGathering`` in the example code above will produce the following userdoc:
```json
{
    "events": {
        "Registered(address)": {
            "notice": "The address `participant` just registered for the gathering."
        }
    },
    "kind": "user",
    "methods": {
        "register()": {
            "notice": "Registers `msg.sender` to take part in the gathering."
        }
    },
    "version": 1
}
```

### Unit Denomination ``gwei``

You can now use ``gwei`` as a unit denomination alongside ``wei``, ``szabo``, ``finney`` and ``ether``:
```solidity
reqire(msg.value >= 10 gwei);
```


## Full Changelog


### Language Features:
 * General: Add unit denomination ``gwei``
 * Yul: Support ``linkersymbol`` builtin in standalone assembly mode to refer to library addresses.
 * Yul: Support using string literals exceeding 32 bytes as literal arguments for builtins.


### Compiler Features:
 * NatSpec: Add fields ``kind`` and ``version`` to the JSON output.
 * NatSpec: Inherit tags from unique base functions if derived function does not provide any.
 * Commandline Interface: Prevent some incompatible commandline options from being used together.
 * NatSpec: Support NatSpec comments on events.
 * Yul Optimizer: Store knowledge about storage / memory after ``a := sload(x)`` / ``a := mload(x)``.
 * SMTChecker: Support external calls to unknown code.
 * Source Maps: Also tag jumps into and out of Yul functions as jumps into and out of functions.


### Bugfixes:
 * NatSpec: Do not consider ``////`` and ``/***`` as NatSpec comments.
 * Type Checker: Disallow constructor parameters with ``calldata`` data location.
 * Type Checker: Do not disallow assigning to calldata variables.
 * Type Checker: Fix internal error related to ``using for`` applied to non-libraries.
 * Wasm backend: Fix code generation for for-loops with pre statements.
 * Wasm backend: Properly support both ``i32.drop`` and ``i64.drop``, and remove ``drop``.
 * Yul: Disallow the same variable to occur multiple times on the left-hand side of an assignment.
 * Yul: Fix source location of variable multi-assignment.

A big thank you to all contributors who helped make this release possible!

Download the new version of Solidity [here](https://github.com/ethereum/solidity/releases/tag/v0.6.11).
