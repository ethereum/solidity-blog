---
layout: post
published: true
title: 'Mapping Named Parameters'
date: '2023-02-08'
author: Matheus Aguiar
category: Explainers
---

### Intro
[Solidity 0.8.18](https://github.com/ethereum/solidity/releases/tag/v0.8.18) introduced a language feature that
was well received by many in the community: Mapping types now have named parameters.
This new feature opens the possibility to name one or more of the parameters, as we show in the following example.

Example 1:

```solidity
mapping(string user => uint256 balance) public balanceOf;
mapping(address key => string) public dictionary;

```
Notice that the naming of parameters is optional and unnamed mapping declarations are still valid in the language.
It is particularly useful in situations where there are nested mappings, such as the one presented in the next example.


Example 2: (Nested mappings)

```solidity

mapping(address owner => mapping(address spender => uint256 balance)) public allowance;

```
### Advantages

Tooling can get more info in the generated code.
TODO: Elaborate on the example of https://github.com/ethereum/solidity/issues/11407 bellow

```
// solidity
mapping(address => mapping(address => uint256)) public allowance;

// typescript
// generated typing
allowance(arg0: string, arg1: string, overrides?: CallOverrides): Promise<BigNumber>;

// would be great to instead have
allowance(spender: string, owner: string, overrides?: CallOverrides): Promise<BigNumber>;

```
Code in general is easier to read, which helps not only in maintenance but also makes faster to understand code.
