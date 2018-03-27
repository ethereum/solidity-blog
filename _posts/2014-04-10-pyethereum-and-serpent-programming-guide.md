---
id: 39
title: Pyethereum and Serpent Programming Guide
date: 2014-04-10T09:50:34+00:00
author: Vitalik Buterin
layout: post
guid: http://blog.ethereum.org/?p=39
permalink: /2014/04/10/pyethereum-and-serpent-programming-guide/
dsq_thread_id:
  - "2790050093"
---
<i>The content of this tutorial is intended to apply to PoC5. Most of the instructions given below will not work in the older PoC4 implementations of AlethZero (C++) and Ethereal (Go)</i>

Over the last few weeks, we have made a large number of changes to the Ethereum protocol. POC4, introducing a large body of changes made by Gavin Wood and myself, was <a href="http://blog.ethereum.org/2014/03/20/the-latest-evm-ethereum-is-a-trust-free-closure-system/">announced as an informal description</a> two weeks ago, and has been formally specified in Gavin Wood’s “yellow paper” at <a href="http://gavwood.com/Paper.pdf">http://gavwood.com/Paper.pdf</a>. The protocol spec did change substantially, but at the same time things are solidifying; we know why we want transactions to pay fees instead of contracts, so that’s not likely to change, we know that code and data will be separate, and the byte-based code and memory and 32-byte-block-based stack and storage are unlikely to change, and we know that the workings of the EVM in general will be similar to what they are now instead of some kind of elaborate Merkle-code-tree construction. POC4 has given myself what I wanted out of <a href="http://blog.ethereum.org/2014/02/03/introducing-ethereum-script-2-0/">Ethereum Script 2</a>, Gavin a much more optimization-friendly VM architecture, and users a <a href="https://github.com/ethereum/cpp-ethereum/wiki/LLL-Examples-for-PoC-4">shiny new currency</a>. Meanwhile, Chen Houwu, Heiko Kees and Konrad Feldmeier have taken the lead as our main Python developers, and the networking side of the pyethereum client is getting to the point where it is getting ready to talk to Go and C++. At the same time, aside from all of the managerial tasks that are part and parcel of having a key role in a large project, I have taken it upon myself to bring up to speed the pyethereum VM implementation and the compiler for the HLL programming language.

The purpose of this post will be to provide an in-depth technical tutorial into the workings of pyethereum and Serpent, and show you how you can start writing the tools to build your own contracts and applications. The Bitcoin Expo hackathon is happening today and tomorrow, so feel free to make an Ethereum contract your project if you are among those attending.

First of all, importantly, HLL is no longer called HLL; the language is now called Serpent. Why? Because it’s basically Python.

With recent upgrades to the compiler, Serpent is now a highly feature-filled programming language, with powerful features including:
<ul>
	<li>Arrays (eg. x[0] = 123)</li>
	<li>Array literals (eg. x = [ 34, 56, 78 ])</li>
	<li>Nested arrays (eg. z = [ 34, [ 5, 6 ], y ])</li>
	<li>Hex support (eg. receiving_address = 0xb156066c2978d7b9188f2467b815d4c62ae32fe2)</li>
	<li>String support (eg. x = "cow")</li>
	<li>Inline message calling (eg. usdprice = eth * msg(ethcontract,0,tx.gas-100,[500],1))</li>
	<li>Out of line message calling (eg. msg(multifeedcontract,0,tx.gas-100,inparray,5,outarray,5))</li>
	<li>Simple value sending operation (eg. send(receiver, value, tx.gas-100))</li>
	<li>Returning values (eg. return(45) and return([10,20,30,40],4))</li>
	<li>Treating message data and storage as arrays (eg. contract.storage[1000] = msg.data[0])</li>
	<li>Byte arrays (eg. x = bytes(100), setch(x,45,"c")), y = getch(x,45)</li>
</ul>
The intent of the Serpent language is to make programming smart contracts and decetralized applications in Ethereum as easy as programming boring command line apps is in Python. The language is designed to be maximally clean and maximally simple, combining the benefits of a compiled language with an easy-to-use coding experience. Just the logic, and nothing but the logic. Unfortunately, floating point numbers are missing, as are higher-order constructs like list comprehensions and closures, but aside from that Serpent has basically everything that you need.
<h3>Getting Started</h3>
So how do you code in Serpent? The first step is to set up the development and execution environment. To do this, first download two libraries: <a href="http://github.com/ethereum/pyethereum">pyethereum</a> and <a href="http://github.com/ethereum/pyethereum">serpent</a>. The simplest way to download is to either download the zip files from Github and unpack them, or run git clone <a href="http://github.com/ethereum/pyethereum">http://github.com/ethereum/pyethereum</a> and git clone<a href="http://github.com/ethereum/serpent">http://github.com/ethereum/serpent</a>. Then, enter the pyethereum directory, and run sudo python setup.py install to install pyethereum to your system, and do the same with serpent.

Now that the software is downloaded, let’s get right to it. To start off, try this:

&nbsp;

> serpent compile_to_assembly 'x = 5'

["$begincode_0.endcode_0", "DUP", "MSIZE", "SWAP", "MSIZE", "$begincode_0", "CALLDATACOPY", "RETURN", "~begincode_0", "#CODE_BEGIN", 5, 0, "MSTORE", "#CODE_END", "~endcode_0"]

The compile_to_assembly instruction compiles the code down into an intermediate human-readable “assembly language” format rather than plain old bytecode. Using plain old serpent compile would give you the much more incomprehensible but compact 6005515b525b600a37f26005600054. In this case, the “core” of the code is [5, 0, "MSTORE"], putting the value 5 into memory slot 0, and the rest of the code basically says to return a contract containing that code. Another command that you may find useful is serpent get_vars; this will give you a list of all the variables together with their associated memory indices. In this case, you get {'x': 0}, meaning that the compiler is choosing to use the memory index 0 to store the variable x. The last interesting command is parse to convert Serpent into an intermediate high-level parse tree. Now, since Serpent is a programming language, we want to run programs, and so ideally we would like to actually create contracts and run them as quickly as possible. Let’s try that. First, open a file, call it “namecoin.se“, and put the following code into it:

if !contract.storage[msg.data[0]]:
contract.storage[msg.data[0]] = msg.data[1]
return(1)
else:
return(0)

This is the two-line Namecoin example that we love so much, but embellished with return values to make it easier to work with for this tutorial. Typing serpent compile namecoin.se should give:

&nbsp;

6025515b525b600a37f260003556601b596020356000355760015b525b54602052f260255860005b525b54602052f2

Now, let’s see if we can actually get the code running. To do that, the first step is actually to create for ourselves an account. The process here is almost exactly the same as in my Python Bitcoin library <a href="https://github.com/vbuterin/pybitcointools">pybitcointools</a>; in general, anyone who is familiar with pybitcointools should feel right at home in pyethereum, although unfortunately in pyethereum it was not really practical to stick to pybitcointools’ “no classes” mantra in the code. The first step is to generate a private key:

&nbsp;

> pyethtool sha3 cow

c85ef7d79691fe79573b1a7064c19c1a9819ebdbd1faaab1a8ec92344438aaf4

In production code, you should obviously replace “cow” with an actually secure password. If you want your account to be a “brainwallet” that you can easily remember, my main advice is to prepend a username, eg. “vbuterin:bl@hbl@hm0nk33y#!$!%”, ensuring that attackers need to target you individually instead of performing a blanket attack on everyone simultaneously; assuming 10000 brainwallet users this reduces your risk from a trial-and-error attack by 99.99%.

If you want to use your key later, on any standard Linux shell you can also type in key=`pyethtool sha3 cow`, and then use$key to use the key thereafter. We’ll use that format here from now on, so if you are following along then you should also do both:

&nbsp;

> key=`pyethtool sha3 cow`

> code=`serpent compile namecoin.se`

So now, let’s keep going.

> addr=`pyethtool privtoaddr $key`

> echo $addr

cd2a3d9f938e13cd947ec05abc7fe734df8dd826

Now, we create a new genesis block, and we'll set the initial endowment to 1018 wei (1 ether) for your address.

&nbsp;

> genesis=`pyethtool mkgenesis $addr 1000000000000000000`

> echo $genesis

f8b2f8aea00000000000000000000000000000000000000000000000000000000000000000a01dcc4de8dec75d7aab85b567b6ccd41ad312451b948a7413f0a142fd40d49347940000000000000000000000000000000000000000a0bcddd284bf396739c224dba0411566c891c32115feb998a3e2b4e61f3f35582a80834000008087038d7ea4c68000830f4240808080a004994f67dc55b09e814ab7ffc8df3686b4afb2bb53e60eae97ef043fe03fb829c0c0

Now that we have that out of the way, we can get to actually doing stuff to the block. The only way to do anything in a blockchain-based architecture, in general, is to create and apply a transaction. Here, we will need multiple transactions: the first to create the contract, and then the latter ones to actually use it. Here's contract creation:

&nbsp;

> unsignedtx=`pyethtool mkcontract 0 0 $code`

> echo $unsignedtx

f83c8085e8d4a510008227108080af6025515b525b600a37f260003556601b596020356000355760015b525b54602052f260255860005b525b54602052f2

> tx=`pyethtool sign $unsignedtx $key`

> echo $tx

f87f8085e8d4a510008227108080af6025515b525b600a37f260003556601b596020356000355760015b525b54602052f260255860005b525b54602052f21ca04565b5a48b29ef623ad2caffe0917a3fc6a6f1b50f1df06876f3caa6fb4957c6a0123c928257c1f248fb3d362c125a0aea091ab08467efb52f8c3676ca73d727bf

Or, the easier way:

&nbsp;

> tx=`pyethtool mkcontract 0 0 $code | pyethtool -s sign $key`

> echo $tx

f87f8085e8d4a510008227108080af6025515b525b600a37f260003556601b596020356000355760015b525b54602052f260255860005b525b54602052f21ca04565b5a48b29ef623ad2caffe0917a3fc6a6f1b50f1df06876f3caa6fb4957c6a0123c928257c1f248fb3d362c125a0aea091ab08467efb52f8c3676ca73d727bf

The first field in mkcontract is a nonce, which must be equal to the number of transactions you already sent from that account. The purpose of requiring a nonce is to prevent replay attacks; otherwise, if you sent Bob 200 ether, Bob could simply replay that transaction over and over again until you run out of money, whereas here due to the nonce requirement the transaction can only go through once. The second field is the amount of ether to send (in the case of contract creation, the amount of ether to initially provide to the contract), and the third field is the code. Note that the Transaction.contract<i>function call</i> also has two more fields between value and recipient: gasprice and startgas. Pyethtool is nice to you and initializes these values to 1 szabo (ie. 1012 wei or one millionth of an ether) per gas and 10000 gas, respectively. This will give you a theoretical maximum of 10000 computational steps for the code to run, although in practice it may run out after 1000 if you use many expensive operations. Finally, once you create the transaction, you need to sign it with your private key.

Once that's done, we just, well:

&nbsp;

> pyethtool applytx $genesis $tx

{"result": "da7ce79725418f4f6e13bf5f520c89cec5f6a974", "block": "f9017ef8d0a00000000000000000000000000000000000000000000000000000000000000000a01dcc4de8dec75d7aab85b567b6ccd41ad312451b948a7413f0a142fd40d49347940000000000000000000000000000000000000000a00bcec36bf7ffc27418b1746986574526efeb09b34f733039749f291f778d4aaca03575f60ad6c929d7c98a50a12ff1ef9b07ecf3182e74962872064648a66f3da0834000008087038d7ea4c68000830f42408204b08080a004994f67dc55b09e814ab7ffc8df3686b4afb2bb53e60eae97ef043fe03fb829f8a9f8a7b881f87f8085e8d4a510008227108080af6025515b525b600a37f260003556601b596020356000355760015b525b54602052f260255860005b525b54602052f21ca04565b5a48b29ef623ad2caffe0917a3fc6a6f1b50f1df06876f3caa6fb4957c6a0123c928257c1f248fb3d362c125a0aea091ab08467efb52f8c3676ca73d727bfa00bcec36bf7ffc27418b1746986574526efeb09b34f733039749f291f778d4aac8204b0c0"}

This gives you two values. The first is the address of the contract, and the second is the new block data. Note that the block data does not represent the entire block; there is also the state data hidden in the statedb folder. Hence, if you try to deserialize the block on a fresh machine it likely will not work. From the values returned, set the first value to contract and the second to med so we can use them later. Now, we need to craft a transaction to actually use this contract. Suppose we want to register "george" to 45. To do that, however, we first need to do another annoying chore: package up the data. Fortunately, the serpent compiler has a utility for doing just that:

&nbsp;

> data=`echo '["george",45]' | serpent -j encode_datalist`

> echo $data

000000000000000000000000000000000000000000000000000067656f726765000000000000000000000000000000000000000000000000000000000000002d

The namecoin contract takes data in two fields, the key and the value, so we simply put them into a JSON array and use Serpent to encode it. The encoder can accept strings and numbers as the individual elements in the array. Note that unfortunately Python's JSON decoder requires double quotes for internal strings; "['george',45]" would not work.

Now, we do this:

&nbsp;

> tx2=`pyethtool mktx 1 $contract 0 $data | pyethtool -s sign $key`

> echo $tx2

f8a50185e8d4a5100082271094da7ce79725418f4f6e13bf5f520c89cec5f6a97480b840000000000000000000000000000000000000000000000000000067656f726765000000000000000000000000000000000000000000000000000000000000002d1ba064363844c718f0f38907d39508adb2c2b9134e52e7d436fb20965044c01f41c2a0e1123d26cf810c4ef9d397974e2fc336d16e452d71df3c3d7245b40ed12c603b

And:

&nbsp;

> pyethtool applytx $med $tx2

{"result": "0000000000000000000000000000000000000000000000000000000000000001", "block": "f9024ef8d0a00000000000000000000000000000000000000000000000000000000000000000a01dcc4de8dec75d7aab85b567b6ccd41ad312451b948a7413f0a142fd40d49347940000000000000000000000000000000000000000a066d2524d921fadb5056983cf4bb215d339cdaeb7048b8913bfdf8fe867eb5682a0d669d3b5cfb150e4ef7f900cc613b0231abc8551544c389ddcd6668f784c4cb3834000008087038d7ea4c68000830f4240820a8f8080a004994f67dc55b09e814ab7ffc8df3686b4afb2bb53e60eae97ef043fe03fb829f90178f8a7b881f87f8085e8d4a510008227108080af6025515b525b600a37f260003556601b596020356000355760015b525b54602052f260255860005b525b54602052f21ca04565b5a48b29ef623ad2caffe0917a3fc6a6f1b50f1df06876f3caa6fb4957c6a0123c928257c1f248fb3d362c125a0aea091ab08467efb52f8c3676ca73d727bfa00bcec36bf7ffc27418b1746986574526efeb09b34f733039749f291f778d4aac8204b0f8cdb8a7f8a50185e8d4a5100082271094da7ce79725418f4f6e13bf5f520c89cec5f6a97480b840000000000000000000000000000000000000000000000000000067656f726765000000000000000000000000000000000000000000000000000000000000002d1ba064363844c718f0f38907d39508adb2c2b9134e52e7d436fb20965044c01f41c2a0e1123d26cf810c4ef9d397974e2fc336d16e452d71df3c3d7245b40ed12c603ba066d2524d921fadb5056983cf4bb215d339cdaeb7048b8913bfdf8fe867eb5682820a8fc0"}

Registration successful! The result here is two values, just as before: the first is the new block state, and the second is the response returned by the contract. Based on the definition of the contract above, "1" means success. Now, just to be sure, let's set end to the block hex returned by the previous command and peek at the state:

&nbsp;

> pyethtool getstate $end

{'nonce': '\x04\x99Og\xdcU\xb0\x9e\x81J\xb7\xff\xc8\xdf6\x86\xb4\xaf\xb2\xbbS\xe6\x0e\xae\x97\xef\x04?\xe0?\xb8)', 'min_gas_price': 1000000000000000L, 'extra_data': '', 'state_root': 'f\xd2RM\x92\x1f\xad\xb5\x05i\x83\xcfK\xb2\x15\xd39\xcd\xae\xb7\x04\x8b\x89\x13\xbf\xdf\x8f\xe8g\xebV\x82', 'difficulty': 4194304L, 'timestamp': 0L, 'number': 0L, 'gas_used': 2703L, 'coinbase': '0000000000000000000000000000000000000000', 'tx_list_root': '\xd6i\xd3\xb5\xcf\xb1P\xe4\xef\x7f\x90\x0c\xc6\x13\xb0#\x1a\xbc\x85QTL8\x9d\xdc\xd6f\x8fxLL\xb3', 'state': {'0000000000000000000000000000000000000000': {'nonce': 0L, 'balance': 2703000000000000L, 'storage': {}, 'code': ''}, 'da7ce79725418f4f6e13bf5f520c89cec5f6a974': {'nonce': 0L, 'balance': 0L, 'storage': {113685359126373L: 45L}, 'code': '60003556601b596020356000355760015b525b54602052f260255860005b525b54602052f2'}, 'cd2a3d9f938e13cd947ec05abc7fe734df8dd826': {'nonce': 2L, 'balance': 997297000000000000L, 'storage': {}, 'code': ''}}, 'uncles_hash': '\x1d\xccM\xe8\xde\xc7]z\xab\x85\xb5g\xb6\xcc\xd4\x1a\xd3\x12E\x1b\x94\x8at\x13\xf0\xa1B\xfd@\xd4\x93G', 'prevhash': '\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00', 'gas_limit': 1000000L}

You can see the contract account near the beginning of the state description, with "george" registered to 45 as expected. We're done! As an exercise, try constructing two more transactions, one registering "george" to 60 and another registering "harry" to 80. If you apply them all sequentially after these two, the one registering "george" to 60 should return 0, but the one registering "harry" to 80 should succceed.
<h3>Doing it in Python</h3>
That's pyethtool, the command line utility. Now, how does it work using pyethereum itself? As it turns out, it's surprisingly easy. Here's the session:

&nbsp;

&gt;&gt;&gt; import serpent

&gt;&gt;&gt; from pyethereum import transactions, blocks, processblock, utils

&gt;&gt;&gt; code = serpent.compile(open('namecoin.se').read())

&gt;&gt;&gt; key = utils.sha3('cow')

&gt;&gt;&gt; addr = utils.privtoaddr(key)

&gt;&gt;&gt; genesis = blocks.genesis({ addr: 10**18 })

&gt;&gt;&gt; tx1 = transactions.contract(0,10**12,10000,0,code).sign(key)

&gt;&gt;&gt; result, contract = processblock.apply_tx(genesis,tx1)

&gt;&gt;&gt; tx2 = transactions.Transaction(1,10**12,10000,contract,0,serpent.encode_datalist(['george',45])).sign(key)

&gt;&gt;&gt; result, ans = processblock.apply_tx(genesis,tx2)

&gt;&gt;&gt; serpent.decode_datalist(ans)

[1]

&gt;&gt;&gt; genesis.to_dict()

'nonce': '\x04\x99Og\xdcU\xb0\x9e\x81J\xb7\xff\xc8\xdf6\x86\xb4\xaf\xb2\xbbS\xe6\x0e\xae\x97\xef\x04?\xe0?\xb8)', 'min_gas_price': 1000000000000000L, 'extra_data': '', 'state_root': '', 'difficulty': 4194304, 'timestamp': 0, 'number': 0, 'gas_used': 2712L, 'coinbase': '0000000000000000000000000000000000000000', 'tx_list_root': '\x17\x90\x87\x966\xbdb!\x14|R\xb0&amp; \xb04\x90\xb9bs\x12\x85\x90\xdaB\xed\x83n*\x8eE\x8e', 'state': {'0000000000000000000000000000000000000000': {'nonce': 0L, 'balance': 2712000000000000L, 'storage': {}, 'code': ''}, 'da7ce79725418f4f6e13bf5f520c89cec5f6a974': {'nonce': 0L, 'balance': 0L, 'storage': {113685359126373L: 45L}, 'code': '60003556601e596020356000355760015b525b54602052f260285860005b525b54602052f2'}, 'cd2a3d9f938e13cd947ec05abc7fe734df8dd826': {'nonce': 2L, 'balance': 997288000000000000L, 'storage': {}, 'code': ''}}, 'uncles_hash': '\x1d\xccM\xe8\xde\xc7]z\xab\x85\xb5g\xb6\xcc\xd4\x1a\xd3\x12E\x1b\x94\x8at\x13\xf0\xa1B\xfd@\xd4\x93G', 'prevhash': '\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00', 'gas_limit': 1000000}

&gt;&gt;&gt; genesis.get_balance(addr)

997288000000000000L

&gt;&gt;&gt; genesis.get_storage_data(contract,'george')

45L

Another important command is processblock.debug = 1; this starts printing code execution step by step, helping you debug what is wrong in your contract code - or my pyethereum VM or Serpent implementation!
<h3>Getting into the Code</h3>
So that's your introduction to how to use pyethereum. Now, let's get into the most fun part, writing contracts. For reading efficiency, let's provide the Namecoin contract again:

if !contract.storage[msg.data[0]]:
contract.storage[msg.data[0]] = msg.data[1]
return(1)
else:
return(0)

What does this contract do? Essentially, this contract implements a name registration database by simply using that as the sole function of the long-term storage of the contract. Contract code theoretically has three places to put data: stack, memory and storage. Of those three, stack and memory are used implicitly in Serpent to support arithmetic and variables, but long-term storage is the only one that survives once execution is over. Here, when you register "george" to 45, the contract first checks ifcontract.storage["george"] is not nonzero, ie. is zero. If it is, then it sets that storage index to the value provided, 45, and then returns 1. If it is not, then it returns zero. Note that this contract has no way for other contracts to access it; it is only really usable by external applications. More advanced name registries would have an API for contracts to fetch the data associated with a name as well.

Now, on to a more intricate example:

init:
contract.storage[0xcd2a3d9f938e13cd947ec05abc7fe734df8dd826] = 1000000
code:
if msg.datasize == 1:
addr = msg.data[0]
return(contract.storage[addr])
else:
from = msg.sender
fromvalue = contract.storage[from]
to = msg.data[0]
value = msg.data[1]
if fromvalue &gt;= value:
contract.storage[from] = fromvalue - value
contract.storage[to] = contract.storage[to] + value
return(1)
else:
return(0)

This is the "currency contract", or more precisely an embellished version of it with return values to make debugging easier. This contract is interesting for several reasons. First, it has an initialization step, which gets called when the contract is first made. This initializes an account with 1000000 currency units owned by that account.

After that, there are two code paths. First, incoming messages might contain only one data field. In that case, these messages are treated as balance queries, and simply return the balance of the queried address. Note that msg.data[0] provides the integer at bytes 0...31 of the transaction data, msg.data[1] provides the integer at bytes 32...63, and so forth. This is a convenience introduced in Serpent; the underlying transaction data is all byte-based. Incidentally, this is why we needed to use Serpent's encode_datalist function to generate the transaction data.

Second, incoming messages might contain two data fields. In that case, the messages are treated as requests to send to that address. The sender is inferred from the sender of the message, and the recipient and the value are taken from the first two fields (ie. first 64 bytes) in msg.data. If there is enough money to transfer, it transfers the money and returns 1; otherwise it returns 0.

<b>Challenge</b>: create a currency contract which takes a fee, denominated in its internal currency, from every transaction, and refunds a small amount of ether to everyone sending a successful transaction, so people (or contracts) who want to deal in this currency would not have to worry about simultaneously maintaining currency and ether balances themselves. The contract would also include a third transaction type, perhaps taking 0 arguments, through which someone can buy internal currency units from the contract by sending it ether. The contract should keep track of two variables: its own balance in its currency, and its ether balance, and it should dynamically adjust the transaction fee and the exchange rate in order to keep both its ether balance and its internal currency balance in bal- uh, in an approximate equilibrium.
<h3>Contracts Calling Contracts</h3>
This is a proprietary data feed contract:

owner = 0xcd2a3d9f938e13cd947ec05abc7fe734df8dd826
if msg.sender == owner and msg.datasize == 2:
contract.storage[msg.data[0]] = msg.data[1]
return(1)
else:
return(contract.storage[msg.data[0]])

This contract is designed to work as a key/value that can be edited only by its owner, but also also allows anyone to query its contents; the point is for the owner to use various storage indices to record changing data like the USD price of ether. Here, there are two main "clauses" in the contract, one for modifying storage which triggers if a key and a value are provided and the message originates from the contract's owner, and the other for just reading storage. The msg.datasize variable tells you the number of 32-byte data fields there is in the message data. There are no particularly new features here; this contract is actually fairly simple, and I encourage you to first follow and make sure you understand the logic involved and then play with the contract, instantiating it in a block and then pushing set and query transactions to it.

The interesting part, however, comes when we use this contract inside of another contract. Meet this monstrosity, a hedging contract:

if !contract.storage[1000]:
contract.storage[1000] = msg.sender
contract.storage[1002] = msg.value
contract.storage[1003] = msg.data[0]
contract.storage[1004] = msg.data[1]
return(1)
elif !contract.storage[1001]:
ethvalue = contract.storage[1002]
if msg.value &gt;= ethvalue:
contract.storage[1001] = msg.sender
datasource = contract.storage[1003]
dataindex = contract.storage[1004]
othervalue = ethvalue * msg(datasource,0,tx.gas-100,[dataindex],1)
contract.storage[1005] = othervalue
contract.storage[1006] = block.timestamp + 86400
return([2,othervalue],2)
else:
datasource = contract.storage[1003]
dataindex = contract.storage[1004]
othervalue = contract.storage[1005]
ethvalue = othervalue / msg(dataindex,0,tx.gas-100,[datasource],1)
if ethvalue &gt;= contract.balance:
send(contract.storage[1000],contract.balance,tx.gas-100)
return(3)
elif block.timestamp &gt; contract.storage[1006]:
send(contract.storage[1001],contract.balance - ethvalue,tx.gas-100)
send(contract.storage[1000],ethvalue,tx.gas-100)
return(4)
else:
return(5)

This contract is bulky because it's designed to be more testing-friendly; an optimal implementation is roughly half the size. The contract works as follows:

1. Party A sends in X ether alongside a data feed contract D and a currency code C as data items, and is registered at contract storage index 1000. X, D and C are registered in contract storage indices 1002, 1003 and 1004. In this case, suppose that the currency code represents USD.

2. Party B sends in X ether, and is registered at contract storage index 1001. The contract then calls D with data C to determine the price of ether in the given currency, and uses this to compute V, the amount of value in USD sent by each party. V is stored at index 1005, and an expiry time set to 24 hours in the future is stored at index 1006.

3. Maybe, the price of ether in USD drops by more than 50%. If this happens, then there is not enough ether in the contract altogether to pay V USD. To prevent this, as soon as the price slips under the 50% mark, anyone (usually A) can ping the contract to withdraw all 2X ether into A's address and thereby recover to A's address almost all of the amount, as measured in USD, that A put in, and leave B with nothing. If this happens, the contract returns 3.

4. Otherwise, after one day, anyone can send a transaction to "ping" the contract and cause it to send V USD worth of ether to A and the remaining ether to B, returning 4.

5. If there is no "margin call" or "expiry" event, then a ping to the contract does nothing and returns 5.

The point of the hedging contract is that A benefits by always getting back the same quantity of USD that he put in, and B benefits if he believes that the value of ether will go up, since a 10% rise in the ether price will, in this circumstance, give him a 20% profit. USD can of course be substituted with anything, including CNY, gold or the consumer price index.

The important new features explored here are msg, send and array literals. msg and send are both ways of sending message to other contracts. The syntaxes are:

&nbsp;

send(to, value, gas)

out = msg(to¸ value, gas, datastart, datalength)

msg(to, value, gas, datastart, datalength, outstart, outlength)

Send is simpler, assuming that all you want to do is send money with no bells and whistles involved. The latter two are equivalent ways of sending a message to another contract, differing only in how they handle the output: the first caps output to 32 bytes and sticks it straight into a variable, whereas the second takes in two arguments for the position in memory where to dump the output. The "output" of a message is blank if the recipient is not-yet-existent, an externally owned account, or does not explicitly specify a return value, and if the output does specify a return value then the output is that value ("value" in this context being an arbitrary-length byte array, not a 32-byte number). These two are thus both ways of saying the same thing:

d = array(3)
d[0] = 5
d[1] = 10
d[2] = 15
x = msg(A, B, C, d, 3)

And:

d = array(3)
d[0] = 5
d[1] = 10
d[2] = 15
w = array(1)
msg(A, B, C, d, 3, w, 1)
x = w[0]

In the contract example above, we used the data feed contract to provide the price of ether in USD, and then directly plugged it into the formula othervalue = ethvalue * msg(datasource,0,tx.gas-100,[dataindex],1).

Array literals are another nice convenience feature; the truly optimal way to write the above code is as follows:

x = msg(A, B, C, [5, 10, 15], 3)

Note that you unfortunately still need to specify the array length. However, here the array itself is created and referenced all inline, without needing to manually set things up. All of the magic is done by the Serpent compiler.

So that's basically it for today. What might you want to code in Serpent? Well, here are a few possibilities:

1. <a href="http://blog.ethereum.org/2014/03/28/schellingcoin-a-minimal-trust-universal-data-feed/">SchellingCoin</a>

2. A contract-based implementation of <a href="http://just-dice.com/">JustDice</a>.

3. Some skeleton code for a decentralized organization.

4. A board game (eg. chess, Go)

5. A decentralized exchange, with a contract-based order book, between ether and the sub-currency contract given above.

6. Any of the other examples in our <a href="https://github.com/ethereum/wiki/wiki/%5BEnglish%5D-White-Paper">whitepaper</a>

Enjoy, and have fun! Also, if you do find any bugs in pyethereum or Serpent, please be sure to point them out.

See also: <a href="https://github.com/ethereum/wiki/wiki/%5BEnglish%5D-Serpent-programming-language-operations">list of Serpent language operations</a>