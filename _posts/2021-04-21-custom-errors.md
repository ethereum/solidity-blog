---
layout: post
published: true
title:  Custom Errors in Solidity
date:   2021-04-21
author: Solidity Team
category: Explainers
---

Starting from [Solidity v0.8.4](https://github.com/ethereum/solidity/releases/tag/v0.8.4), there is
a convenient and gas-efficient way to explain to users why an operation failed through the use of
custom errors. Until now, you could already use strings to give more information about failures
(e.g., `revert("Insufficient funds.");`), but they are rather expensive, especially when it comes to
deploy cost, and it is difficult to use dynamic information in them.

Custom errors are defined using the ``error`` statement, which can be used inside and outside of
contracts (including interfaces and libraries).

## Example

The following contract shows an example usage of an error:

```solidity
// SPDX-License-Identifier: GPL-3.0
pragma solidity ^0.8.4;

error Unauthorized();

contract VendingMachine {
    address payable owner = payable(msg.sender);

    function withdraw() public {
        if (msg.sender != owner)
            revert Unauthorized();

        owner.transfer(address(this).balance);
    }
    // ...
}
```

The syntax of errors is similar to that of
[events](https://docs.soliditylang.org/en/latest/contracts.html#events). They have to be used
together with the [revert
statement](https://docs.soliditylang.org/en/latest/control-structures.html#revert-statement) which
causes all changes in the current call to be reverted and passes the error data back to the caller.
Using errors together with `require` is not yet supported (see below).


## Errors with Parameters

It is also possible to have errors that take parameters. For example,

```solidity
// SPDX-License-Identifier: GPL-3.0
pragma solidity ^0.8.4;

/// Insufficient balance for transfer. Needed `required` but only
/// `available` available.
/// @param available balance available.
/// @param required requested amount to transfer.
error InsufficientBalance(uint256 available, uint256 required);

contract TestToken {
    mapping(address => uint) balance;
    function transfer(address to, uint256 amount) public {
        if (amount > balance[msg.sender])
            // Error call using named parameters. Equivalent to
            // revert InsufficientBalance(balance[msg.sender], amount);
            revert InsufficientBalance({
                available: balance[msg.sender],
                required: amount
            });
        balance[msg.sender] -= amount;
        balance[to] += amount;
    }
    // ...
}
```

The error data would be encoded identically as the ABI encoding for function calls, i.e.,
``abi.encodeWithSignature("InsufficientBalance(uint256,uint256)", balance[msg.sender], amount)``.

We hope that frameworks will provide direct support for custom errors. The following is an example on
decoding error data using the current version of [ethers.js](https://docs.ethers.io/v5/):

```javascript
import { ethers } from "ethers";

// As a workaround, we have a function with the
// same name and parameters as the error in the abi.
const abi = [
    "function InsufficientBalance(uint256 available, uint256 required)"
];

const interface = new ethers.utils.Interface(abi);
const error_data =
    "0xcf479181000000000000000000000000000000000000" +
    "0000000000000000000000000100000000000000000000" +
    "0000000000000000000000000000000000000100000000";

const decoded = interface.decodeFunctionData(
    interface.functions["InsufficientBalance(uint256,uint256)"],
    error_data
);
// Contents of decoded:
// [
//   BigNumber { _hex: '0x0100', _isBigNumber: true },
//   BigNumber { _hex: '0x0100000000', _isBigNumber: true },
//   available: BigNumber { _hex: '0x0100', _isBigNumber: true },
//   required: BigNumber { _hex: '0x0100000000', _isBigNumber: true }
// ]
console.log(
    "Insufficient balance for transfer. " +
    `Needed ${decoded.required.toString()} but only ` +
    `${decoded.available.toString()} available.`
);
// Insufficient balance for transfer. Needed 4294967296 but only 256 available.
```

The compiler includes all errors that a contract can emit in the contract's
[ABI-JSON](https://docs.soliditylang.org/en/latest/abi-spec.html?#json). Note that this will not
include errors forwarded through external calls. Similarly, developers can provide
[NatSpec](https://docs.soliditylang.org/en/latest/natspec-format.html) documentation for errors
which would then be part of the user and developer documentation and can explain the error in much more detail at no cost.

Please be careful when using error data since its origin is not tracked. The error data by default bubbles up through the chain
of external calls, which means that a contract may forward an error not defined in any of the
contracts it calls directly. Furthermore, any contract can fake any error by returning data that
matches an error signature, even if the error is not defined anywhere.

Currently, there is no convenient way to catch errors in Solidity, but this is planned, and progress
can be tracked in issue [#11278](https://github.com/ethereum/solidity/issues/11278). Also,
`require(condition, "error message")` should be translated to `if (!condition) revert
CustomError()`.

## Errors in Depth

In the [first example](#example), `revert Unauthorized();` is equivalent to the following Yul code:

```
let free_mem_ptr := mload(64)
mstore(free_mem_ptr, 0x82b4290000000000000000000000000000000000000000000000000000000000)
revert(free_mem_ptr, 4)
```

This is the same as the ABI encoding of a function call with the name `Unauthorized()`. Here
`0x82b42900` is the 'selector' for `Unauthorized()`. In contrast, using a revert string, i.e.,
`revert("Unauthorized");` leads to the following Yul code:

```
let free_mem_ptr := mload(64)
mstore(free_mem_ptr, 0x08c379a000000000000000000000000000000000000000000000000000000000)
mstore(add(free_mem_ptr, 4), 32)
mstore(add(free_mem_ptr, 36), 12)
mstore(add(free_mem_ptr, 68), "Unauthorized")
revert(free_mem_ptr, 100)
```

Here `0x08c379a0` is the 'selector' of `Error(string)`. In this comparison, one can see that custom
errors decrease both deploy and runtime gas costs. Note that runtime gas cost is only relevant when
the revert condition is met.
