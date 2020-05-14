---
layout: post
published: true
title: 'Solidity 0.6.8 Release Announcement'
date: '2020-05-14'
author: Solidity Team
category: Announcements
---

[Solidity v0.6.8](https://github.com/ethereum/solidity/releases/tag/v0.6.8) fixes three important bugs in the code generator and introduces a recommendation to use [SPDX license identifiers](https://https://spdx.dev/ids/#how). Furthermore, support for WebAssembly has been extended and it is now possible to access the `min` and `max` values of an integer type directly.

Please upgrade if you are using array slices, backslashes in string literals for ABIEncoderV2 or are concerned about non-payable constructors.

## Important Bugfixes

Please note that the three bugs outlined below have all been rated with a severity level of "very low".

### Incorrect Internal Escaping of String Literals

**Problem**: When ABIEncoderV2 is enabled, string literals passed directly to encoding functions or external function calls are stored as strings in the intemediate code. Characters outside the printable range are handled correctly, but backslashes are not escaped in this procedure. This leads to double backslashes being reduced to single backslashes and consequently re-interpreted as escapes potentially resulting in a different string being encoded.

**Solution**: String literals containing backslash characters are now properly escaped.

**Example of affected code**

```solidity
pragma experimental ABIEncoderV2;
contract C {
    function f() public payable returns (bytes memory) {
        // The reason string should contain two backslashes,
        // but during compilation, only a single backslash
        // ends up in the message.
        require(msg.value > 0, "back\\\\slash");
        // This string literal is not directly passed to
        // an encoding function and is properly escaped.
        return "back\\\\slash";
    }
}
```

We especially thank Alex Groce (@agroce) who found the bug through fuzzing!

### Array Slice Dynamically Encoded Base Type Bug

**Problem**: For arrays with dynamically sized base types, index range accesses that use a start expression that is non-zero will result in invalid array slices. Any index access to such array slices will result in data being read from incorrect calldata offsets. Array slices are only supported for dynamic calldata types and all problematic types require ABIEncoderV2 to be enabled.

**Solution**: Disallow array slices of arrays with dynamically encoded base types.

**Example of affected code**

```solidity
pragma experimental ABIEncoderV2;
struct S { uint[] data; }
contract C {
    function f(
        S[] calldata arrayOfStructs,
        uint[][] calldata arrayOfArrays
    ) external pure {
        // These two do not result in the correct value.
        // Both expressions are disallowed now.
        arrayOfStructs[1:];
        arrayOfArrays[1:];
    }
}

```
 

### Implicit Constructor Callvalue Check Bug

**Problem**: Starting from Solidity 0.4.5, the creation code of contracts without explicit payable constructor is supposed to contain a callvalue check that results in contract creation reverting, if non-zero value is passed. However, this check was missing in case no explicit constructor was defined in a contract at all, but the contract has a base that does define a constructor. In these cases it is possible to send value in a contract creation transaction or using inline assembly without revert, even though the creation code is supposed to be non-payable.

**Solution**: We added a callvalue check to the creation code of a contract which does not define a constructor but has a base that does define a constructor.

**Code example**

```solidity
contract Base {
    constructor() public payable {}
}
contract Derived is Base {
    // No constructor defined
}
```

Before 0.6.8, it was possible to send Ether together with the creation
transaction of an instance of ``Derived``. At the same time - confusingly -
the compiler did now allow Solidity-based creation of the form
``new Derived{value: 1}()``. We solved this discrepancy by reverting
during the constructor of ``Derived`` if Ether was provided with the creation.

To replicate the old behaviour, you can add a payable constructor to the most
derived contract.

## Other Bugfixes

* **ABI**: Skip `private` or `internal` constructors.
* **Code Generator**: Fixed an "Assembly Exception in Bytecode" error where requested functions were generated twice.
* **Natspec**: Fixed a bug that ignored `@return` tag when no other developer-documentation tags were present.
* **Type Checker**: Checks if a literal exponent in the `**` operation is too large or fractional.
* **Type Checker**: Disallow accessing `runtimeCode` for contract types that contain immutable state variables.
* **Yul Assembler**: Fix source location of variable declarations without value.

## New Features

### Language Features
* Implemented `type(T).min` and `type(T).max` for every integer type T that returns the smallest and largest value representable by the type.

### Compiler Features
* **Commandline Interface**: Don't ignore `--yul-optimizations` in assembly mode.
* **ABI**: Allow using abi encoding functions for calldata array slices without explicit casts.
* **Wasm binary output**: Implement `br` and `br_if`.

A big thank you to all contributors who helped  make this release possible!

Download the new version of Solidity [here](https://github.com/ethereum/solidity/releases/tag/v0.6.8).
