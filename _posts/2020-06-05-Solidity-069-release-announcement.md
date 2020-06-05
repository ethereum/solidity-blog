---
layout: post
published: true
title: 'Solidity 0.6.9 Release Announcement'
date: '2020-06-05'
author: Solidity Team
category: Releases
---

[Solidity v0.6.9](https://github.com/ethereum/solidity/releases/tag/v0.6.9) adds SMT-checking to solc-js,
allows ``calldata`` for all variables and provides a mechanism to specify an import directory.

Please note that the solc-js / soljson binary includes the Z3 SMT solver built-in, which causes an increase in the binary size.

## Notable New Features

### SMTChecker

The [SMTChecker](https://solidity.readthedocs.io/en/v0.6.8/security-considerations.html#formal-verification) is a component of the Solidity compiler which has been in development for quite some time now. 

Its aim is to bring formal verification methods  to the Solidity compiler. Solidity implements a formal verification approach based on SMT/Horn solving. The SMTChecker module automatically tries to prove that the code satisfies the specification given by `require/assert` statements. The SMTChecker also checks automatically for arithmetic underflow/overflow, trivial conditions,
unreachable code, and popping empty storage arrays.


So far, the SMTChecker has lacked an easy and convenient way to be used.
The difficulty lies in the fact that it requires an SMT solver to be compiled or linked into the compiler. To aid that, we are now shipping solc-js with the SMT solver Z3 compiled in. This means you can directly use the SMTChecker with everything
that uses solc-js - including [Remix](https://remix.ethereum.org/)!

Below, you find a simple example contract which shows an overflow check being reported. If you copy-paste it to Remix, it will show the error.

```solidity
// SPDX-License-Identifier: GPL-3.0
pragma solidity >=0.6.9;
pragma experimental SMTChecker;

contract Token {
  mapping(address => uint) balances;
  address owner;
  constructor () public { owner = msg.sender; }
  function transfer(address recipient, uint amount) public {
    // If we comment out the next line, SMTChecker
    // will complain about underflow.
    require(balances[msg.sender] >= amount);
    balances[msg.sender] -= amount;
    // SMTChecker detects a possible overflow in the next line
    // and even provides example inputs:
    // Warning: Overflow (resulting value larger than 2**256 - 1) happens here
    // for:
    // <result> = 2**256
    // amount = 1
    // balances[msg.sender] = 0
    // balances[recipient] = 2**256 - 1
    balances[recipient] += amount;
  }
  function mint(address recipient, uint amount) public {
      require(msg.sender == owner);
      balances[recipient] += amount;
  }
}
```

The component can also analyze more complex cases including possible call chains from external callers, as demonstrated in the following buggy state machine example:

```solidity
// SPDX-License-Identifier: GPL-3.0
pragma solidity >=0.6.9;
pragma experimental SMTChecker;

contract StateMachine {
    enum State { Init, End }
    State state;
    uint[] a;
    
    constructor() public {
        state = State.Init;
        a.push();
    }
    
    function move() public {
        require(state == State.Init);
        state = State.End;
        a.pop();
    }
    
    function reset() public {
        require(state == State.End);
        state = State.Init;
        a.pop();
    }
}
```

A sequence of 3 transactions (deployment, `move`, `reset`) causes an empty array pop, where SMTChecker then reports:
```solidity
browser/state_machine.sol:24:9: Warning: Empty array "pop" detected here.
        a.pop();
        ^-----^
```

Note that the SMTChecker makes several queries to the SMT solver, which might take some time since it is a computationally expensive procedure.

### Calldata Variables

You can now use `calldata` as the data location for reference variables everywhere, not only in external functions. Note that since the EVM does not allow modification of calldata, you cannot create new values in calldata or copy something to calldata.


```solidity
// SPDX-License-Identifier: GPL-3.0
pragma solidity >=0.6.9;

contract C {
  address[] owners;
  function addOwners(address[] calldata _newOwners) public {
    // We pass _newOwners on as a calldata array.
    checkUnique(_newOwners);
    for (uint i = 0; i < _newOwners.length; i++)
      owners.push(_newOwners[i]);
  }
  /// This internal function can iterate over the calldata array.
  /// Creating a copy in memory is not required.
  function checkUnique(address[] calldata _newOwners) internal pure {
    for (uint i = 0; i < _newOwners.length; i++)
      for (uint j = i + 1; i < _newOwners.length; j++)
        require(_newOwners[i] != _newOwners[i]);
  }
}
```

The benefit of using calldata variables is that it saves
a copy from calldata to memory and you can be sure that the
array or struct cannot be modified.

Usually, the ABI decoder performs range checks on the entire calldata when it is copied to memory to avoid malformed calldata from causing any problems.
Note that when using calldata variables for nested calldata structs or arrays those checks will instead only be performed when you access them.

### BasePath

With the introduction of the ``--base-path`` option to both
solc and solcjs, we hope that imports will be easier to use.

If you run ``solc --base-path ./contracts/ ./contracts/MainContract.sol``
and that file contains ``import "./OtherContract.sol";``, the Solidity
compiler will automatically search for ``./contracts/OtherContract.sol``
and import it.

While this sounds like a pretty trivial feature, we hope that you still like it!

Also note that if you use ``--base-path``, the base path itself will
not be included in any metadata artefacts, you will only get relative
paths starting from the base path.


## Full Changelog

### Language Features
 * Permit calldata location for all variables.
 * **NatSpec**: Support NatSpec comments on state variables.
 * **Yul**: EVM instruction `pc()` is marked deprecated and will be removed in the next breaking release.

### Compiler Features
 * **Build system**: Update the soljson.js build to emscripten 1.39.15 and boost 1.73.0 and include Z3 for integrated SMTChecker support without the callback mechanism.
 * **Build system**: Switch the emscripten build from the fastcomp backend to the upstream backend.
 * **Code Generator**: Do not introduce new internal source references for small compiler routines.
 * **Commandline Interface**: Adds new option ``--base-path PATH`` to use the given path as the root of the source tree (defaults to the root of the filesystem).
 * **SMTChecker**: Support array ``length``.
 * **SMTChecker**: Support array ``push`` and ``pop``.
 * **SMTChecker**: General support to BitVectors and the bitwise ``and`` operator.

## Bugfixes

This release comes with many minor, non-security relevant bugfixes. The correspoding bugs do not affect (the correctness of) the generated
bytecode. 

You can track all important, security-relevant bugs in the [Solidity bug json file](https://github.com/ethereum/solidity/blob/develop/docs/bugs.json).

 * **Code Generator**: Trigger proper unimplemented errors on certain array copy operations.
 * **Commandline Interface**: Fix internal error when using ``--assemble`` or ``--yul`` options with ``--machine ewasm`` but without specifying ``--yul-dialect``.
 * **NatSpec**: DocString block is terminated when encountering an empty line.
 * **Optimizer**: Fixed a bug in BlockDeDuplicator.
 * **Scanner**: Fix bug when two empty NatSpec comments lead to scanning past EOL.
 * **SMTChecker**: Fix internal error on try/catch clauses with parameters.
 * **SMTChecker**: Fix internal error when applying arithmetic operators to fixed point variables.
 * **SMTChecker**: Fix internal error when assigning to index access inside branches.
 * **SMTChecker**: Fix internal error when short circuiting Boolean expressions with function calls in state variable initialization.
 * **Type Checker**: Disallow assignments to storage variables of type ``mapping``.
 * **Type Checker**: Disallow inline arrays of non-nameable types.
 * **Type Checker**: Disallow usage of override with non-public state variables.
 * **Type Checker**: Fix internal compiler error when accessing members of array slices.
 * **Type Checker**: Fix internal compiler error when forward referencing non-literal constants from inline assembly.
 * **Type Checker**: Fix internal compiler error when trying to decode too large static arrays.
 * **Type Checker**: Fix wrong compiler error when referencing an overridden function without calling it.

Last but not least, as always, we'd like to thank all contributors who helped  make this release possible!

You can download the new version of Solidity [here](https://github.com/ethereum/solidity/releases/tag/v0.6.9).
