# Solidity 0.6.x features: Fallback functions

In versions of Solidity before 0.6.x, developers typically added a [fallback function](https://solidity.readthedocs.io/en/v0.5.15/contracts.html#fallback-function) as a function that a contract executed
if a contract received ether, but no data, or if it received data, but no other
functions matched the function called.

## 0.5.x fallback function for receiving ether and no data

The first use case of the fallback function is to receive ether and react to it,
a typical pattern used by token-style contracts. For example, to reject a transfer,
emit an event, or forward the ether. The function is called when a contract is
called without any data and no specific function such as `.send()` or `.transfer()` function.
The 0.5.x syntax is:

```solidity
pragma solidity ^0.5.0;

contract Receiver {
    function() external payable {
        // React to ether
    }
}
```

This use of the fallback function uses simple and clear code and is reasonably safe.

## 0.5.x fallback function for proxying contracts

The second use led to the use of the common "proxy contract" pattern for modularity
and upgradability of deployed smart contracts. The exact implementation details can
vary, but typically take the form of something like the below:

```solidity
pragma solidity ^0.5.0;

contract Proxy {
    address internal proxied;
 
    function () payable external {
        address addr = proxied;
 
        assembly {
            calldatacopy(0, 0, calldatasize)
            let result := delegatecall(gas, addr, 0, calldatasize, 0, 0)
            returndatacopy(0, 0, returndatasize)
            switch result
            case 0 { revert(0, returndatasize) }
            default { return(0, returndatasize) }
        }
    }
}
```

A contract developer typically designs a dapp to call the proxied contract with an address.
As there is only a fallback function present, it takes the address passed and calls the functional contract.
Calling the contract uses assembly code that we won't go into detail of here,
but [you can read more in Zeppelin's documentation](https://docs.openzeppelin.com/upgrades/2.6/proxies).
Because of this complexity, this use of the fallback function is prone to bugs or security issues.

## Splitting the fallback function

We realised that the dual-purpose of the function confused developers, and at worst,
potential security problems. For example, a developer typically implements a fallback function,
expecting only ether transfers to call it, but it is also called when a function is missing from a contract.
And confusingly, as this was expected behaviour, no error is reported.

Because of this, version 0.6.x splits fallback functions into two different functions: a reworking 
of `[fallback()](https://solidity.readthedocs.io/en/latest/contracts.html#fallback-function)` and 
the new `[receive()](https://solidity.readthedocs.io/en/latest/contracts.html#receive-ether-function)` function.

### The new fallback function

The fallback function has different syntax, declared using `fallback () external [payable] {…}` 
(without the `function` keyword). This function cannot have arguments, cannot return anything and 
must have `external` visibility. The fallback function always receives data, but to also receive 
Ether, mark it as `payable`. To replicate the example above under 0.6.0, use the following code:

```solidity
pragma solidity ^0.6.0;

contract Proxy {
    address internal proxied;

    fallback() external {
        address addr = proxied;
        assembly {
            calldatacopy(0, 0, calldatasize)
            let result := delegatecall(gas, addr, 0, calldatasize, 0, 0)

            returndatacopy(0, 0, returndatasize)

            switch result
            case 0 { revert(0, returndatasize) }
            default { return(0, returndatasize) }
        }
    }
}
```

### The new receive ether function

A contract can also have only one `receive` function, executed on a call to the contract with no 
data (`calldata`), but plain ether transactions. Typically these are functions such as `send()` or 
`transfer()`, but if none of these functions exists, the contract executes the `receive` function 
instead. You declare the function with the syntax `receive() external payable {…}` (without the 
`function` keyword). The function cannot have arguments, cannot return anything and must have 
`external` visibility and `payable` state mutability. To replicate the example above under 0.6.0,
use the following code:

```solidity
receive() external payable {
    // React to ether
}
```

## Fit for purpose

And that's about it. We hope you find the logical dividing of the fallback function 
clearer for your dapp design, and we welcome any feedback you have on the new syntax.
