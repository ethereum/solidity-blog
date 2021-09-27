---
layout: post
published: true
title:  User Defined Value Types in Solidity
date:   2021-09-27
author: Solidity Team
category: Explainers
---

[Solidity v0.8.8](https://github.com/ethereum/solidity/releases/tag/v0.8.8) introduces [user defined
value types](https://docs.soliditylang.org/en/latest/types.html?#user-defined-value-types) as a
means to create zero-cost abstractions over an elementary value type that also increases type safety
and improves readability.

## Motivation

A problem with primitive value types is that they are not very descriptive: they only specify how
the data is stored and not how it should be interpreted. For example, one may want to use `uint128`
to store the price of some object as well as the quantity available. It is quite useful to have
stricter type rules to avoid intermingling of the two different concepts. For example, one may want
to disallow assigning a quantity to a price or vice versa.

One option for solving this issue is by using
[structs](https://docs.soliditylang.org/en/latest/types.html#structs). For example, price and
quantity can be abstracted as structs as follows:

```solidity
struct Price { uint128 price; }
struct Quantity { uint128 quantity; }

function toPrice(uint128 price) returns(Price memory) {
    return Price(price);
}
function fromPrice(Price memory price) returns(uint128) {
   return price.price;
}
function toQuantity(uint128 quantity) returns(Quantity memory) {
    return Quantity(quantity);
}
function fromQuantity(Quantity memory quantity) returns(uint128) {
    return quantity.quantity;
}
```

However, a [struct](https://docs.soliditylang.org/en/latest/types.html?#structs) is a reference type
and therefore always points to a value in `memory`, `calldata` or `storage`. This means that the
above abstraction has a runtime overhead, i.e., additional gas when compared to using just `uint128`
to represent the underlying value. In particular, the functions `toPrice` and `toQuantity` involve
storing the value in memory. Similarly, the functions `fromPrice` and `fromQuantity` read the
respective value from memory. Together, these functions pass the value from `stack -> memory ->
stack` which wastes memory and incurs a runtime cost. This issue is solved by user defined value
types, which are abstractions of elementary value types (such as `uint8` or `address`), without any
additional runtime overhead.

## Syntax for User Defined Value Types

A user defined value type is defined using `type C is V;`, where `C` is the name of the newly
introduced type and `V` has to be a built-in value type (the "underlying type"). They can be defined
inside or outside contracts (including libraries and interfaces). The function `C.wrap` is used to
convert from the underlying type to the custom type. Similarly, the function `C.unwrap` is used to
convert from the custom type to the underlying type.

Going back to the problem from the [motivation](#motivation) section, one can replace the structs by:

```solidity
pragma solidity ^0.8.8;

type Price is uint128;
type Quantity is uint128;
```

The functions `toPrice` and `toQuantity` can be replaced by `Price.wrap` and `Quantity.wrap`
respectively. Similarly, the functions `fromPrice` and `fromQuantity` can be replaced by
`Price.unwrap` and `Quantity.unwrap` respectively.

The data-representation of the values of such types are inherited from the underlying type and the
underlying type is also used in the ABI. This means that the following two `transfer` functions
would be identical, i.e., they have the same [function
selector](https://docs.soliditylang.org/en/latest/abi-spec.html#function-selector) as well as the
same [ABI encoding and
decoding](https://docs.soliditylang.org/en/latest/abi-spec.html#mapping-solidity-to-abi-types). This
allows using user defined value types in a backwards compatible way.

```solidity
pragma solidity ^0.8.8;

type Decimal18 is uint256;

interface MinimalERC20 {
    function transfer(address to, Decimal18 value) external;
}

interface AnotherMinimalERC20 {
    function transfer(address to, uint256 value) external;
}
```

Notice in the above example, how the user defined type `Decimal18` makes it clear that a value is
supposed to represent a number with 18 decimals.

## Example

The following example illustrates a custom type `UFixed` representing a decimal fixed point type
with 18 decimals and a minimal library to do arithmetic operations on the type.

```solidity
// SPDX-License-Identifier: GPL-3.0
pragma solidity ^0.8.8;

// Represent a 18 decimal, 256 bit wide fixed point type
// using a user defined value type.
type UFixed is uint256;

/// A minimal library to do fixed point operations on UFixed.
library FixedMath {
    uint constant multiplier = 10**18;

    /// Adds two UFixed numbers. Reverts on overflow, 
    /// relying on checked arithmetic on uint256.
    function add(UFixed a, UFixed b) internal pure returns (UFixed) {
        return UFixed.wrap(UFixed.unwrap(a) + UFixed.unwrap(b));
    }
    /// Multiplies UFixed and uint256. Reverts on overflow,
    /// relying on checked arithmetic on uint256.
    function mul(UFixed a, uint256 b) internal pure returns (UFixed) {
        return UFixed.wrap(UFixed.unwrap(a) * b);
    }
    /// Take the floor of a UFixed number.
    /// @return the largest integer that does not exceed `a`.
    function floor(UFixed a) internal pure returns (uint256) {
        return UFixed.unwrap(a) / multiplier;
    }
    /// Turns a uint256 into a UFixed of the same value.
    /// Reverts if the integer is too large.
    function toUFixed(uint256 a) internal pure returns (UFixed) {
        return UFixed.wrap(a * multiplier);
    }
}
```

Notice how `UFixed.wrap` and `FixedMath.toUFixed` have the same signature but perform two very
different operations: The `UFixed.wrap` function returns a `UFixed` that has the same data
representation as the input, whereas `toUFixed` returns a `UFixed` that has the same numerical
value. One can allow some form of type-encapsulation by only using the `wrap` and `unwrap` functions
in the file that defines the type.

## Operators and Type Rules

Explicit and implicit conversions to and from other types are disallowed.

Currently, no operators are defined for user defined value types. In particular, even the operator
`==` is not defined. However, allowing operators is currently being discussed. To give a short
outlook on the applications, one may want to introduce a new integer type that always does wrapping
arithmetic as follows:

```solidity
/// Proposal on defining operators on user defined value types
/// Note: this does not fully compile on Solidity 0.8.8; only a concept.

type UncheckedInt8 is int8;

function add(UncheckedInt8 a, UncheckedInt8 b) pure returns(UncheckedInt8) {
    unchecked {
        return UncheckedInt8.wrap(UncheckedInt8.unwrap(a) + UncheckedInt8.unwrap(b));
    }
}
function addInt(UncheckedInt8 a, uint b) pure returns(UncheckedInt8) {
    unchecked {
        return UncheckedInt8.wrap(UncheckedInt8.unwrap(a) + b);
    }
}

using {add as +, addInt as +} for UncheckedInt8;

contract MockOperator {
    UncheckedInt8 x;
    function increment() external {
        // This would not revert on overflow when x = 127
        x = x + 1;
    }
    function add(UncheckedInt8 y) external {
        // Similarly, this would also not revert on overflow.
        x = x + y;
    }
}
```

You can join or follow this discussion in the [Solidity
forum](https://forum.soliditylang.org/t/user-defined-types-and-operators/456) and [issue
#11969](https://github.com/ethereum/solidity/issues/11969). Also, you can join or follow the
discussion about allowing the constructor syntax for user defined value types in the [issue
#11953](https://github.com/ethereum/solidity/issues/11953).
