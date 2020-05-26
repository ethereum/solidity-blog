---
layout: post
published: true
title: 'Solidity 0.6.x features: Array Slices'
date: '2020-05-26'
author: Solidity Team
category: Explainers
---

Starting from [version 0.6.0](https://github.com/ethereum/solidity/releases/tag/v0.6.0), Solidity supports array slices. 
[Array slices](https://solidity.readthedocs.io/en/v0.6.8/types.html#array-slices) are handy when you want to reference a 
contiguous portion of an array but do not want to perform a full copy of that portion. For now, array slices are only 
supported for calldata arrays.

## How to use Array Slices

The expression ``x[start:end]`` references a portion of the calldata array ``x`` starting at index ``start`` and ending just 
before index ``end``. 

Both ``start`` and ``end`` are optional. If not provided, ``start`` defaults to ``0`` and ``end`` defaults to the length of 
the respective array.

Please note that no copy from calldata to memory is performed.

You can use the slice syntax on variables, but also with ``msg.data``.

Array slices can be especially useful in combination with ``abi.decode`` in the 
[fallback function](https://solidity.ethereum.org/2020/03/26/fallback-receive-split/):


```solidity
pragma solidity ^0.6.5;

interface OtherContract {
    function changeOwner(address) external;
}

contract Proxy {
    OtherContract implementation;
    address badOwner;
    fallback() external {
        if (msg.sig == OtherContract.changeOwner.selector) {
            // Here, abi.decode directly operates on the calldata array slice.
            address newOwner = abi.decode(msg.data[4:], (address));
            require(newOwner != badOwner);
        }
        (bool success,) = address(implementation).delegatecall(msg.data);
        require(success);
    }
}
```
## How Array Slices Work Internally

Calldata arrays have always been represented by two values: A start pointer and a length value. This is exactly the same 
representation we use for calldata array slices.


## Limitations and Future Plans

Array slices support index access and ``.length`` just like arrays, but do not support most copy operations. 

As of now, array slices are only implemented for calldata.
