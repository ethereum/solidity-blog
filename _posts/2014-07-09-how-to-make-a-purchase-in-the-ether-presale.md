---
id: 206
title: Background on the mechanics of the ether pre-sale
date: 2014-07-09T08:16:56+00:00
author: Taylor Gerring
layout: post
guid: https://blog.ethereum.org/?p=206
permalink: /2014/07/09/how-to-make-a-purchase-in-the-ether-presale/
dsq_thread_id:
  - "2831185857"
---
<p class="p1">During the run-up to the ether sale we will be releasing a series of blog posts regarding our development plans and intended organizational structure. Today we describe some of the background details of the ether sale module.  The sale module will be displayed on our website at <a href="http://www.ethereum.org/"><span class="s1">https://www.ethereum.org</span></a> for the duration of the sale.</p>
<p class="p1"></p>
<p class="p1"><b>What is it?</b></p>
<p class="p4">The ether sale is an HTML/JavaScript web application that enables people and organizations to pre-purchase ether tokens with bitcoin. Ether may be purchased by parties interested in using it in their development of distributed applications on the Ethereum platform or simply to pay for the use of applications on the platform once the genesis block is released.  The application runs mostly inside the browser, including pseudo random number generation (PRNG) and encryption/decryption of user secrets.</p>
<p class="p4">Additionally, we will be making available a standalone pure Python application that can be used from a command line interface to purchase ether in lieu of using the website store.</p>
<p class="p4"></p>
<p class="p3"><b>Website workflow:</b></p>

<ol class="ol1">
	<li class="li5">After agreeing to the store’s Terms and Conditions, the buyer enters the desired amount of ether to purchase, their email address, a password to use for encryption, and generates a “random” seed forming the basis for their transaction-specific bitcoin &amp; Ethereum addresses.</li>
	<li class="li5">The <span class="s2">user</span> buyer sends payment in bitcoin to the generated address which is displayed on the store page and also provided in a downloadable encrypted wallet file. When the browser, which is monitoring the bitcoin blockchain, sees enough funds have been sent to this generated address, the browser creates a new transaction forwarding the bitcoin to the genesis sale address. This transaction is signed by the buyer’s browser and submitted to the bitcoin network. No transaction data is stored on <span class="s2">the</span> any servers because the Bitcoin blockchain serves as the purchase database.</li>
	<li class="li5">After the transaction has been successfully submitted, the <span class="s2">user</span> buyer is again prompted to download a backup copy of their encrypted wallet in addition to being emailed another backup file.  The buyer should be aware that keeping the wallet as an email attachment in an email account for any period of time represents a vulnerability.  The email attachment should be downloaded to a safe computer at the earliest opportunity and the email should be expunged from the email system.</li>
</ol>
<p class="p4"></p>
<p class="p4"><em>(What follows is technical information, the GUI we have developed abstracts all of this is in a very user-friendly manner)</em></p>
<p class="p4"><em><strong>
</strong></em><strong>Wallet encryption process: </strong></p>

<ol class="ol1">
	<li class="li5">Randomgen a SEED.</li>
	<li class="li5">BKPKEY = sha3(SEED + “\x02″)[:16]</li>
	<li class="li5">KEY = pbkdf2(PW)</li>
	<li class="li5">ENCSEED = encrypt(KEY, SEED)</li>
	<li class="li5">WALLET = [ ENCSEED, BKPKEY ] (plus non-sensitive data like ethereum and bitcoin address)</li>
</ol>
<p class="p4">The BTC and ETH privkeys and addresses are derived from the SEED.</p>
<p class="p4"></p>
<p class="p3"><b>Seed recovery process: </b></p>

<ol>
	<li>WALLET + PW recovery:  (normal)</li>
	<li>KEY = pbkdf2(PW)</li>
	<li>SEED = decrypt(KEY, ENCSEED)</li>
</ol>
<p class="p4"></p>
<p class="p4"><strong>Overview of website steps for making an ether purchase:</strong></p>

<ol class="ol1">
	<li class="li5">Buyer clicks “Buy Ether” and agrees to the Terms &amp; Conditions</li>
	<li class="li5">Buyer enters initial purchase information
<ol class="ol1">
	<li class="li7">Buyer enters the amount of ether they’d like to purchase</li>
	<li class="li7">Buyer enters email address as the destination for wallet backup file</li>
	<li class="li7">Buyer enters passphrase, which serves as an encryption seed. A very strong passphrase is highly recommended</li>
</ol>
</li>
	<li class="li5">Buyer generates entropy by moving their mouse or tapping their screen. A “seed” is constructed based on this generated entropy as well as other random system inputs. When the desired entropy length is achieved the wallet is generated using the seed.  The wallet contains:
<ol class="ol1">
	<li class="li7">an individual BTC address to send the funds to</li>
	<li class="li7">an individual ETH address</li>
	<li class="li7">an encrypted seed</li>
	<li class="li7">a backup encrypted seed</li>
</ol>
</li>
	<li class="li5">The browser sale app checks the newly created BTC address to see if funds arrive. Before sending any funds, the buyer has an opportunity to download the wallet. If a problem occurs between transactions, any funds send to the buyer’s personal deposit address will be accessible with the passphrase. If the unspent balance of the BTC address &gt; 0.01 BTC, the browser sale app generates a signed transaction from the newly created BTC address with 2 specific outputs:
<ol class="ol1">
	<li class="li7">an output of the total unspent balance minus miners fee to the main sale BTC address–this btc address is where all the funds go, it’s a fixed, known BTC address.</li>
	<li class="li7">an output of 10000 satoshi to a BTC address generated from the (string) ETH address–this bitcoin address is for confirming ethereum addresses and is unique for every transaction.</li>
</ol>
</li>
	<li class="li5">The browser sale app prompts the purchaser to download their wallet again <span class="s2">&amp;</span> and also sends the buyer an email containing a backup for the ether wallet.  The same caution as stated above applies.  Do not leave the wallet as an email attachment in an email system.  Download as soon as possible and ensure that the email is expunged.</li>
	<li class="li5">The browser sale app will display the number of bitcoin transaction confirmations</li>
</ol>
<p class="p4"></p>
<p class="p4">So that’s it! Although there’s a lot of technical things happening in the background, the GUI we have developed will make the process a click by click operation.</p>