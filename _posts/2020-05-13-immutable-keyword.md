---
layout: post
published: true
title: 'Solidity 0.6.x features: Saving Storage Costs with Immutables'
date: '2020-05-13'
author: Daniel Kirchner
category: Explainers
---

With [version 0.6.5](https://github.com/ethereum/solidity/releases/tag/v0.6.5), Solidity introduced the 
[``immutable`` keyword](https://solidity.readthedocs.io/en/v0.6.7/contracts.html?highlight=immutable#constant-and-immutable-state-variables) 
for state variables. Immutable state variables can only be assigned during contract creation, but will remain 
constant throughout the life-time of a deployed contract.
The big advantage of immutables is that reading them is significantly cheaper than reading from regular state variables, 
since immutables will not be stored in storage, but their values will be directly inserted into the runtime code.

## How to use Immutables

Immutable state variables can be declared using the ``immutable`` keyword. They cannot be read during contract creation, 
but either have to be written to exactly once in the constructor or initialized directly in their declaration. 
Runtime code can only read immutables, but not write to them.
Here is an example of ``immutable`` in action:
```solidity
pragma solidity >=0.6.5;
contract SimpleDonationManager {
    uint256 immutable minDonation = 42;
    uint256 immutable maxDonation = calcMaxDonation();
    address immutable owner;
    constructor() public {
        owner = msg.sender;
        // Error: Immutables cannot be read in constructor.
        // assert(minDonation <= maxDonation);
        // Error: Immutables can only be assigned once.
        // owner = address(0);
    }
    function withdraw(uint256 amount) public {
        require(msg.sender == owner); // Requires no sload!
        msg.sender.transfer(amount);
    }
    receive() external payable {
        require(msg.value >= minDonation && msg.value <= maxDonation);
    }
    function calcMaxDonation() internal pure returns (uint256 max) {
        max = 10000 * 42;
        // Error: Cannot read from Immutables in construction context.
        // max = 10000 * minDonation;
    }
    // Error: cannot write to Immutables from runtime context.
    // function changeOwner(address newOwner) external {
    //     owner = newOwner;
    // }
}

```

## How Immutables Work

While regular state variables are stored at a fixed offset in storage, this is not the case for immutables. Instead, 
immutables are stored directly in the deployed bytecode, i.e. the runtime code will contain placeholders for every 
read of an immutable and the creation code will insert the values of the immutables into these placeholders. 
Effectively, this means that reading from an immutable at runtime will reduce to a plain ``PUSH32`` opcode.


While this mechanism may change in the future, currently, during contract creation, a dedicated memory area is reserved 
for the values of immutables. The constructor code can then store the intended values for the immutables in that memory area.
The compiler-generated part of the creation code that is executed after the constructor and returns the contract's 
runtime code will read back the values of the immutables and insert them into all occurrences in the runtime bytecode.


## Limitations and Future Plans

So far, only immutables of value types are supported. However, this restriction might be relaxed in the future. 
This will likely work by storing an offset into the runtime code using the same mechanism as for immutables of value-type 
and using ``codecopy`` to read the contents of the immutable of reference type to memory.
Possibly, it may also be allowed to read from immutables in the creation context, in case the compiler can be sure that the 
immutable will already have been assigned before the read.
