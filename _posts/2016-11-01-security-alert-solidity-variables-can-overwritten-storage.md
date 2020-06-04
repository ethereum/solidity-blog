---
layout: post
published: true
title: 'Security Alert: Variables can be overwritten in storage'
date: '2016-11-01'
author: Christian Reitwiessner
category: Security Alerts
---
_This post was originally published on the [Ethereum blog](https://blog.ethereum.org/2016/11/01/security-alert-solidity-variables-can-overwritten-storage/)._

**Summary:** In some situations, variables can overwrite other variables in storage.

**Affected Solidity compiler versions: **0.1.6 to 0.4.3 (including 0.4.4 pre-release versions)

**Detailed description:**

Storage variables that are smaller than 256 bits are packed together into the same 256 bit slot if they can fit. If a value larger than what is allowed by the type is assigned to the first variable, that value will overwrite the second variable.

This means if an attacker can cause an overflow in the value of the first variable, then the second variable can be modified. Creating an overflow in the first variable is possible using arithmetics or by directly passing in a value from the call data (values in call data are aligned to 32 bytes, and padding is neither verified nor enforced).

Contracts that only use the types listed below for state variables are **not** affected. Arrays, mappings and structs (based on those following types) are also **not** affected:
- signed integers, including sizes smaller than 256 bits
- bytesNN types, including sizes smaller than 256 bits
- unsigned integers (uint) of 256 bits

Contracts with types smaller than 256 bits that are never next to each other (note that state variables of base contracts are "pulled in") are **not** affected.

The Ethereum multisignature wallet contract is **not** affected.
Note that addresses take up 160 bits, so contracts that only use addresses and 256-bit types are safe. Additionally, addresses and booleans are almost never manipulated via arithmetic operations in practice, so contracts using only addresses, booleans and 256 bit types should also be safe.

The following contracts may be affected:
Contracts containing two or more contiguous state variables where the sum of their sizes is less than 256 bits and the first state variable is not a signed integer and not of bytesNN type.

Types smaller than 256 bits include:
bool, enums, uint8, ..., uint248, int8, ..., int248, address, any contract type

**Recommended action:**
- Recompile contracts that have not yet been deployed using at least Solidity release 0.4.4 (not the pre-release or nightly version).
- Deactivate, remove funds from, or upgrade already deployed contracts.

This vulnerability was found by [github.com/catageek](https://github.com/catageek): [https://github.com/ethereum/solidity/issues/1306](https://github.com/ethereum/solidity/issues/1306)
