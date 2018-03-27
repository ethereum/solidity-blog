---
id: 627
title: Ether Purchase Troubleshooting
date: 2014-07-23T04:05:24+00:00
author: Vitalik Buterin
layout: post
guid: https://blog.ethereum.org/?p=627
permalink: /2014/07/23/ether-purchase-troubleshooting/
dsq_thread_id:
  - "2865874357"
---
Although we hope that the ether purchasing experience goes smoothly for everyone, we recognize that there will always be situations where things do not quite go as planned. Perhaps your internet connection dies in the middle of your purchase. Perhaps you accidentally click a back button or some link or simply hit refresh while the purchase is in progress. Perhaps you forgot to download your wallet. Perhaps you think you may have forgotten your password, and you want to make sure you have it down correctly. For all of these situations, the user experience is unfortunately going to be a bit more tricky than simply downloading a web app; a bit of command line action with a <a href="https://github.com/ethereum/pyethsaletool">Python script</a> will be required.

First of all, let's go over downloading the Python script. To get the script installed, download the <a href="https://github.com/ethereum/pyethsaletool/archive/master.zip">zip archive</a> from here, and unpack it. Then, navigate to the directory, and you should see a number of files, including pyethsaletool.py. At this point, open up a command line in this directory. Run <code>python pyethsaletool.py</code>, and you should see a list of help instructions.

Now, let's go over the most common potential issues one by one.

<b>1) I forgot to download my wallet before closing the browser tab.</b>

You should receive a backup of your wallet in your email. If you entered a fake email address, and at the same time forgot to download your wallet, then unfortunately you have no recourse.

<b>2) I want to make sure that my ether was actually purchased.</b>

Run <code>python pyethsaletool.py list -w /path/to/your/wallet.json</code>, substituting the path with the path where you downloaded your wallet to. You should see a record of your purchase. If not, then run <code>python pyethsaletool.py getbtcaddress -w /path/to/your/wallet.json</code> and look up the address on <a href="http://blockchain.info">blockchain.info</a>. If there is a nonzero balance, you are in situation #4.

<b>3) I want to make sure that I remember my password.</b>

Run <code>python pyethsaletool.py getbtcprivkey -w /path/to/your/wallet.json</code>, substituting the path. When it prompts you for the password enter it, and see whether you get an error. If you get an error to do with PKCS7 padding, you entered the wrong password; if you get a BTC private key out (ie. a sequence of 51 characters starting with a 5), then you're fine.

<b>4) I sent my BTC into the intermediate address, but it never made it to the exodus.</b>

Run <code>python pyethsaletool.py getbtcprivkey -w /path/to/your/wallet.json</code>, substituting the path appropriately. Then, import this private key into the <a href="http://blockchain.info/wallet">blockchain.info wallet</a> or <a href="http://kryptokit.com">Kryptokit</a>. Alternatively, you may also run <code>python pyethsaletool.py finalize -w /path/to/your/wallet.json</code> to finish the purchasing process through Python.

<b>5) I want to make sure I will be able to access my ether later.</b>

Run <code>python pyethsaletool.py getethprivkey -w /path/to/your/wallet.json</code>, substituting the path. Then, download <a href="http:github.com/ethereum/pyethereum">pyethereum</a>, install it, and use <code>pyethtool privtoaddr c85ef7d79691fe79573b1a7064c19c1a9819ebdbd1faaab1a8ec92344438aaf4</code>, substituting in the Ethereum privkey that you got from the first step. If the address that you get matches the address that you saw when you were purchasing ether, then you know that you have your Ethereum private key.

<b>6) I sent more BTC into the intermediate address after the web app finalized.</b>

This situation is identical to #4. You can recover the BTC or finalize it at your leisure.

If you have any other issues, please ask them in the comments and they will be added to this article.