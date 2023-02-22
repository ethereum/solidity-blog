---
layout: post
published: true
title: 'Feature Deep-Dive: User-Defined Operators'
date: '2023-02-22'
author: Kamil Śliwak, Matheus Aguiar
category: Explainers
---

The highlight of [Solidity 0.8.19](https://github.com/ethereum/solidity/releases/tag/v0.8.19) release
is the support for defining *operators* on [user-defined value types](https://docs.soliditylang.org/en/latest/types.html#user-defined-value-types) (UDVTs).

If you have not been keeping up with recent features, UDVTs are a new class of types introduced in Solidity 0.8.8.
They provide an abstraction over an elementary value type that results in a completely new type.
This is similar to creating an alias, but the new type is distinct from the underlying value type
and all other UDVTs derived from that underlying type.
The ability to use operators is meant to bring the UDVTs closer to being as natural to use as the
built-in types and promote the use of type-safe interfaces.
Below, we are going to take a closer look into the details of this new mechanism.

### A Quick Refresher
Before we get into the details of the new feature and how it affects the language, it may be a good
idea to first go over the two existing features that it builds upon: built-in operators and `using for`.

#### Built-in Operators
Solidity, like the vast majority of programming languages,
[offers a set of operators](https://docs.soliditylang.org/en/latest/types.html#operators) as syntactic sugar
over common arithmetical and logical functions.
The difference between operators and functions is purely syntactical, and the decision
to choose one over the other is to a large extent a matter of readability.
The use of operators makes expressions more concise, which is helpful as long as they are well
understood by the reader.
What is included in that set varies widely from language to language.

Here is the full list for Solidity:

| Category             | Operators                                                                |
|----------------------|--------------------------------------------------------------------------|
| Bitwise              | `&`, `\|`, `^`, `<<`, `>>`, `~`                                          |
| Arithmetic           | `+`, `-`, `*`, `/`, `%`, `**`                                            |
| Comparison           | `==`, `!=`, `<`, `<=`, `>`, `>=`                                         |
| Boolean              | `!`, `&&`, `\|\|`                                                        |
| Increment/decrement  | `++`, `--`                                                               |
| Simple assignment    | `=`                                                                      |
| Composite assignment | `+=`, `-=`, `^=`, `*=`, `/=`, `%=`, `&=`, `\|=`, `^=`, `<<=`, `>>=`      |
| Miscellaneous        | `delete`, `new`, `.`, `()`, `[]`, `?:`                                   |

It is important to note that in many languages, an assignment is a statement that is handled in a special way.
In Solidity, like in the C-like languages it took inspiration from, an assignment operator can be freely used in
expressions on par with other operators.

In most cases, built-in operators are only defined for [value types](https://docs.soliditylang.org/en/latest/types.html#value-types).
Notable exceptions are the simple assignment and the "miscellaneous" group.
For example, you can put almost anything in parentheses or branches of the ternary operator, as a part
of a bigger expression, even if its type cannot be named or even assigned to anything.

#### `using for`
Since operators are a purely syntactic element, you can always use a function as a substitute.
Solidity has a mechanism that is often used this way:
[`using for`](https://docs.soliditylang.org/en/latest/contracts.html#using-for).

The directive `using ... for ...` can be used to attach functions to a type.
These functions can then be used as members on any object of that type and receive the object as
their first parameter.

```solidity
pragma solidity ^0.8.13;

type Int is int;
using {add} for Int global;

function add(Int a, Int b) pure returns (Int) {
    return Int.wrap(Int.unwrap(a) + Int.unwrap(b));
}

function test(Int a, Int b) pure returns (Int) {
    return a.add(b);
}
```

Currently, `using for` has 3 forms:
1. `using L for T`: attaches all non-private functions from library `L` to type `T`.
1. `using L for *`: attaches all non-private functions from library `L` to any type, including types that
    cannot be named in the language and used explicitly (e.g. types of literals or array slices).
1. `using {f, L.g, ...} for T`: attaches given free or library functions `f`, `L.g`, ... to type `T`.

The directive can be used:
- ✅ At file level.
- ✅ Inside contract and library definitions.

The directive can not be used:
- ❌ Inside interface definitions.

When used inside contracts, it is not inherited.
Its effects extend only to the contract that has it and not to any other contracts derived from it.

At file level, the directive affects all the free functions, contracts and libraries defined within that file.
It does not affect other files that import the file containing it, unless it is marked as `global`.
`global` extends its effect to all files.
The use of `global` is possible only with UDVTs, structs, and enums that are defined in its source unit.

### Introducing User-Defined Operators
The new feature extends the third form of the `using for` syntax to allow binding operators to functions.
The function is invoked whenever the operator is used on the values of the type:

```solidity
pragma solidity ^0.8.19;

type Int is int;
using {add as +} for Int global;

function add(Int a, Int b) pure returns (Int) {
    return Int.wrap(Int.unwrap(a) + Int.unwrap(b));
}

function test(Int a, Int b) pure returns (Int) {
    return a + b; // Equivalent to add(a, b)
}
```

An operator can only be bound in a `global` directive and its definition must be a `pure` free function.
The type specified in `using for` must be a user-defined value type and must be the type
of all the parameters of the function and its return value.
The only exception are comparison operator definitions, where the return type must be `bool`.

Since currently there are no implicit conversions allowed to and from UDVTs, user-defined operators
must always be invoked with exact types.

The following operators can be defined:

| Category   | Operators                        |
|------------|----------------------------------|
| Bitwise    | `&`, `\|`, `^`,  `~`             |
| Arithmetic | `+`, `-`, `*`, `/`, `%`          |
| Comparison | `==`, `!=`, `<`, `<=`, `>`, `>=` |

Of the operators listed above, `~` is unary, `-` can be both unary and binary and the rest is always binary.
Note that Solidity does not have a unary `+`.
Unary and binary `-` are considered different operators and need separate definitions.
Whether `-` refers to the unary or binary variant depends on the number of parameters of the function.

It is not possible to replace operator definitions.
This applies both to the built-in operator definitions provided by the compiler and to operators
bound to a function provided by the user.
Once the operator is bound, the definition cannot be changed.

#### Operator Bindings vs Attached Functions
User-defined operators are independent of attached functions.
One can be done without the other, and both can be done simultaneously, even in the same `using for` directive.

The following example illustrates this:

```solidity
pragma solidity ^0.8.19;

type Int is int;
using {add as +} for Int global;
using {sub as -, sub} for Int global;

function add(Int a, Int b) pure returns (Int) {
    return Int.wrap(Int.unwrap(a) + Int.unwrap(b));
}

function sub(Int a, Int b) pure returns (Int) {
    return Int.wrap(Int.unwrap(a) + Int.unwrap(b));
}

function test(Int x, Int y) pure {
    x + y;
    x.add(y); // ERROR: Member "add" not found or not visible after argument-dependent lookup in Int.

    x - y;
    x.sub(y); // OK -- "sub" was also attached in "using for"
}
```

In the above example, the function `add()` is used *only* as the operator `+`, and not callable as `x.add(y)`
(but it can, of course, be also called as `add(x, y)`).
`sub()`, on the other hand, can both be used as the operator `-` and called on the type.

The rules for `using for global` with operators are the same as for attached functions
(with an additional restriction that operators *must* be global):
it is only allowed at file level, the type must be defined in the same file and it makes
the operator available everywhere.
Note that only the directive itself must be colocated with the type.
The operator definitions may be imported from a separate file.

#### Parameter Cleanup
If you decide to use inline assembly in your operator implementation, you should be aware of
a dangerous pitfall when using types that do not fill the whole stack slot.
Accessing Solidity variables in inline assembly bypasses the
[variable cleanup](https://docs.soliditylang.org/en/latest/internals/variable_cleanup.html)
mechanism that normally masks the dirty bits of your value.
In inline assembly you must perform that cleanup manually.

Consider an example that naively accesses the parameters and return values of operators
without properly cleaning up `uint8` values:
```solidity
pragma solidity ^0.8.19;

type U8 is uint8;
using {yoloAdd as +, yoloDiv as /} for U8 global;

function yoloAdd(U8 x, U8 y) pure returns (U8 z) {
    assembly {
        z := add(x, y) // Wrong! No cleanup.
    }
}

function yoloDiv(U8 x, U8 y) pure returns (U8 z) {
    assembly {
        z := div(x, y) // Wrong! No cleanup.
    }
}

contract C {
    function divAddNoOverflow(U8 a, U8 b, U8 c) external pure returns (U8) {
        return a / (b + c);
    }
}
```

Now, let's see what happens when you call `divAddNoOverflow(4, 0xff, 3)`.
You might expect it to return 4, i.e. `8 / ((255 + 3) % 256)`.
After all, `z` is based on `uint8` and an unchecked addition of 3 to 255 should wrap around and give you 2.
`8 / 2` is 4.
This is not what happens.
The actual result is zero.

Note that local variables take up whole stack slots and all calculations in inline assembly are
performed on 256-bit numbers.
The result of `add(0xff, 3)` is actually `0x0102` and `yoloAdd()` does not clear the higher bits.
`yoloDiv()` then receives that value as `y`, and again does not clear the higher bits.
This results in an integer division by 258 rather than by 2.

The problem can be avoided with manual cleanup.
The code would work as designed if `add(x, y)` was replaced with
```solidity
and(0xff, add(and(0xff, x), and(0xff, y)))
```
and `div(x, y)` with
```solidity
and(0xff, div(and(0xff, x), and(0xff, y)))
```

#### Examples

##### Unchecked Counter
This example shows how operators can be used to bypass the checked arithmetic for types
specifically defined for cases where those checks are superfluous:

```solidity
pragma solidity ^0.8.19;

type UncheckedCounter is uint;

using {add as +, lt as <} for UncheckedCounter global;

UncheckedCounter constant ONE = UncheckedCounter.wrap(1);

function add(UncheckedCounter x, UncheckedCounter y) pure returns (UncheckedCounter) {
    unchecked {
        return UncheckedCounter.wrap(
            UncheckedCounter.unwrap(x) +
            UncheckedCounter.unwrap(y)
        );
    }
}

function lt(UncheckedCounter x, UncheckedCounter y) pure returns (bool) {
    return UncheckedCounter.unwrap(x) < UncheckedCounter.unwrap(y);
}

contract C {
    uint internalCounter = 12;

    function testCounter() public returns (uint) {
        for (
            UncheckedCounter i = UncheckedCounter.wrap(12);
            i < UncheckedCounter.wrap(20);
            i = i + ONE
        )
            ++internalCounter;

        return internalCounter;
    }
}
```

##### A More Complex Abstract Example
This is a more complex example that shows multiple aspects of user-defined operators.
The exact calculations it performs do not matter, the point is to show the syntax in a larger context.

`redBlueScore.sol`
```solidity
pragma solidity ^0.8.19;

import {Red, Blue, Score, RED_LIMIT, BLUE_LIMIT} from "./types.sol";

contract RedBlueScore {
    Red public redGauge;
    Blue public blueGauge;

    function voteRed(Red value, Red base) public {
        require(-RED_LIMIT <= value * base && value * base <= RED_LIMIT);
        redGauge = redGauge + value * base.exp(3);
    }

    function voteBlue(Blue value) public {
        require(-BLUE_LIMIT <= value && value <= BLUE_LIMIT);
        blueGauge = blueGauge + value - Blue.wrap(100);
    }

    function calculateScore() public view returns (Score) {
        return
            redGauge.toScore() / RED_LIMIT.toScore() -
            blueGauge.toScore() / BLUE_LIMIT.toScore();
    }
}
```

`types.sol`
```solidity
pragma solidity ^0.8.19;

import "./operators.sol" as op;

type Red is int;
using {op.RedLib.toScore, op.RedLib.exp, op.addRed as +, op.mulRed as *, op.unsubRed as -} for Red global;
using {op.lteRed as <=, op.gtRed as >} for Red global;

type Blue is int;
using {
    op.addBlue as +,
    op.unsubBlue as -,
    op.subBlue as -,
    op.BlueLib.toScore,
    op.lteBlue as <=,
    op.gtBlue as >
} for Blue global;

type Score is int128;
using {op.addScore as +} for Score global;
using {op.subScore as -} for Score global;
using {op.divScore as /} for Score global;

Red constant RED_LIMIT = Red.wrap(10);
Blue constant BLUE_LIMIT = Blue.wrap(20);
```

`operators.sol`
```solidity
pragma solidity ^0.8.19;

import {Red, Blue, Score} from "./types.sol";

library RedLib {
    function toScore(Red x) internal pure returns (Score) {
        return Score.wrap(int128(Red.unwrap(x))) + Score.wrap(10);
    }

    function exp(Red x, uint y) internal pure returns (Red) {
        return Red.wrap(Red.unwrap(x) ** y);
    }
}

library BlueLib {
    function toScore(Blue x) internal pure returns (Score) {
        return Score.wrap(int128(Blue.unwrap(x))) - Score.wrap(20);
    }

    function add(Blue x, Blue y) external pure returns (Blue) {
        return Blue.wrap(Blue.unwrap(x) + Blue.unwrap(y));
    }
}

using {BlueLib.add} for Blue;

function addRed(Red x, Red y) pure returns (Red)  { return Red.wrap(Red.unwrap(x) + Red.unwrap(y)); }
function mulRed(Red x, Red y) pure returns (Red)  { return Red.wrap(Red.unwrap(x) * Red.unwrap(y)); }
function unsubRed(Red x)      pure returns (Red)  { return Red.wrap(-Red.unwrap(x)); }
function lteRed(Red x, Red y) pure returns (bool) { return Red.unwrap(x) <= Red.unwrap(y); }
function gtRed(Red x, Red y)  pure returns (bool) { return !(x <= y); }

function addBlue(Blue x, Blue y) pure returns (Blue) { return x.add(y); }
function unsubBlue(Blue x)       pure returns (Blue) { return Blue.wrap(-Blue.unwrap(x)); }
function subBlue(Blue x, Blue y) pure returns (Blue) { return x + -y; }
function lteBlue(Blue x, Blue y) pure returns (bool) { return Blue.unwrap(x) <= Blue.unwrap(y); }
function gtBlue(Blue x, Blue y)  pure returns (bool) { return !(x <= y); }

function addScore(Score x, Score y) pure returns (Score) { return Score.wrap(Score.unwrap(x) + Score.unwrap(y)); }
function subScore(Score x, Score y) pure returns (Score) { return Score.wrap(Score.unwrap(x) - Score.unwrap(y)); }
function divScore(Score x, Score y) pure returns (Score) { return Score.wrap(Score.unwrap(x) / Score.unwrap(y)); }
```

### AST Changes
The new feature introduces two new attributes to the Abstract Syntax Tree (AST) produced by the compiler:
1. `functionList` on `UsingForDirective` nodes can now contain operator entries of the following structure:
    ```json
    {
        "operator": "+",
        "definition": { /* function */ }
    }
    ```
    in addition to the current entries for functions that are structured as follows:
    ```json
    {
        "function": { /* function */ }
    }
    ```
1. `BinaryOperation` and `UnaryOperation` nodes may now have a `function` attribute.
    If present, the attribute indicates that the operation is actually an invocation of a user-defined
    operator and specifies the ID of the function used to define it.

This effectively introduces a new way to perform a function call not represented by a `FunctionCall` node.
It will affect any tool that needs to detect and follow function calls, for example, to build a
control flow graph.

### Design Rationale

#### Why Does Solidity Need User-Defined Operators?
The support for user-defined value types in Solidity cannot be considered complete without more natural
ways to operate on them.
The ability to use operators was envisioned from the beginning, and the main reason it was not provided
immediately was the uncertainty regarding interactions with other features we plan to introduce.
We decided that it was best to release the feature in a minimal usable state and extend it over time,
taking into account user feedback.

UDVTs in general were meant to fulfill two needs:
1. More type-safety when performing operations on values of the same underlying value type that express
    conceptually different things.
    For example quantities of different dimensions could both be stored as `uint` but adding them without
    an explicit conversion is a mistake that can only be prevented if the compiler knows that they
    are values of different kinds.

1. The ability to define new types using value types as the underlying representation.
    The best example of this are fixed-point types.

    Fixed-point types were originally planned to be an integral part of the language, and the
    partially implemented [`ufixed` and `fixed`](https://docs.soliditylang.org/en/latest/types.html#fixed-point-numbers)
    types are the vestiges of that old design.
    While trying to agree on the final design for the feature, however, we realized that no implementation
    will be a good enough trade-off for most situations and cannot become a suitable default choice.
    We decided to leave the choice up to library authors and instead focus on providing primitives
    necessary to build their own fixed-point types that feel like native types.
    User-defined operators are a step towards that goal.

#### Will User-Defined Operators Make Auditing Harder?
In the feedback we received so far one of the most common concerns is the effect of the feature on contract
auditing and potential to mask malicious code.
We want to address these concerns here.

First of all, any new syntax increases the complexity of the language and has some potential to make code more obscure
and harder to understand when abused.
It cannot be completely avoided and is always a trade-off between all the good and bad ways it can be used.
We think that improving the ergonomics of UDVTs is important enough that it outweighs any of the downsides we considered.

The readability benefits provided by operators cannot be overstated.
Code containing even relatively simple expressions can be hard to understand at a glance when many function calls are involved:
```solidity
if (equals(add(mul(a, x), mul(b, y)), add(div(c, z), mod(d, w)))) {
    return add(div(c, z), mod(d, w));
}
```
Chaining attached functions somewhat helps by placing the operation name between the arguments (similar to an infix operator):
```solidity
if (a.mul(x).add(b.mul(y)).equals(c.div(z).add(d.mul(w)))) {
    return c.div(z).add(d.mod(w)));
}
```
It is, however, still a far cry from the clean, concise notation that can be achieved with the use of operators:
```solidity
if (a * x + b * y == c / z + d % w) {
    return c / z + d % w;
}
```

The last example is how we would prefer real code to look like.
By the way, did you spot the mistake in the second example?

It is also essential to realize the actual scope of the feature.
You may imagine an elaborate system from languages like C++, with complicated rules and many corner cases.
As implemented now the feature is deliberately very limited.
We tried both to limit surprises and to avoid prematurely adding support for cases that we have not carefully thought out.

In the current implementation, it is not possible to:

1. Have operators on types other than UDVTs.
    - User-defined operators on [reference types](https://docs.soliditylang.org/en/latest/types.html#reference-types)
      like structs or arrays are not allowed.
    - It is not possible to define them on built-in value types, function types or enums.
    - You cannot define them to unnamed types using the type wildcard (`*`).
1. Have operators that are not `pure`.
    Operators cannot modify storage.
    They can, at most, make pure external calls.

    The worst that is technically possible with tricks is for them to modify memory (only through inline assembly)
    or make staticcalls (through interfaces that do not match deployed code).
1. Have operators on mixed types.
    Parameter and return value types of operators must be the same.
    Also, no implicit conversions are possible, so it is very simple to determine if a particular operation
    will invoke a user-define operator or a built-in one (just look at the type).
1. Redefine operators.
    You cannot replace any of the built-in operators with a custom definition.
    For user-defined operators, only one definition can be provided.
1. Bind operators to contract functions.
    This also means that operator definitions cannot be inherited, virtual, or remain unimplemented.
1. Bind operators to overloaded functions.
1. Use built-in functions to define operators.
    You cannot e.g. bind an operator directly to `keccak256()`.
1. Change the precedence or associativity of an operator.
1. Define any of the operators with more complex semantics.
    You cannot define:
    - `&&` and `||` due to their short-circuiting behavior.
    - `++` and `--` because they modify their argument and also have separate prefix and postfix variants with different semantics.
    - `+=`, `-=`, `^=`, `*=`, `/=`, `%=`, `&=`, `\|=`, `^=`, `<<=`, `>>=` because they modify their argument.
    - `=`, `()`, `?:`, and `.` because they are built-in for all types.
    - `delete`, `new`, and `[]` because they are not usable with UDVTs.
1. Define custom operators that do not exist in the language, like `===` or unary `+`.
1. Bind operators in bulk or accidentally.
    The `using L for T` syntax for attaching a whole library to a type does not cover operators.
    Operators must always be bound explicitly.

In addition to these restrictions, we decided to limit them even more in response to feedback we received before releasing Solidity 0.8.18.
This means you also cannot:
1. Use library functions to define operators.
    This excludes external, public, and private functions.
    It is also now impossible to apply modifiers to operators because free functions cannot have them.
1. Have non-global operators, which has several consequences:
    - Operators can only be bound in the file that also defines the type.
        They are available wherever the type is available and cannot have different definitions in different scopes.
    - Local bindings at contract level are not possible (i.e. inside contracts and libraries).
        This also makes it impossible to define operators for local types that are defined inside contracts or libraries.
1. Define `!`, `<<`, `>>` and `**`.

We may lift *some* of these restrictions in the future, but we will consider every case carefully.
Some of them, like the inability to use overloaded functions, stem from pre-existing limitations of the syntax, but
others, like a ban on redefining built-in operators, are very intentional and meant to limit potential misuse.

In the end, any audit must include a careful review of type definitions and functions, and we think
that this will reveal most cases of malicious behavior.
Given the above limitations, bad operators cannot do much more than mislead, which is something that
will come up in the review of the definition.
Weird behavior, like `+` that does anything other than perform addition should raise a red flag,
just like a function called `transfer()` that does not transfer anything.

Reviewing the invocations of an operator should not pose significant problems either, as long as
the reviewer is aware of the types involved, which is always necessary to understand the expression.
This is not a new thing, since the behavior of the built-in operators is has always been affected by operand types.
For example the overflow semantics of integer types depend on the size of the type in question.

Note that user-defined operators cannot alter semantics of the existing code.
For built-in types behavior cannot be altered at all.
For UDVTs any operator must necessarily be user-defined so seeing it is not much different from
seeing a function call.
An operator can have only one definition, the definition must be explicitly bound in the file that
defines the type and cannot be shadowed.
In both cases the name may be misleading and a look at the definition may be necessary.

It is also worth noting that the feature is meant to be predominantly used in reusable libraries providing common types.
In this context, once the library is audited, the concern about operators having misleading or unexpected behavior should be minimal.

### The Future of User-Defined Operators and UDVTs
Now that we know the new feature and its limitations, the  question on everyone's mind is probably
"Will it stay that way?".
How will the feature evolve in the short and long term?

#### Literal Suffixes
The very next step for UDVTs is the ability to provide custom literals of user-defined value types.
The ease of using literal values in expressions is a big part of what makes a type feel native.
Solidity already provides fractional literals and our intention is to make them usable with custom
fixed-point number implementations.
To achieve that we are going to allow defining suffix functions that return UDVT by converting
literals that are already available in the language:

```solidity
type UFixed128x18 is uint128;

function uf(int16 mantissa, uint8 exponent) pure suffix returns (UFixed128x18) { ... }
```
```solidity
UFixed128x18 circumference = 2 uf * 3.14 uf * radius;
```

While the application of a suffix will involve a function call, eventually, with the help of
compile-time evaluation, literal suffixes are going to become as efficient as native literals.

#### More Operators
Many operators have been excluded from the initial implementation.
Some of them will very likely be user-definable at some point:

1. `<<`, `>>` and `**` were excluded due to their non-uniform arguments.
    For built-in types, the right-hand side is always an unsigned type.
1.  `!` was excluded because we have to decide whether it should return the same type as the argument
    or just `bool`.
    We are leaning towards the latter, but it needs more consideration.
1. `++` and `--` are very convenient, and we want to have them eventually.
    They just need to be handled in a special way, and there is more than way to do so.
1. Composite assignment operators, such as `+=`, will likely be provided automatically
    in the future as long as the corresponding arithmetic/bitwise operator is available.
    Allowing a direct implementation is currently not possible anyway, because there are no references
    to value types in the language.

On the other hand, it is unlikely that we will allow redefining some other operators:

1. `=` is already provided by the compiler for all types and it is very unlikely that we will ever allow redefining it.

#### Operators on Complex Types
While UDVTs were the primary target, in some cases it may also be reasonable to build a custom type
with a more complex underlying representation, such as a struct or a dynamic array.
The main thing holding us back here are upcoming changes to the type system that are a significant part
of our roadmap for the near future.
To avoid having to introduce breaking changes later, we decided to defer support for such operators until
these changes are behind us.

#### What Will Not Change?
Due to concerns about the safety of the feature, there are some key elements that are very unlikely to ever change regarding operators.
Operators will likely always be global and defined at file level with free functions.
It will not be possible to replace built-in opertors or redefine already defined user-defined operators.
