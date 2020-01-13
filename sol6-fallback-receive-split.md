# Solidity 0.6.x features: Fallback functions split

In versions of Solidity before 0.6.x, developers typically added a [fallback function](https://solidity.readthedocs.io/en/v0.5.15/contracts.html#fallback-function) as a function that a contract executed if no other functions matched the function called, or if a contract received ether, but no data.

However, we realised that the dual-purpose of the function caused confusion to developers, and at worst, potential security problems.

For example, a developer typically implements a fallback function, expecting only ether transfers to call it, but it is also called when a function is missing from a contract. Even worse, as this was expected behaviour, no error is reported.

```solidity
// TODO: EXAMPLE and explanation
```

Because of this, version 0.6.x splits fallback functions into two different functions: a reworking of `[fallback()](https://solidity.readthedocs.io/en/latest/contracts.html#fallback-function)` and the new `[receive()](https://solidity.readthedocs.io/en/latest/contracts.html#receive-ether-function)` function.

The fallback function has different syntax, declared using `fallback () external [payable] {…}` (without the `function` keyword). This function cannot have arguments, cannot return anything and must have `external` visibility. The fallback function always receives data, but to also receive Ether, mark it as `payable`.

A contract can also have only one receive function, executed on a call to the contract with no data (`calldata`), but plain ether transactions. Typically these are functions such as `send()` or `transfer()`, but if none of these functions exists, the contract executes the receive function instead. You declare the function with the syntax `receive() external payable {…}` (without the `function` keyword). The function cannot have arguments, cannot return anything and must have `external` visibility and `payable` state mutability.
