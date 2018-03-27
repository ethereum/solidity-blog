---
id: 618
title: Launching the Ether Sale
date: 2014-07-22T22:00:44+00:00
author: Vitalik Buterin
layout: post
guid: https://blog.ethereum.org/?p=618
permalink: /2014/07/22/launching-the-ether-sale/
dsq_thread_id:
  - "2865299802"
---
First of all, I would like to thank the community, and especially those close to the project who have in many cases abandoned their jobs to dedicate their time to it, for their extreme patience regarding the launch of the ether sale. We have been promising that the sale would arise in two weeks for six months, and many team members have endured substantial hardships because of expectations that we set regarding when we would be able to provide funding. We certainly miscalculated the sheer difficulty of navigating the relevant legal processes in the United States and Switzerland, as well as the surprisingly intricate technical issues surrounding setting up a secure sale website and cold wallet system. However, the long wait is finally over, and we are excited to announce the launch of the sale.

In the intervening six months, our time has been spent on the following:
<ul>
	<li>Developing the Ethereum clients, to the point where we now have four full implementations of the protocol, of which two are fully compatible and the other two nearly so. Originally, the intent was for only three implementations; however, a member of the community (Roman Mandeleil) has independently come forward with a functional Java client. PoC5 has been released today.</li>
	<li>Developing the Ethereum programming languages and compilers, to the point where we have moderately stable versions of Serpent, LLL and Mutan.</li>
	<li>Developing the Ethereum protocol. Major changes between February and June include:
<ul>
	<li>The transactions-pay-for-computation system, where a transaction specifies the amount of "gas" that it is allowed to consume and pre-pays a fee for it. Originally, contracts paid for computation, requiring every contract to have a security "wrapper" mandating a minimum fee to avoid balance-starvation attacks.</li>
	<li>The concept of a recursive virtual machine, where contracts can "call" other contracts like functions and receive "return values" in response. This allows for a high level of generalization and code reuse in the blockchain, cutting down bloat.</li>
	<li>A 10 to 20-second block time, down from 60s. This will be added in PoC6.</li>
	<li>The concept of "micro-chains", where the top-level block header will only contain PoW-relevant information, reducing the minimum storage and bandwidth requirements for a light node by 4x (although the fast block time will once again increase these requirements by 4x). This will be added in PoC6.</li>
	<li>The placement of transactions into a Merkle tree, allowing for the development of a light client protocol for verification of individual transaction execution.</li>
	<li>The concept of memory as a byte array, making it easier to port existing compilers to compiling Ethereum scripts.</li>
	<li>Mid-stage developments in the mining algorithm thanks primarily to the work of Vlad Zamfir. More details should be released soon.</li>
</ul>
</li>
	<li>Developing an increasingly large repertoire of testing facilities in order to help ensure cross-client compatibility.</li>
	<li>Setting up our legal organizational structure in Switzerland.</li>
	<li>Legal due-diligence processes in the United States.</li>
</ul>
&nbsp;

Ether will be purchasable directly through our website at <a href="https://ethereum.org">https://ethereum.org</a>. There are a number of documents providing more details on the sale as well, including a development plan (<a href="https://www.ethereum.org/pdfs/ĐΞVPLAN.pdf">ĐΞV PLAN</a>), roadmap, <a href="https://www.ethereum.org/pdfs/IntendedUseOfRevenue.pdf">Intended Use of Revenue document</a>, <a href="https://www.ethereum.org/pdfs/TermsAndConditionsOfTheEthereumGenesisSale.pdf">Terms and Conditions of the Ethereum Genesis Sale</a>, <a href="https://www.ethereum.org/pdfs/EtherProductPurchaseAgreement.pdf">Ether Product Purchase Agreement, </a>and updated <a href="https://www.ethereum.org/pdfs/EthereumWhitePaper.pdf">White Paper</a> and <a href="https://www.ethereum.org/pdfs/EthereumYellowPaper.pdf">Yellow Paper</a>. Particularly note the following:
<ul>
	<li><b>Ether will NOT be usable or transferable</b> until the launch of the genesis block. That is to say, when you buy ether and download your wallet, you will not be able to do anything with it until the genesis block launches.</li>
	<li>The price of ether is initially set to a discounted price of <b>2000 ETH per BTC</b>, and will stay this way for <b>14 days</b> before linearly declining to a final rate of <b>1337 ETH per BTC</b>. The sale will last <b>42 days</b>, concluding at 23:59 Zug time September 2.</li>
	<li>The expected launch time for the genesis block is <b>winter 2014-2015</b>. This date has been continually pushed back in large part due to funding delays, but development should soon start to proceed much more quickly.</li>
	<li>Ether is a product, <b>NOT a security or investment offering</b>. Ether is simply a token useful for paying transaction fees or building or purchasing decentralized application services on the Ethereum platform; it does not give you voting rights over anything, and we make no guarantees of its future value.</li>
	<li>We are not blocking the US after all. Yay.</li>
	<li>It is very likely that alternative versions of Ethereum (“alt-etherea” or “forks”) will be released as separate blockchains, and these chains will compete against the official Ethereum for value. Successfully purchasing ETH from the Genesis Sale only guarantees that you will receive ETH on the official network, not any forks.</li>
	<li>The Terms and Conditions of the Ethereum Genesis Sale, and the Ether Product Purchase Agreement, are the only authoritative documents regarding the sale. Any statements that we have made up to this point are null and void.  The ethereum.org website, the blog posts since Taylor Gerring's recent howto and other posts we have made on various forums including our forum and reddit are to be treated as informal communications designed to keep the community updated on what is happening with Ethereum.</li>
	<li>The annual issuance rate is <b>0.26x</b> the initial quantity of ether sold, reduced from the 0.50x that had been proposed in January. We may choose later on to adopt alternative consensus strategies, such as hybrid proof of stake, so future patches may reduce the issuance rate lower. However, we make absolutely no promises of this, except that the issuance rate will not exceed 26.00% per annum of the quantity of ether sold in the Genesis Sale.</li>
	<li>There are <b>two endowment pools, each 0.099x</b> the initial quantity of ether sold, that will be allocated in the first case to early contributors to the project and in the second case to a long-term endowment to our non-profit foundation (see sale docs for more info). This is reduced from our proposal in April of 0.075x to early contributors and 0.225x to a foundation.</li>
	<li>We reserve the right to use up to 5000 BTC from the pre-sale while the pre-sale is running in order to speed up development. We may or may not take this option depending on community response; if done at all it will be done transparently.</li>
	<li>If you are eligible for backpay or a share of the early contributor pool, you will be contacted soon.</li>
        <li><b>Do NOT refresh the page</b> while using the ether sale website.</li>
	<li>If you purchase ether, <b>DO NOT lose your wallet file or your password or you will never be able to access your ether. </b>Additionally, make sure you <b>download your wallet</b> in the first place.</li>
</ul>
The exodus address is <a href="https://blockchain.info/address/36PrZ1KHYMpqSyAQXSG8VwbUiq2EogxLo2">36PrZ1KHYMpqSyAQXSG8VwbUiq2EogxLo2</a>; however, <b>DO NOT send BTC to this address directly</b>, since transactions that are not properly formatted will be invalid and treated as gifts to Ethereum Switzerland GmbH. Use the sale application on the website. Unlike some other projects, however, paying with BTC using our sale application from a Coinbase account or an exchange account poses no problems. The BTC address that you send from does not matter.

For advanced users and those who are uncomfortable with purchasing through a website, a Python library for making purchases is also available at <a href="https://github.com/ethereum/pyethsaletool">https://github.com/ethereum/pyethsaletool</a>.

The address is a 3-of-4 cold storage, multisig setup, between four centers which are themselves protected via mechanisms such as m-of-n Shamir's Secret Sharing. Check <a href="https://blockchain.info/tx/592eaf32d83a7db47321642c4b3837d38edfbe57910f4f6abd548e578f51f80a">this transaction on blockchain.info</a>, input 4, to see proof that we are capable of spending funds from the address. We will be releasing a more detailed description of the cold storage policy soon, although strategically important variables (eg. precise locations of the sites, passwords, combinations to safes) will unfortunately be kept secret.

More information will be released throughout the sale, and we will soon have an AMA on Reddit. We look forward to continuing this exciting journey together!