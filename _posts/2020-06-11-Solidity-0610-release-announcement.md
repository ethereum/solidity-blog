---
layout: post
published: true
title: 'Solidity 0.6.10 Release Announcement'
date: '2020-06-11'
author: Solidity Team
category: Releases
---

[Solidity v0.6.10](https://github.com/ethereum/solidity/releases/tag/v0.6.10) fixes an important bug that was introduced in the previous release and adds error codes.

Please upgrade if you are using internal library functions with calldata parameters in connection with ``using for``.

## Important Bugfixes

### Invalid Access for Calldata Parameters in Internal Library Functions

**Problem**: The previous release, [Solidity 0.6.9](https://github.com/ethereum/solidity/releases/tag/v0.6.9), introduced the possibility to use ``calldata`` types in internal and not, as previously, only in external functions.
This resulted in a bug related to calling library functions with calldata parameters via ``using for``.
More specifically, the compiler would copy all calldata arguments to memory first - like done for calls to public library functions.
The internal library function would then receive the memory pointer but interpret it as a calldata pointer resulting in
calldata reads at the wrong location and potential stack corruption. The stack corruption is caused by the fact that
calldata pointers can use two stack slots while memory pointers always only use one stack slot. This can in turn cause
problems for other variables and a wrong jump at the function return.

We assigned a severity of "very low" due to the fact that the bug was only introduced a week ago
and is very easy to detect through testing.

**Solution**: Upgrade to [Solidity 0.6.10](https://github.com/ethereum/solidity/releases/tag/v0.6.10), which does not perform the copy to memory.

**Example of affected code**

```solidity
library L {
    function helper(bytes calldata _x) internal pure returns (byte) {
        return _x[0];
    }
}

contract C {
    using L for bytes;
    function fun(bytes calldata _x) public pure returns (byte) {
        // This would incorrectly copy ``_x`` from calldata
        // to memory and then call L.helper with this memory array,
        // but the function expects a calldata array and thus
        // reads the data from the wrong calldata offset.
        return _x.helper();
    }
}
```
We thank Luis from Gelato ([@gitpusha](https://github.com/gitpusha)) for reporting the bug!

## Notable New Features 

### Error Codes

A notable new feature is the introduction of error codes. In Standard JSON input/output mode
(``--standard-json``) the error codes will always be available under the property ``"errorCode"``.
In the command-line interface, you can enable error codes via the switch ``--error-codes``.

Two errors that are essentially about the same thing but contain different error messages, e.g. when the error concerns two different types, will use the same error code.

Error codes are intended to be reasonably stable across releases.

We hope that this allows improved analysis of smart contracts without complicated and fragile checks on the error message.

**Example:**

The following source code generates three error messages:

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.6.10;
contract C {
  uint8 x = 1000;
  function f(uint _y) public {
    x = _y;
    x.g();
  }
}
```

```
# solc --error-codes c.sol
Error (7407): Type int_const 1000 is not implicitly convertible to expected type uint8.
 --> c.sol:5:13:
  |
5 |   uint8 x = 1000;
  |             ^^^^
Error (7407): Type uint256 is not implicitly convertible to expected type uint8.
 --> c.sol:7:9:
  |
7 |     x = _y;
  |         ^^
Error (9582): Member "g" not found or not visible after argument-dependent lookup in uint8.
 --> c.sol:8:5:
  |
8 |     x.g();
  |     ^^^
```

Note that the errors concerning invalid type conversions use the same code even though the text is different.
The error about the missing member uses a different code, though.

We currently do not plan to create a complete list of all error codes. If you would like to look them up, please take a look at the source code of the compiler.

Error codes are currently always four-digit decimals with no structure in the number assignments.

## Full Changelog

### New Features

 * **Commandline Interface**: Re-group help screen.
 * **Output compilation error codes** in standard-json and when using ``--error-codes``.
 * **Yul**: Raise warning for switch statements that only have a default and no other cases.

### Bugfixes

* **SMTChecker**: Fix internal error when encoding tuples of tuples.
* **SMTChecker**: Fix aliasing soundness after pushing to an array pointer.
* **Type system**: Fix internal compiler error on calling externally a function that returns variables with calldata location.
* **Type system**: Fix bug where a bound function was not found if ``using for`` is applied to explicit reference types.

A big thank you to all contributors who helped make this release possible!

Download the new version of Solidity [here](https://github.com/ethereum/solidity/releases/tag/v0.6.10).
