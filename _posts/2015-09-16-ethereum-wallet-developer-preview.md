---
id: 2093
title: 'Ethereum Wallet &#8211; Developer Preview'
date: 2015-09-16T20:43:39+00:00
author: Fabian Vogelsteller
layout: post
guid: https://blog.ethereum.org/?p=2093
permalink: /2015/09/16/ethereum-wallet-developer-preview/
dsq_thread_id:
  - "4135878052"
---
<span style="font-weight: 400">We are happy to announce our very first developer-preview of the Ethereum Wallet ÐApp. The point of this release is to gather feedback, squash bugs and, most importantly, get the code audited</span>.

<span style="font-weight: 400">Please note that this is a developer-preview and </span><b>not</b><span style="font-weight: 400"> the final release. We </span><span style="font-weight: 400">advise</span><span style="font-weight: 400"> you to be </span><span style="font-weight: 400">extremely</span><span style="font-weight: 400"> careful putting large amount of Ether in the wallet contracts.</span><i><span style="font-weight: 400"> Using the wallet on the mainnet should only be done with small amounts!</span></i>

<a href="https://blog.ethereum.org/wp-content/uploads/2015/09/Screen-Shot-2015-09-16-at-11.05.36.png"><img src="https://blog.ethereum.org/wp-content/uploads/2015/09/Screen-Shot-2015-09-16-at-11.05.36.png" alt="Wallet - dashboard" /></a>

&nbsp;

<span style="font-weight: 400">As Steve Ballmer once said </span><b>Developers! Developers! Developers! </b><span style="font-weight: 400">And note that this is exactly our target </span><span style="font-weight: 400">audience</span><span style="font-weight: 400">, don’t blindly trust us and we ask (</span><b>and advise!</b><span style="font-weight: 400">) you to take a thorough look through the code in the <a href="https://github.com/ethereum/meteor-dapp-wallet" target="_blank">ethereum wallet repository</a>!</span>

<span style="font-weight: 400">If you’d like to build the wallet yourself, you need to head over to the Mist repository, use the <a href="https://github.com/ethereum/mist/tree/wallet" target="_blank">wallet branch</a> and follow the instructions in the </span><span style="font-weight: 400"><a href="https://github.com/ethereum/mist/tree/wallet#mist-browser" target="_blank">Readme</a></span><span style="font-weight: 400">. </span>
<h3>Reporting Issues</h3>
If you have any issues with the wallet, open the developer console of the wallet (Menu -&gt; Developer -&gt; Toggle console) and provide the logs from there and the terminal where you started <code>geth</code> or <code>eth</code> from. <strong>Please report all issues to the <a href="https://github.com/ethereum/meteor-dapp-wallet" target="_blank">wallet repository</a>.</strong>
<h2>How to run it?</h2>
First download the binary for you OS: (**Note**: You can find newer releases <a href="https://github.com/ethereum/mist/releases">here</a>)
<ul>
	<li><a href="https://github.com/ethereum/mist/releases/download/0.2.6/Ethereum-Wallet-darwin-x64-0-2-6.zip">Mac OS X</a></li>
	<li><a href="https://github.com/ethereum/mist/releases/download/0.2.6/Ethereum-Wallet-win32-x64-0-2-6.zip">Windows 64bit</a></li>
	<li><a href="https://github.com/ethereum/mist/releases/download/0.2.6/Ethereum-Wallet-win32-ia32-0-2-6.zip">Windows 32bit</a> (Though we probably won't support bundled nodes with 32bit)</li>
	<li><a href="https://github.com/ethereum/mist/releases/download/0.2.6/Ethereum-Wallet-linux-x64-0-2-6.zip">Linux 64bit</a></li>
	<li><a href="https://github.com/ethereum/mist/releases/download/0.2.6/Ethereum-Wallet-linux-ia32-0-2-6.zip">Linux 32bit</a></li>
</ul>
This developer preview doesn't come bundled with a node, as there are a few things still to be finalised, so you still need to start one yourself.

<span style="font-weight: 400">For this developer preview the supported clients are </span><code>geth</code> <span style="font-weight: 400">and </span><code>eth</code><span style="font-weight: 400">. Python is currently not supported because it does not have the required IPC interface to run the wallet.</span>

If you don't have one of these nodes installed yet, <a href="https://ethereum.org/cli" target="_blank">follow the instructions here</a> or download a <a href="https://build.ethdev.com/builds/" target="_blank">pre-build version</a>. Make sure you have updated to the latest version.  and start a node by simply running:

<strong>Go:</strong>
<pre>$ geth</pre>
If you want to unlock an account to be able to transfer add <code>--unlock &lt;my account&gt;</code>, or start a console with <code>$ geth attach</code> and unlock it  using the JavaScript interface: <code>personal.unlockAccount('&lt;my account&gt;')</code>.

<strong>C++:</strong>
<pre>$ eth</pre>
<strong><span style="font-weight: 400"><strong><img class="wp-image-2149 alignright" src="https://blog.ethereum.org/wp-content/uploads/2015/09/Screen-Shot-2015-09-16-at-19.11.43.png" alt="" width="359" height="234" />

It is important to note that the wallet is expecting a fully synced up node.</strong>

In future versions of </span><code><span style="font-weight: 400">geth</span></code><span style="font-weight: 400"> and </span><code><span style="font-weight: 400">eth</span></code><span style="font-weight: 400"> the wallet will make use of the new <a href="https://github.com/ethereum/wiki/wiki/JSON-RPC#eth_syncing" target="_blank">eth_syncing</a> method in the JSON RPC, </span></strong>allowing you to see a sync screen when you start the wallet.
<em>This feature is currently already supported by <code>geth</code> and <code>eth</code> on their develop branches.</em>

Finally start the wallet by clicking the executable!
<h2><a href="https://blog.ethereum.org/wp-content/uploads/2015/09/Screen-Shot-2015-09-16-at-11.05.12.png"><img class="alignnone size-full wp-image-2102" src="https://blog.ethereum.org/wp-content/uploads/2015/09/Screen-Shot-2015-09-16-at-11.05.12.png" alt="Wallet - sending a transaction" width="2272" height="1584" /></a></h2>
<h2>Running on a testnet</h2>
If you want to try the wallet on a testnet you need to start your node with a different network id and probably a different data directory. To make sure the wallet can still connect to your node you manually need to set the IPC path:

<strong>OS X</strong>:
<pre>$ geth --networkdid "1234" --datadir "/some/other/path" --ipcpath "/Users/&lt;username&gt;/Library/Ethereum/geth.ipc"</pre>
<strong>Linux</strong>:
<pre>$ geth --networkdid "1234" --datadir "/some/other/path" --ipcpath "/home/&lt;username&gt;/.ethereum/geth.ipc"</pre>
Additional you should probably provide <a href="https://gist.github.com/frozeman/7e40884bb9722a298912" target="_blank">your own genesis block</a> using the <code>--genesis</code> flag. For more details about the flags <a href="https://github.com/ethereum/go-ethereum/wiki/Command-Line-Options" target="_blank">see the wiki</a>.

<a href="https://blog.ethereum.org/wp-content/uploads/2015/09/Screen-Shot-2015-09-16-at-19.12.13.png"><img src="https://blog.ethereum.org/wp-content/uploads/2015/09/Screen-Shot-2015-09-16-at-19.12.13.png" alt="" /></a>

After the node is started you can simple start the wallet again.
<em>Note</em> that you need to wait sometimes a bit, and click in the button in the corner.

<em>Once you opened the wallet you will see a popup asking you to deploy a wallet contract on your testnet, which will be used as a <strong>code basis</strong> for your future wallet contracts. The main advantage is that it is much cheaper (1.8mio vs 180k gas).
<strong>Note:</strong> Make sure you have the displayed account unlocked and has at least 1 ether.</em>
<h2>Using the wallet</h2>
The wallet allows you to create two types of  wallets:
<ul>
	<li><strong>A simple wallet</strong> - works like a normal account <span style="font-weight: 400">(additional features are being worked on; e.g. adding owners, setting a daily limit)</span></li>
	<li><strong>A multisig wallet</strong> - allows you to add any number of owner accounts and set a daily limit.
Every owner can send money from that account as long as it is under the daily limit. If above you need the signatures of the required other owners.</li>
</ul>
<em><span style="font-weight: 400">When operating on the main net make sure you write down / backup the wallet contract address! This address is required in case you need to reimport your wallet on a different computer or during backup/recovery.</span></em>
<h3>Multisig</h3>
If you want to send and amount which is over the daily limit, your other owners need to sign. This should mostly be done from another computer, though you could as well add accounts you have in the same node.

If a pending request comes in it will look as follows:

<a href="https://blog.ethereum.org/wp-content/uploads/2015/09/Screen-Shot-2015-09-16-at-11.56.37.png"><img class="wp-image-2117 aligncenter" src="https://blog.ethereum.org/wp-content/uploads/2015/09/Screen-Shot-2015-09-16-at-11.56.37.png" alt="Wallet - pending confirmation" width="584" height="407" /></a>

Simply click approve and the transaction goes through.
<h3>Deleting wallets</h3>
<a href="https://blog.ethereum.org/wp-content/uploads/2015/09/Screen-Shot-2015-09-16-at-12.00.51.png"><img class=" wp-image-2118 alignright" src="https://blog.ethereum.org/wp-content/uploads/2015/09/Screen-Shot-2015-09-16-at-12.00.51.png" alt="Wallet - Delete wallet" width="344" height="132" /></a><span style="font-weight: 400">

If you’d like to delete a wallet </span>click the trash icon on the wallet page, next to the wallet name. After you typed the name of the wallet it will be deleted from the Ðapp.

If you wrote the address down, you can always re-import the wallet in the "Add Wallet" section.
<h2>Roadmap</h2>
When everything works fine and we finished the binary integration we are planning to release a first official version in 1-2 weeks™

Until then please <a href="https://github.com/ethereum/meteor-dapp-wallet/issues" target="_blank">file issues</a> and discuss it on <a href="https://www.reddit.com/r/ethereum/comments/3l7c6c/first_developer_preview_of_the_ethereum_wallet/" target="_blank">reddit</a>!