---
layout: post
published: true
title: 'User Defined Value Types Bug'
date: '2021-09-29'
author: Solidity Team
category: Security Alerts
---

On September 28th, 2021, Harry Altman ([@haltman-at](https://github.com/haltman-at)) of Truffle
discovered a bug in user defined value types.

The bug has no influence on the correctness of Solidity contracts, but contracts compiled with
Solidity 0.8.8 that use the new feature are unnecessarily wasteful and might have problems with
tooling or contract upgrades.

The bug exists only in Solidity 0.8.8 and is fixed in 0.8.9.

We assigned the bug a severity of "very low".

## Storage Layout of User Defined Value Types

The compiler did not correctly compute the storage layout of user defined value types based on types
that are shorter than 32 bytes. It would always use a full storage slot for these types, even if the
underlying type is shorter. A user defined value type in 0.8.8 should have the same data
representation as its underlying type in all aspects with the exception of storage layout.

Types shorter than 32 bytes can share a storage slot with other values if there is enough space. For
example:

```solidity
pragma solidity =0.8.8;

type MyInt128 is int128;

contract WastefulStorageLayout {
    // slot 0
    MyInt128 a;
    // slot 1
    MyInt128 b;
}

contract PackedStorageLayout {
    // slot 0
    int128 a;
    // slot 0
    int128 b;
}
```

You can verify this by running `solc --storage-layout Contract.sol`.

## Impact

Solidity tries to guarantee that the storage layout of contracts remains consistent across releases.
This allows for certain kind of upgrade patterns to exist that may use different Solidity versions.
Since user defined value types should behave exactly as the underlying type, Solidity 0.8.8
introduced inconsistent storage layout for contracts with short user defined value types as state
variables. The Solidity 0.8.9 release corrects the layout. Be extra careful when upgrading from a
contract that was compiled with 0.8.8, and that uses short user defined value types, to a version
higher than 0.8.9.

