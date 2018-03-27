---
id: 1832
title: Final Steps
date: 2015-07-27T20:20:01+00:00
author: Stephan Tual
layout: post
guid: https://blog.ethereum.org/?p=1832
permalink: /2015/07/27/final-steps/
dsq_thread_id:
  - "3976730105"
---
An update as promised: all systems are now ‘Go’ on the technical side (pun intended) and we intend to release Frontier this week. 

Thank you to everyone who provided feedback on my previous blog post. What became apparent is that prior to the big day, many of you wanted to know more about what the sequence of events would exactly be, and how to prepare your machine for the release.


# A transparent and open release

Frontier users will need to first generate, then load the Genesis block into their Ethereum client. The Genesis block is pretty much a database file: it contains all the transactions from the Ether sale, and when a user inputs it into the client, it represents their decision to join the network under its terms: it is the first step to consensus. 

Because the ether pre-sale took place entirely on the bitcoin blockchain, its contents are <a href="https://blog.ethereum.org/2014/08/08/ether-sale-a-statistical-overview/">public</a>, and anyone can generate and verify the Genesis block. In the interest of decentralization and transparency, Ethereum will not provide the Genesis block as a download, but instead has created an open source script that anyone can use to generate the file, a link to which can be found later on in this article.

Since the script is already available and the release needs to be coordinated, an argument to the script has to be provided in order to 'kick off' Frontier in unison. But how can we do this <em>and</em> stay decentralized? 

The argument needs to be a random parameter that no one, not even us, can predict. As you can imagine, there aren’t too many parameters in the world that match this criteria, but a good one is the hash of a future block on the Ethereum testnet. We had to pick a block number, but which one? 1,028,201 turns out to be both prime and palindromic, just the way we like it. So #1028201 is it.


*Sequence of steps to the release:*

* Final steps to the release revealed: You are reading this now.
* Block #1028201 is formed on the Ethereum tesnet, and is given a hash.
* The hash is used by users around the world as a unique parameter to the Genesis block generation script.




# What you can do today

First, you’ll need the client installed, I’ll use Geth as an example, but the same can be achieved with <a href="https://github.com/ethereum/cpp-ethereum/wiki">Eth</a> (the C++ implementation of Ethereum). Geth installation instructions for Windows, Linux and OSX can be found on our <a href="https://github.com/ethereum/go-ethereum/wiki/Building-Ethereum">wiki</a>.

Once you have installed a client, you need to download the python script that generates the Genesis file. It’s called 'mk_genesis_block.py', and can be downloaded <a href="https://raw.githubusercontent.com/ethereum/genesis_block_generator/master/mk_genesis_block.py">here</a>.

Depending on your platform, you can also download it from the console by installing curl and running; 

```
curl -O https://raw.githubusercontent.com/ethereum/genesis_block_generator/master/mk_genesis_block.py
```

This will create the file in the same folder where you invoked the command. You now need to install the pybitcointools created by our very own Vitalik Buterin. You can obtain this through the python package manager pip, so we’ll install pip first, then the tools right afterwards. 

The following instructions should work on OSX and Linux. Windows users, good news, pip ships with the standard <a href="https://www.python.org/downloads/windows/">Python installer</a>. 

```
curl -O https://bootstrap.pypa.io/get-pip.py
sudo python get-pip.py
```

```
sudo pip install bitcoin
```
or (if you had it installed already),
```
sudo pip install --upgrade bitcoin
```

One last step, if you are using Eth, we recently to support the new Genesis block parameter, so you'll need to pick up the correct release of the software to be ready for the big day:


```
cd ~/go-ethereum/
git checkout release/1.0.0
git pull
make geth
```

Those who would like to be ‘as ready as possible’ can follow instructions up to this point, that said, a `git pull` just before the fateful block is probably recommended to operate the newest version of any software. 

If you have been running the clients before:

* Back up your keys (maybe some of them are eligible for Olympic rewards) - they are located in ./ethereum/keystore
* Delete your old chain please (it's located in ./ethereum, delete the following 3 folders only: ./extra, ./state, ./blockchain)
* You can safely leave your ./ethereum/nodes, ./ethereum/history and ./ethereum/nodekey in place
* Having DAGs pregenerated in ./ethash will not hurt, but feel free to delete them if you need the space

For a complete breakdown as to where the config files are located, please check out this <a href="https://forum.ethereum.org/discussion/2114/where-are-my-config-files-go-and-cpp">page</a> on our forums.

Then, it's a matter of waiting for block #1028201, which at the current block resolution time, should be formed approximately Thursday evening GMT+0.

Once 1028201 has formed, its hash will be accessible by querying a node running the testnet using `web3.eth.getBlock(1028201).hash`, however we will also make that value available on this blog as well as all our <a href="https://www.facebook.com/ethereumproject">social</a> <a href="http://forum.ethereum.org/">media</a> <a href="https://twitter.com/ethereumproject">channels</a>.

You will then be able to generate the Genesis block by running:

```
python mk_genesis_block.py --extradata hash_for_#1028201_goes_here > genesis_block.json
```

By default, the script uses Blockr and Blockchain.info to fetch the Genesis pre-sale results. You can also add the `--insight` switch if you’d instead prefer to use the private Ethereum server to obtain this information. If you are facing issues with the script, please raise an issue on its <a href="https://github.com/ethereum/genesis_block_generator/issues">github</a>.


While we will not provide the Genesis block as a file, we will still provide the Genesis block hash (shortly after we generate it ourselves) in order to insure that third party invalid or malicious files are easily discarded by the community.

Once you are satisfied with the generation of the Genesis block, you can load it into the clients using this command:

```
./build/bin/geth --genesis genesis_block.json
```

or:


```
./build/eth/eth --genesis genesis_block.json
```

From there, instructions on creating an account, importing your pre-sale wallet, transacting, etc., can be found on the ‘Getting Started’ Frontier guide at http://guide.ethereum.org/

Note that if you've used Ethereum before, you should generate new keys using a recent (RC) client, and not reuse testnet keys. 


# A couple more things… 

We also would like to give you a bit of heads up on the ‘thawing’ phase -- the period during which the gas limit per block will be set very low to allow the network to grow slowly before transactions can take place. You should expect network instability at the very beginning of the release, including forks, potential abnormal display of information on our http://stats.ethdev.com page, and various Peer to Peer connectivity issues. Just like during the Olympic phase, we expect this instability to settle after a few hours/days. 

We would also like to remind everyone that while we intend to provide a safe platform in the long term, Frontier is a technical release directed at a developer audience, and not a general public release. Please keep in mind that early software is often affected by bugs, issues with instability and complex user interfaces. If you would prefer a more user friendly experience, we encourage you to wait for the future Homestead or Metropolis Ethereum releases. 

Please be especially wary of third party websites and software of unknown origin -- Ethereum will only ever publish software through its github platform at <a href="https://github.com/ethereum/">https://github.com/ethereum/</a>.

Finally, for clarity, it’s important to note that the <a href="https://blog.ethereum.org/2015/05/09/olympic-frontier-pre-release/">Olympic</a> program ended at block 1M this morning, however, the bug bounty is still on --- and will continue until further notice. Security vulnerabilities, if found, should continue to be reported to <a href="https://bounty.ethdev.com/">https://bounty.ethdev.com/</a>.


--
### Updates
*27/07/15*: added instructions for users upgrading from previous installations
*28/07/15*: minor edits, added link to script github
*29/07/15*: added recommendation to create new keys and not reuse testnet ones