---
layout: post
published: true
title: 'Solidity Import Path FAQ'
date: '2021-08-16'
author: Kamil Śliwak
category: Explainers
---

Paths are one of the things that "just work" as long as you're not too particular about the details.
If you can just hand off the path to a library, it's no longer your problem.
And the details can get complicated if your program has to work on a variety of platforms with completely different filesystem conventions or even not having a filesystem at all (e.g. a browser).

This is the very reason why the Solidity compiler takes a hands-off approach to the problem.
The [Remix IDE](https://remix.ethereum.org/) is widely known for supporting Github URLs but what many developers do not know is that you can use literally any string to identify your file.
Internally the compiler makes very few assumptions about that string which, ironically, sometimes leads to misunderstandings when it clashes with programmer's expectations.
In the end the source code in most cases comes from a filesystem and users do not refer to it using abstract identifiers.
They expect the compiler to map their local filesystem content to its own internal structure in a consistent way regardless of the platform.
Even when using [Standard JSON](https://docs.soliditylang.org/en/latest/using-the-compiler.html#compiler-input-and-output-json-description) format, allows tools to access that structure directly, the burden just shifts to the tool.

The details of the whole mechanism have recently been documented in [Import Path Resolution](https://docs.soliditylang.org/en/latest/path-resolution.html) but many of the consequences of the documented behavior may not be apparent.
This blog post is meant to be a more complete exploration of problems you might encounter when dealing with paths in Solidity.
It will show you specific corner cases, explain why they happen and tell what you can do to avoid them.

### Q: What's wrong with relative paths in Solidity? Aren't `import "file.sol"` and `import "./file.sol"` the same thing?
No, in Solidity these two statements are processed very differently.
`file.sol` is actually considered absolute in compiler's virtual filesystem.
Why?
Let's take a look at how the compiler decides which source unit an import refers to.

#### Compiler's VFS and source unit names
First of all, the VFS is not hierarchical and it's better to think about it as a flat database table.
Each source unit is associated with a unique name (key) which can consist of arbitrary characters.
It does not have the concept of a path though filesystem paths are valid keys and commonly used as such.

Here's an example of what the content of the VFS might look like when compiling a project consisting of a few files with imports:

| Source unit name                              | Source unit content                                                                                                                                |
|-----------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------|
| `contracts/MyContract.sol`                    | <pre lang=solidity>import "./math/Math.sol";&#10;import "ipfs://Qmdyq9ZmWcaryd1mg";&#10;contract MyContract { ... }</pre>                          |
| `contracts/math/Math.sol`                     | <pre lang=solidity>import "@openzeppelin/contracts/utils/Array.sol";&#10;import "459a6f79ad9b13cbcb5f692d2cc7a94d";&#10;library Math { ... }</pre> |
| `@openzeppelin/contracts/utils/Array.sol`     | <pre lang=solidity>import "./math/Math.sol";&#10;library Arrays { ... }</pre>                                                                      |
| `@openzeppelin/contracts/utils/math/Math.sol` | <pre lang=solidity>library Math { ... }</pre>                                                                                                      |
| `ipfs://Qmdyq9ZmWcaryd1mg`                    | <pre lang=solidity>contract Ballot { ... }</pre>                                                                                                   |
| `459a6f79ad9b13cbcb5f692d2cc7a94d`            | <pre lang=solidity>import "contracts/math/Math.sol";&#10;import "/";&#10;contract Token { ... }</pre>                                              |
| `/`                                           | <pre lang=solidity>library Util { ... }</pre>                                                                                                      |

If you give the compiler input in the Standard JSON format, you are free to choose any names you like.
They can be paths, URLs, numbers, random strings, geographical coordinates, pet names, you name it.
As you can see even names that would be completely unacceptable for a file in a real filesystem, like `/`, can be freely used.

Any valid name is considered absolute in the VFS because it unambiguously identifies a source unit.
Names ending with `/` are completely fine and do not represent directories - the concept of a directory does not exist at the VFS level.
Since there's no root directory, names do not have to start with `/` to be absolute.

#### Direct and relative imports
The most basic way to refer to a source unit is to spell out its whole name.
This is the equivalent of an absolute path in an actual filesystem.
An import using a full name is called a [direct import](https://docs.soliditylang.org/en/latest/path-resolution.html#direct-imports).
For example `import "contracts/math/Math.sol"` will always refer to code under `contracts/math/Math.sol` in the VFS regardless of where you place it.
It is not relative to anything.

Solidity's syntax for a relative source unit name is to start an import with `./` or `../`.
This is a [relative import](https://docs.soliditylang.org/en/latest/path-resolution.html#relative-imports).
To determine what such an import refers to, it has to be resolved into a full name.
Just like in an actual filesystem relative paths are relative to the current working directory, relative names are relative to the name of the current source unit.
For example `import "./math/Math.sol"` placed in `contracts/MyContract.sol` resolves into `contracts/math/Math.sol` but in `@openzeppelin/contracts/utils/Array.sol` it resolves into `@openzeppelin/contracts/utils/math/Math.sol` instead.

Relative import resolution is the only situation where the compiler expects a path-like structure from a source unit name.
Even this is optional - if your names are not paths you can just not use relative imports.
`/` characters and `.` and `..` segments are treated specially only for the purpose of that operation.
You can also use such import with names that only partially resemble paths - for example they work perfectly with HTTP URLs as long as you are careful not to step into the domain part.

### Q: How does the compiler determine if two imports refer to the same file? Do import paths get normalized?

#### Equivalence of source unit names
The rule is very simple, much simpler that many users expect - source unit names must be exactly identical.
There is no normalization for source unit names and it does not matter if their content is identical or not.
The same content under multiple names will be compiled multiple times and produce multiple copies of symbols defined in it that just happen to have the same name.

**Example 2.1**: Importing the same symbol twice using the same exact direct import is fine:
- `test.sol`
    ```solidity
    import {ERC20} from "@openzeppelin/contracts/token/ERC20/ERC20.sol";
    import {ERC20} from "@openzeppelin/contracts/token/ERC20/ERC20.sol";
    ```

**Example 2.2**: If you use a relative import that resolves into the same source unit name as a direct one, it's fine too.
- `@openzeppelin/contracts/token/ERC20/test.sol`
    ```solidity
    import {ERC20} from "@openzeppelin/contracts/token/ERC20/ERC20.sol";
    import {ERC20} from "./ERC20.sol";
    ```

**Example 2.3**: Even a tiny difference in a direct import results in the compiler seeing two separate source units:
- `test.sol`

    ```solidity
    import {ERC20} from "@openzeppelin/contracts/token/ERC20/ERC20.sol";
    import {ERC20} from "@openzeppelin/contracts/token/ERC20//ERC20.sol";
    ```

For both of these imports the default import callback would load the content from the same location on disk but the compiler cannot assume that.
It can only see that the source unit names are not identical and both source units just happen to contain a contract named `ERC20`.
The result is a compilation error:
```
Error: Identifier already declared.
 --> test.sol:2:9:
  |
2 | import {ERC20} from "@openzeppelin/contracts/token/ERC20//ERC20.sol";
  |         ^^^^^
Note: The previous declaration is here:
  --> @openzeppelin/contracts/token/ERC20/ERC20.sol:33:1:
   |
33 | contract ERC20 is Context, IERC20, IERC20Metadata {
   | ^ (Relevant source part starts here and spans across multiple lines).
```

**Example 2.4**: Let's look at a slightly more complex example that shows how this lack of normalization can lead to seemingly inexplicable errors:
- `contracts/MyERC20.sol`
    ```solidity
    import {ERC20} from "contracts/../node_modules/@openzeppelin/contracts/token/ERC20/ERC20.sol";

    contract MyERC20 is ERC20("MyERC20", "MERC") {}
    ```
- `contracts/test.sol`
    ```solidity
    import {ERC20} from "@openzeppelin/contracts/token/ERC20/ERC20.sol";
    import {MyERC20} from "./MyERC20.sol";

    function run(ERC20 token) {}

    function test() {
        run(new MyERC20());
    }
    ```
- Compilation commands:
    ```bash
    npm install @openzeppelin/contracts
    solc contracts/test.sol --allow-paths . @openzeppelin=node_modules/@openzeppelin
    ```

If you try to compile the code above, you will run into the following error:
```
Error: Invalid type for argument in function call. Invalid implicit conversion from contract MyERC20 to contract ERC20 requested.
 --> contracts/test.sol:7:9:
  |
7 |     run(new MyERC20());
  |         ^^^^^^^^^^^^^
```

But `MyERC20` does inherit from `ERC20` so it should be implicitly convertible to it.
What's going on?
Let's take a look at the content of the VFS:

| Source unit name                                                          | Source unit content                                                                                                                                           |
|---------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `contracts/test.sol`                                                      | <pre lang=solidity>import {ERC20} from "@openzeppelin/contracts/token/ERC20/ERC20.sol";&#10;import {MyERC20} from "./MyERC20.sol";&#10;&#10;...</pre>         |
| `contracts/MyERC20.sol`                                                   | <pre lang=solidity>import {ERC20} from "contracts/../node_modules/@openzeppelin/contracts/token/ERC20/ERC20.sol";&#10;&#10;contract MyERC20 is ERC20 {}</pre> |
| `node_modules/@openzeppelin/contracts/token/ERC20/ERC20.sol`              | <pre lang=solidity>contract ERC20 { ... }</pre>                                                                                                               |
| `contracts/../node_modules/@openzeppelin/contracts/token/ERC20/ERC20.sol` | <pre lang=solidity>contract ERC20 { ... }</pre>                                                                                                               |

Note how there are two separate source units containing `ERC20`.
`contracts/../node_modules/` and `node_modules/` part would be the same directory in an actual filesystem but the Solidity compiler does not perform such a normalization.

#### Imports undergo partial normalization in some cases
While source unit names must be strictly identical to refer to the same source unit, you do not always specify a name directly in an import.
This is only true for direct imports (and even these are subject to [import remapping](https://docs.soliditylang.org/en/latest/path-resolution.html#import-remapping)).

**Example 2.5**: `import "contracts/math/../math/.///Math.sol"` is a direct import.
It will not match `contracts/math/Math.sol` - the source unit name must be exactly `contracts/math/../math/.///Math.sol`.

For relative imports the translation is more complex and there is some limited normalization involved.
To resolve them the compiler needs two pieces of information: the import path (i.e. the part that is given explicitly in the import statement) and the name of the importing source unit.
The only part that gets normalized is the import path.
The compiler collapses any redundant `.` and `..` segments and removes sequences of multiple slashes.

**Example 2.6**: `import ".//math/../math/.///Math.sol"` is a relative import.
Placed inside `contracts/MyContract.sol` it would refer to `contracts/math/Math.sol` and not `contracts/math/../math/.///Math.sol`.

The name of the importing source unit is not normalized.

**Example 2.7**: `import "../../../math/Math.sol"` placed inside `/home//user/contracts/.././//MyContract.sol` will resolve into `/home//user/math/Math.sol` and not `/math/Math.sol`.
Note how `..` and `.` in the importing source unit name are treated as directory names and get canceled by `..`.
This is different from filesystem normalization where joining `x/y/../` and `../a.sol` results in `a.sol` and not `x/y/a.sol`.

This behavior makes it possible to handle relative imports gracefully without making too many extra assumptions about the name of the importing source unit.
For example it would not be possible to use URLs in imports otherwise because the `://` would get mangled into `:/`.
This problem is also completely and easily avoidable if you always use the normalized form of the path in your imports.

**Example 2.8**: `import "./math/Math.sol"` placed inside `https://example.com/MyContract.sol` will resolve into `https://example.com/MyContract.sol` and not `https:/example.com/MyContract.sol`.

We are considering introducing more intuitive normalization rules for imports in future versions of the compiler.

#### Paths specified on the command line
In the current version of the compiler paths specified on the commandline are not normalized at all.
This behavior is unintended and will change in the next release but you should be aware of it when using older compiler binaries.

**Example 2.9**
- `/home/user/project/test.sol`
    ```solidity
    import "contract.sol";
    ```
- `/home/user/project/contract.sol`
    ```solidity
    contract C {}
    ```
- Compilation commands:
    ```bash
    cd /home/user/project/
    solc test.sol /home/user/project/contract.sol
    ```

This will create two different source units containing contract `C` in the VFS: `contract.sol` and `/home/user/project/contract.sol`.
The contract will be compiled twice because the source unit name assigned to the file is exactly what is stated on the command line.

Note also that on Windows the drive letter becomes part of the source unit name.

**Example 2.10**: On Windows the drive letter becomes part of the source unit name.
- Compilation commands:
    ```bash
    cd C:\Users\project\
    solc.exe C:\Users\project\contract.sol
    ```

The compiler replaces the Windows-specific path separators with `/` but does not strip the drive letter so the source unit name assigned in the VFS is `C:/Users/project/contract.sol` and not `/Users/project/contract.sol` or `contract.sol`.

Changes in the next release will make the source unit name independent of the form in which the path was specified.
The path will be converted to a canonical form and made relative if they're inside base path.

#### Paths in Standard JSON
When using Standard JSON, you can assign source unit names directly.
This means that the translation you expect in imports does not apply.
There is no resolution of relative source unit names and no remappings.

**Example 2.11**
- `input.json`
    ```json
    {
        "language": "Solidity",
        "sources": {
            "contract.sol":    {"content": "import \"prefix/Math.sol\";"},
            "./math/Math.sol": {"content": ""},
            "./Util.sol":      {"content": "import \"./math/Math.sol\";"}
        },
        "settings": {"remappings": [":prefix/=./math/"]}
    }
    ```
- Compilation commands:
    ```bash
    solc --standard-json input.json
    ```

This is the content of the VFS the compiler creates from the above.

| Source unit name  | Source unit content                                |
|-------------------|----------------------------------------------------|
| `contract.sol`    | <pre lang=solidity>import "prefix/Math.sol";</pre> |
| `./math/Math.sol` |                                                    |
| `./Util.sol`      | <pre lang=solidity>import "./math/Math.sol";</pre> |

Note how the `./` prefix has not been stripped.
It's a part of source unit name, not a part of relative import syntax.
You can give a source unit such a name even though it cannot even be used in an import - it would be interpreted as a relative import.
The example shows two tricks that can be used to work around this problem - remapping to a different prefix that does not start with `./` and, alternatively, a relative import from another source unit whose name starts with th3e same prefix.

While possible, using source unit names that look like relative imports is not recommended.
Future versions of the compiler might disallow them completely.

### Q: The compiler is telling me `File import callback not supported`. What does it mean?
From user's perspective the answer is simply that the file does not exist or the path is wrong.
The extra detail in the message is not aimed only at users though.
You are most likely to encounter this message when using Solidity frameworks and tools that do not provide their own [import callback](https://docs.soliditylang.org/en/latest/path-resolution.html#import-callback).
Such tools have to implement their own file discovery mechanisms and are expected to provide a more informative error message of their own when they cannot find a file but they might also decide to proceed despite this and just show you the compiler output.

To understand the whole situation better let's analyze the situation in more detail.

The file discovery mechanism built into the compiler depends on an *import callback*.
The compilation process is isolated from the actual filesystem and when the compiler processes an import it only looks for the content in its internal VFS.
The VFS, however, contains only files specified in the initial input, that is the files whose paths were given on the command line or the source code provided inside Standard JSON input.
If the source unit being imported is not present in the VFS, the only way for the compiler to get it is to ask the callback for it.
If there is not callback the compilation will not succeed unless files corresponding to all the imports in the source code are already listed in the initial input.

The default callback simply loads the file from the filesystem.
Native compiler binaries can only use this callback and it cannot be disabled so any tools relying on such binaries will display a slightly different message when they cannot find a file:

```
ParserError: Source "util.sol" not found: File not found.
 --> contract.sol:1:1:
  |
1 | import "util.sol";
  | ^^^^^^^^^^^^^^^^^^
```

JavaScript-based tools, however, usually use [solc-js](https://github.com/ethereum/solc-js), which is a wrapper library over a compiler binary compiled to WebAssembly (or asm.js in the past).
Such binaries are platform-agnostic and cannot assume that there is a filesystem available since they are meant to be usable for example in a web browser.
The code related to the command-line interface and file loading is not present in these binaries.
They can instead provide an import callback written in JavaScript.
The advantage of this approach is that they can use the full potential of source unit names and support values other than paths.

A tool can also choose not to provide a callback.
This means that it must discover all the source files on its own by recursively scanning them for imports.
Relative imports and import remappings must be correctly interpreted to find the source unit name the compiler will use in the VFS and then the file corresponding to that name must be located on disk.

If such a tool does not find the file and proceeds with the compilation anyway, the error message informs that the file was not found in the VFS and that there is no callback that could load it from another source:
```
ParserError: Source "util.sol" not found: File import callback not supported
  --> project:/contract.sol:1:1:
  |
1 | import "util.sol";
  | ^^^^^^^^^^^^^^^^^^
```

### Q: Why do my paths only work/break on Windows/macOS
Since source unit names can contain arbitrary characters, Standard JSON input is platform-independent as long as it is self-contained and no additional files need to be loaded using an import callback.
If you do rely on an import callback, however, you have to ensure that it can match your imports to files on disk.
In case of the default callback it means that they must look like paths that are valid on that platform.
This is also true if instead of a callback you rely on a framework to find the files for you and build Standard JSON input, if the tool uses paths as source unit names.

Fortunately there is a common subset of paths that is valid on all common platforms.
Below we'll go through the common differences you might encounter.
We'll also see how to use platform-specific paths correctly if you do decide to use them.

#### Path separators
##### On the command line
All paths given on the command line are intepreted in a platform-specific way.
Invoking the compiler with a path like `contracts\token.sol` on Windows is completely fine and expected.
The compiler will internally convert `\` to `/` when assigning source unit name to the file.

If you are using Bash and common Linux tools on Windows (e.g. those installed with [Git for Windows](https://gitforwindows.org)) you should be aware of the [Automatic Unix ⟶ Windows Path Conversion](https://www.msys2.org/docs/filesystem-paths/#automatic-unix-windows-path-conversion).
The conversion results in any arguments that look like UNIX paths being replaced with platform-specific equivalents.
For example if you have git installed in `C:\Program Files\Git\`, invoking `solc.exe /projects/contract.sol` will result in the compiler receiving `C:\Program Files\Git\projects\contract.sol` as the input path.
This is likely not the path you expect.
For this reason it's recommended to use paths in the native format when invoking the compiler on the command line.
This behavior can also be disabled by setting the `MSYS_NO_PATHCONV` environment variable.

##### In the default import callback
Path separators in source unit names are intepreted in a platform-specific way when passed to the default import callback.
On Windows both forward- and backslashes are treated as separators while on Linux and macOS this behavior is limited to forward slahes.
For this reason the use of backslashes in imports, while allowed, is highly discouraged.

Note that to use a forward slash in a Solidity import you have to escape it.
For example `import "C:\Users\test\contract.sol"` refers to `C:Users	estcontract.sol` because `\U`, `\c` and `\t` are escape sequences.
`\U` and `\c` are equivalent to just `U` and `c`, and `\t` represents a tab character.
To actually use a path with forward slashes the import must look like this: `import "C:\\Users\\test\\contract.sol"`.

##### In relative imports
At the VFS level the only situation where path separators matter is the resolution of relative imports.
These expect forward slashes on all platforms.
Backslashes are treated as a part of the file name in such imports, even on Windows.
In particular imports starting with `.\` or `..\` are **not** recognized as relative imports at all.
for example `import ".\\contract.sol"` is a direct import and the resulting source unit name is always `.\contract.sol`.

#### Drive letters
Having multiple drives with separate directory tries is a feature unique to Windows.
The situation is very similar to that with path separators - the recommended alternative is to refer the filesystem root as `/`.
For this to work you need to ensure that all source files are on a single drive and that the compiler's working directory is also located on that drive.
If you want to keep some of the files on another drive, you can still avoid referring to that drive directly by creating symbolic links (if your version of Windows supports them).

##### On the command line
In the current version of the compiler absolute paths specified on the command line are not normalized and made relative when source unit names are assigned in the VFS.
For example running `solc.exe C:\Users\test\contract.sol` will make the compiler use `C:/Users/test/contract.sol` as the name.
This means that on Windows if you include the drive letter, it will end up in your contract metadata.
This behavior is going to change in the next release and the compiler will instead attempt to use paths relative to [base path](https://docs.soliditylang.org/en/latest/path-resolution.html#base-path) whenever possible.
For the time being, you should avoid using absolute paths on the command line.

Another thing worth pointing out is that the drive letter alone is not a valid directory name on Windows.
For example passing `--allow-paths C:` option to the compiler will not whitelist the whole drive.
You need `--allow-paths C:\` instead.
Or just `--allow-paths \` if it's the current drive.

#### Restricted characters
The only restriction on source unit names is that they cannot contain the `\0` character (a byte with a value of zero).
Actual filesystems often have many more restrictions:
- A wider range of disallowed characters.
    This varies greatly but many filesystems disallow at least `/`.
- Characters invalid at certain positions.
    For example on Windows file names cannot end with a dot.
- File names or paths as a whole may have a maximum length.
    Limits are not consistent across different filesystems.

For a more complete list see the Wikipedia article for [Filename](https://en.wikipedia.org/wiki/Filename).

The general recommendation is to avoid names that are excessively long or contain characters other than letters and some commonly used safe symbols like `_`.

#### Case-sensitivity
Compiler's VFS is case-sensitive.
Even if your filesystem is case-insensitive (usually the case on Windows and macOS), you should consistently refer to files using the same case.
Otherwise each case variant will be seen as a different source unit and compiled separately.

For example the default import callback will be able to locate source code for both `import "Contract.sol"` and `import "contract.sol"` but you will get two separate entries in the VFS.
Also `solc.exe Contract.sol contract.sol` will result in the contract being compiled twice.
This will remain true even after the compiler starts normalizing the paths specified on the command line, though using inconsistent case might start producing a warning in future releases.

#### Unicode characters
The compiler does not make any assumptions about the encoding of characters used in source unit names.
For example any UTF-8 encoded text is valid and bytes that represent ill-formed UTF-8 character sequences are accepted as well.

The import syntax is more restrictive though.
The import statement requires a string literal and [unicode literals](https://docs.soliditylang.org/en/latest/types.html#unicode-literals) are not accepted, which means that any non-ASCII characters must be escaped.

**Example**: To import a file called `ユニコーン.sol` you need `import "\u30e6\u30cb\u30b3\u30fc\u30f3.sol"`.

Some filesystems perform [Unicode normalization](https://en.wikipedia.org/wiki/Unicode_equivalence#Errors_due_to_normalization_differences), which poses a problem similar to case-sensitivity.
HFS+ used on macOS is an example of such a filesystem.
The normalization means that some multiple byte sequences can be treated as representing same name while on other systems they would be all considered different.
This complication is yet another reason to avoid non-ASCII characters in file names.

#### UNC paths
On Windows paths starting with `\\` or `//` followed by a host name ([UNC paths](https://en.wikipedia.org/wiki/Path_(computing\)#UNC)) are used to represent shared network resources.
On UNIX-like systems such paths have no special meaning but are often still treated as distinct from normal paths.

**Example**: `/home/user/`, `//home/user/`, `///home/user/` on Linux all refer to user's home directory, but `///home/user/` gets normalized into `/home/user/`, while `//home/user/` does not.

Making UNC paths relative is especially problematic since they cannot reliably be used as the working directory.
The `cmd.exe` shell does not allow changing the directory to an UNC paths without resorting to tricks.
The compiler does work with absolute UNC paths but they are not recommended for the same reason any other absolute paths are.

#### Symbolic links
Symbolic links have been [supported on Windows](https://en.wikipedia.org/wiki/Symbolic_link#Microsoft_Windows) for quite some time.
There are, however, important differences compared to how they work on UNIX-like systems.
Most importantly, on Windows you have to indicate whether the link is pointing at a file or a directory and using the wrong type will make it non-functional.
Also forward slashes do not work reliably when used in the target path so it's recommended to use the native path separator instead.

### Q: Why are absolute import paths bad? What does it have to do with source code verification?
The problem is not the form of the path on its own but rather the superflous information that ends up being included when the path is made absolute.
It's completely fine to use an absolute path in a generic and reproducible environment like a Docker image where it can remain constant no matter where the environment is deployed.

An important factor is that you might be building your contract with absolute paths even if you do not put them directly in your imports.
Frameworks and tools, which construct the Standard JSON input are free to use anything for source unit names and they might choose to use absolute paths for that.
If you're only using relative imports, you won't see them unless you inspect the compiler input or contract metadata.

Another way to end up with absolute paths is using them as [import remapping](https://docs.soliditylang.org/en/latest/path-resolution.html#import-remapping) targets.
Remappings do not simply tell the compiler where to find the files on disk, they affect source unit names.
Even if your import is a clean `import "@openzeppelin/contracts/utils/Array.sol"`, by compiling the contract with `solc contract.sol @openzeppelin/=/home/user/.packages/OpenZeppelin/`, you end up with `/home/user/.packages/OpenZeppelin/` in your contract metadata.

Finally, using absolute paths on the command line currently results in them being used for source unit names too, though this is going change in the upcoming release.

Using absolute paths hurts bytecode reproducibility.
Source unit names are included in contract metadata and a hash of the metadata is always appended to the bytecode.
Just having absolute paths in the Standard JSON input constructed by the tool you are using means that you are likely to obtain different bytecode in different environments.
If files are stored inside home directory, the paths include the username is usually different on different machines.
On Windows the paths may also include the drive letter.

To deal with this problem verification tools often completely strip metadata hash from the bytecode.
This, unfortunately, weakens the guarantees you get from verification.
Without ensuring that metadata is the same, the tool allows submitting modified source code as long as it produces the same bytecode.
It may have different comments, internal functions, local variable names or be laid out in files completely differently (which is why flattening works at all).
While such differences cannot affect the behavior of the contract, they may still obscure how the contract really works and mislead users into believing that it's safe.
Ignoring them also allows anyone who has access to the source code to modify the licensing or authorship information and submit such modified source before the contract author.

Another downside of using absolute paths is that they may contain information you do not want to be leaked to the blockchain.
The most obvious example is the local username being exposed as a part of the path to the home directory.

### Q: What is the purpose of `--base-path`? Why does the compiler prepend it even to absolute paths?
Base path is a mechanism that was originally introduced in an attempt to make it easy to use relative paths to refer to project files.

By default base path is empty, which means that source unit names are directly interpreted as paths.
Absolute ones remain absolute while relative ones are relative to the working directory.
For example, assuming that the working directory is `/home/user/`:

| Source unit name                          | Filesystem path                                      |
|-------------------------------------------|------------------------------------------------------|
| `contracts/file.sol`                      | `/home/user/contracts/file.sol`                      |
| `/home/user/contracts/file.sol`           | `/home/user/contracts/file.sol`                      |
| `@openzeppelin/contracts/utils/Array.sol` | `/home/user/@openzeppelin/contracts/utils/Array.sol` |


When set to a non-empty value, base path is instead automatically prepended to all source unit names.
This turns both absolute and relative paths into ones relative to base path.
For example, assuming that the project is compiled with `--base-path=project/`:

| Source unit name                          | Filesystem path                                              |
|-------------------------------------------|--------------------------------------------------------------|
| `contracts/file.sol`                      | `/home/user/project/contracts/file.sol`                      |
| `/home/user/contracts/file.sol`           | `/home/user/project/home/user/contracts/file.sol`            |
| `@openzeppelin/contracts/utils/Array.sol` | `/home/user/project/@openzeppelin/contracts/utils/Array.sol` |

In effect, base path represents the project root.
Making the leading `/` in direct imports refer to project root reinforces the idea that they are not relative to the location of the source file containing them but rather to some fixed point in the filesystem.

### Q: What is the right way to import files from packages when using `--base-path`?
Unfortunately the base path mechanism on its own is not enough to support imports from packages installed in arbitrary locations in the filesystem.
This is not an obstacle for frameworks because many of them have their own mechanisms for locating files and can place them under the right names in the Standard JSON input.
It's still a problem in situations where files are loaded using the default import callback.

For this very reason the upcoming release will introduce the `--include-path` option, which will allow specifying extra directories to be searched by the default import callback.
For example `--include-path node_modules/` will make `import "@openzeppelin/contracts/utils/Array.sol"` just work as long as OpenZeppelin is installed using npm.

Until then there are several ways to work around this problem:

1. Use [import remapping](https://docs.soliditylang.org/en/latest/path-resolution.html#import-remapping).
    For example `solc @openzeppelin=node_modules/@openzeppelin contract.sol`.
    As long as the remapping target is not an absolute path, this keeps the project portable.
    The downside is the need to remap libraries individually.
2. Create symbolic links inside the base path.
    For example a link named `@openzeppelin` pointing at `node_modules/@openzeppelin`.
    This method is recommended if the libraries are installed outside of base path because the link target path is not included in contract metadata.

    This is the convention used by dapp-tools.
    Your own contracts reside in `src/` subdirectory while libraries are git submodules inside `lib/`.

### Q: What happens if my path has so many `..` segments that it crosses the filesystem boundary?
In the shell absolute paths with extra leading `..` segments (e.g. `/../../project/contract.sol`) are valid.
There's no directory above `/` so the extra segments are simply removed (`/project/contract.sol`).
The same thing happens with internal `..` segments.
E.g. `/home/user/../../../project/contract.sol` is just `/project/contract.sol`.
This is the behavior you can expect when passing paths to the compiler on the command line.

The same principle applies to imports except you have to remember that there are two different filesystem boundaries: in the VFS and in the actual filesystem.

**Example**: Let's consider the following imports placed in a source unit named `contract.sol` loaded from `/home/user/project/`:
```solidity
import "../node_modules/@openzeppelin/contracts/utils/Array.sol";
import "utils/../../node_modules/@openzeppelin/contracts/utils/Array.sol";
```
Assuming that the `utils/` directory exists, it might seem that both should result in the same file being loaded by the default import callback.
This is, however, not the case.
Since there are no path separators in `contract.sol`, the importing source unit name used to resolve the first import is empty.
As a result the `../` segment crosses the VFS boundary and is ignored.
The resulting source unit name is `node_modules/@openzeppelin/contracts/utils/Array.sol` and the callback will expect to find it in `/home/user/project/node_modules/@openzeppelin/contracts/utils/Array.sol`.
The second import is a direct one so the source unit name passed to the callback unchanged.
Since it is being resolved in the actual filesystem, the resulting path is `/home/user/node_modules/@openzeppelin/contracts/utils/Array.sol`.

While it is usually easy for the user to notice that something is wrong with a relative import that crosses the VFS boundary due to resulting errors, the cause not necessarily obvious.
The compiler might start issuing warning about such imports in the future.
