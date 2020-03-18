---
layout: post
title:  "Solidity 0.6.x features: fallback and receive functions"
date:   2020-03-23
author: Elena Gesheva
---

In versions of Solidity before 0.6.x, developers typically used the [fallback function](https://solidity.readthedocs.io/en/v0.5.15/contracts.html#fallback-function) to handle logic in two scenarios:

 - contract received ether and no data
 - contract received data but no function matched the function called

The main use case of the pre-0.6.x fallback function is to receive ether and react
to it, a typical pattern used by token-style contracts to reject transfers, emit
events or forward the ether. The function executes when a contract is called
without any data e.g. via `.send()` or `.transfer()` functions. The 0.5.x
syntax is:

    pragma solidity ^0.5.0;
    contract Charity {
        function() external payable {
            // React to receiving ether
        }
    }

The second use case was made popular by the "delegate proxy" pattern
for implementing upgradable contracts. It has a simple Proxy contract that
only declares a fallback function. The fallback function is invoked when no
function in the contract matches the function identifier in the call data.
This permits the "delegate proxy" pattern where functionality is implemented
outside the called contract. Sample implementation:

    pragma solidity ^0.5.0;
    contract DelegateProxy {
        address internal implementation;
        
        function() external payable {
            address addr = implementation;
        
            assembly {
                calldatacopy(0, 0, calldatasize())
                let result := delegatecall(gas(), addr, 0, calldatasize(), 0, 0)
                returndatacopy(0, 0, returndatasize())
                switch result
                case 0 { revert(0, returndatasize()) }
                default { return(0, returndatasize()) }
            }
        }
    }

Calling the contract uses assembly code that we won't go into detail here,
but you can read more in [Zeppelin's documentation](https://docs.openzeppelin.com/upgrades/2.6/proxies).

## Splitting the fallback function

We realised that the dual-purpose of the function confused developers, which
could lead to potential security problems. For example, a developer typically implements a fallback function, expecting only ether transfers to call it, but
it is also called when a function is missing from a contract. And confusingly,
as this was expected behaviour, no error was reported. Example implementation
demonstrating this confusing behaviour is below.

    pragma solidity ^0.5.0;
    contract Charity {
        mapping (address => uint256) public donations;
    
        function processDonation(address user) external payable {
            donations[user] += msg.value;
        }
    }
    contract Receiver {
        event ValueReceived(address user, uint amount);
    
        function() external payable {
            emit ValueReceived(msg.sender, msg.value);
        }
    }
    contract CharitySplitter {
        function donate(Charity charity) external payable {
            charity.processDonation.value(msg.value)(msg.sender);
        }
    }

When calling `CharitySplitter.donate()` with a charity contract address,
its `processDonation` function is correctly invoked to process the donation
as expected. However if by mistake the `Receiver` contract address is passed,
its fallback function ends up being called, swallowing the sent value.

    const goodCharity = await Charity.new();
    const receiver = await Receiver.new();
    const badCharity = await Charity.at(receiver.address);
    const charitySplitter = await CharitySplitter.new();
    // Charity.processDonation is executed successfully and 10 wei donation is recorded
    await charitySplitter.donate(goodCharity, { value: 10 });
    // Triggers the underlying Receiver fallback function
    // 10 wei is acquired and ValueReceived event emitted 
    await charitySplitter.donate(badCharity, { value: 10 });

Since the EVM is untyped, Solidity is unable to check the actual type of a
contract from its address and has to rely on what the user supplies. The
function signatures also do not provide a perfect solution against type
confusion but can work in many cases.

This is why in version 0.6.x, the fallback function was split into two
separate functions:

 - [receive()](https://solidity.readthedocs.io/en/latest/contracts.html#receive-ether-function) `external payable` — for empty calldata (and any value)
 - [fallback()](https://solidity.readthedocs.io/en/latest/contracts.html#fallback-function) `external payable` — when no other function matches (not even the receive function). Optionally `payable`.

This separation provides an alternative to the fallback function for
contracts that want to receive plain ether.

### receive()

A contract can now have only one `receive` function, declared with the syntax:
`receive() external payable {…}` (without the `function` keyword).

It executes on calls to the contract with no data (`calldata`), e.g. calls made
via `send()` or `transfer()`.

The function cannot have arguments, cannot return anything and must have
`external` visibility and `payable` state mutability. To replicate the example
above under 0.6.0, use the following code:

    pragma solidity ^0.5.0;
    contract Charity {
        receive() external payable {
            // React to receiving ether
        }
    }

### fallback()

The fallback function now has a different syntax, declared using
`fallback() external [payable] {…}` (without the `function` keyword). This
function cannot have arguments, cannot return anything and must have
`external` visibility. The fallback function always receives data, but to
also receive Ether, you should mark it as `payable`. To replicate the example
above under 0.6.0, use the following code:

    pragma solidity ^0.6.0;

    contract DelegateProxy {
        address internal implementation;
        
        fallback() external payable {
            address addr = implementation;
        
            assembly {
                calldatacopy(0, 0, calldatasize())
                let result := delegatecall(gas(), addr, 0, calldatasize(), 0, 0)
                returndatacopy(0, 0, returndatasize())
                switch result
                case 0 { revert(0, returndatasize()) }
                default { return(0, returndatasize()) }
            }
        }
    }

### Migrated and fixed sample contract

Thus we translate the problematic contract to v0.6.x , having it declare a
`receive()` function which only accepts incoming ether without data and
avoids the type confusion that led to the loss of value demonstrated above.

    pragma solidity ^0.6.0;
    contract Charity {
        mapping (address => uint256) public donations;
        
        function processDonation(address user) external payable {
            donations[user] += msg.value;
        }
    }
    contract Receiver {
        event ValueReceived(address user, uint amount);
        
        receive() external payable {
            emit ValueReceived(msg.sender, msg.value);
        }
    }

    contract CharitySplitter {
        function donate(Charity charity) external payable {
            charity.processDonation{value:msg.value}(msg.sender);
        }
    }

The calls to the fixed contract will now revert when called with the `Receiver` contract address fails:

    // The following call now reverts
    await charitySplitter.donate(badCharity, { value: 10 });

We hope you find the logical division of the fallback function clearer for your design, and welcome any feedback you have on the new syntax.