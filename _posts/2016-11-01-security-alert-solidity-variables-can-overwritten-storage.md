---
id: 3363
title: 'Security Alert: Variables can be overwritten in storage'
date: 2016-11-01T08:40:44+00:00
author: Christian Reitwiessner
layout: post
guid: https://blog.ethereum.org/?p=3363
permalink: /2016/11/01/security-alert-solidity-variables-can-overwritten-storage/
---
<strong>Summary:</strong> In some situations, variables can overwrite other variables in storage.

<strong>Affected Solidity compiler versions: </strong>0.1.6 to 0.4.3 (including 0.4.4 pre-release versions)

<strong>Detailed description:</strong>

Storage variables that are smaller than 256 bits are packed together into the same 256 bit slot if they can fit. If a value larger than what is allowed by the type is assigned to the first variable, that value will overwrite the second variable.

This means if an attacker can cause an overflow in the value of the first variable, then the second variable can be modified. Creating an overflow in the first variable is possible using arithmetics or by directly passing in a value from the call data (values in call data are aligned to 32 bytes, and padding is neither verified nor enforced).

Contracts that only use the types listed below for state variables are <strong>not</strong> affected. Arrays, mappings and structs (based on those following types) are also <strong>not</strong> affected:

- signed integers, including sizes smaller than 256 bits
- bytesNN types, including sizes smaller than 256 bits
- unsigned integers (uint) of 256 bits

Contracts with types smaller than 256 bits that are never next to each other (note that state variables of base contracts are "pulled in") are <strong>not</strong> affected.

The Ethereum multisignature wallet contract is <strong>not</strong> affected.
Note that addresses take up 160 bits, so contracts that only use addresses and 256-bit types are safe. Additionally, addresses and booleans are almost never manipulated via arithmetic operations in practice, so contracts using only addresses, booleans and 256 bit types should also be safe.

The following contracts may be affected:
Contracts containing two or more contiguous state variables where the sum of their sizes is less than 256 bits and the first state variable is not a signed integer and not of bytesNN type.

Types smaller than 256 bits include:
bool, enums, uint8, ..., uint248, int8, ..., int248, address, any contract type

<strong>Recommended action:</strong>
<ul>
 	<li>Recompile contracts that have not yet been deployed using at least Solidity release 0.4.4 (not the pre-release or nightly version).</li>
 	<li>Deactivate, remove funds from, or upgrade already deployed contracts.</li>
</ul>
This vulnerability was found by [github.com/catageek](https://github.com/catageek): [https://github.com/ethereum/solidity/issues/1306](https://github.com/ethereum/solidity/issues/1306)
