---
layout: post
published: true
title: 'Sourcify: Towards Safer Contract Interaction for Humans'
date: '2020-06-02'
author: Edi Sinovčić, Franziska Heintel
category: Announcements
---

**tl;dr**: Building sensible blockchain applications for humans is **hard**. You can enhance the user experience of 
your dapp today by leveraging the power of open source. Increase awareness and give more transparency on what users 
are *actually* doing when interacting with your code on the blockchain, i.e. when signing a transaction, by publishing 
the source code to this decentralized repository and using metadata files, which translate “random” hex strings into 
human-readable language. Sourcify is a tool to help you do exactly that. If you want to upload your source code, either visit [verification.komputing.org](https://verification.komputing.org/), the [Sourcify Github repo](https://github.com/ethereum/sourcify) or use the Sourcify 
plugin in Remix. And, to learn how Sourcify works and why it matters, simply keep on reading.

You might already know Sourcify under the name “Source-Verify”/ ”Source-Verifier” - don’t despair, we just decided to give 
it a nicer name!

## YOLO Signing - One of Today’s Everyday Problems in Ethereum UX 🙈📝🧨

Think about the following: How many developers of the dapps you are using do you know personally?

Let’s imagine you want to interact with a dapp using MetaMask and it shows you this dialogue: 

![Yolo Signing](/img/2020/05/MetaMask_Yolosign.png)

*You Only Live Once (YOLO)  Signing - confirm at your peril*

What does all that hex data mean? You basically have no idea what you are agreeing to  - but you only live once, right? 
Do you reject or confirm? If you go ahead and sign, you are **YOLO signing**. YOLO signing is bad because even if you only 
live once, you can probably regret some stuff forever! ;) 

By confirming, you blindly trust the developers of this dapp that it will do what they promised. And while it’s great to 
have faith in humanity, it’s even greater to have the option to verify what is happening and take an informed decision.

## Source Verification & NatSpec to the Rescue

To prevent your users from having to blindly trust you and YOLO sign, you, as a dapp developer, can verify your code.

By verifying and uploading your code, you allow any other developer to recompile the source code and check it for themselves.

Properly “verifying” your code involves two steps:
1. Commenting the code using [NatSpec](https://solidity.readthedocs.io/en/latest/natspec-format.html). NatSpec is the 
Ethereum Natural Language Specification Format which describes functions, return values etc. in a friendly way.
1. Publishing the dapp’s source code and its metadata to a decentralized network.

## Meet Sourcify, a decentralized automated contract verifying service 📝🔍✔️

Sourcify tries to make the process of source verification (step 2 in the list above) easy and transparent. There are different ways 
how you can make use of the tool. 

You can either...
* Use the interface hosted on [verification.komputing.org](https://verification.komputing.org/)
* Use [Remix](https://remix.ethereum.org/) plugins (we’ll show you a play by play in one of the following posts!)
* Publish each source code file and the metadata file to [IPFS](https://docs.ipfs.io/concepts/what-is-ipfs/) manually 
(Sourcify will then pick it up automatically around the time of deployment)
* [Run](https://github.com/ethereum/sourcify) Sourcify yourself

## How does Sourcify work under the hood? 📖

Sourcify is a project on the Ethereum blockchain that:
* Listens for the deployment of smart contracts.
* Checks to see if their metadata and source code is published on IPFS or Swarm.
* Recompiles the code.
* Checks that it is the same as the deployed code.
* Permanently stores the source code, ABI and other metadata on IPFS.

![Sourcify Flowchart](/img/2020/05/Sourcify_Flowchart.png)

*Sourcify Verification Flow*

Sourcify has a monitoring service that listens to new blocks which are mined on an Ethereum blockchain. Currently it is 
monitoring the Ethereum mainnet and the Rinkeby and Goerli testnets.

It takes the transactions in the blocks, derives their bytecode metadata and tries to find links in the data that go to 
either IPFS or Swarm for the source contracts and metadata files.

Subsequently, it tries to fetch the files. If that is successful, the retrieved files are sent to the verifier which 
attempts to recompile the code and checks that it is the same as the deployed code. If everything went well and recompiled 
metadata and code are a match, all of it gets indexed, pinned and stored (in a public decentralized system, currently IPFS).

And if you’d like to verify the verifier, no problem: Simply take the files from IPFS and recompile them yourself - 
the necessary compiler options are also stored in the system.

## Verify all the Sources - Are we there yet? ⛅

**The bad news:** The road towards a more secure, more aware, educated and trustless UX for Ethereum dapps is certainly 
still long. 

**The good news:** There are many things we can do *right now* to contribute towards it!

What dapp developers can do:
* Use NatSpec to comment your code
* Upload your source code using Sourcify

What the broader Ethereum community can do:
* Expand NatSpec
* Implement NatSpec into wallets
* Give wallets confidence in the source of the NatSpec (e.g. via Sourcify)
* Implement wallet support for Sourcify
* Support dapp devs to use NatSpec, metadata, Sourcify by extending the developer tooling around it (e.g. help with 
auto-publishing tools)

With [Version 0.6.8](https://github.com/ethereum/solidity/releases/tag/v0.6.8), Solidity added a recommendation to use 
SPDX license identifiers for all source files, which is also stored in the contract metadata. This change helps tools like 
Sourcify as well as deployment tools by enabling them to check for open source licenses before uploading the code.

We hope you enjoyed this first high level introduction into why source verification matters and how it could work. Feel 
free to reach out to us with any question you might have and stay tuned for more content to follow soon. 

---
Follow Sourcify on [Twitter](https://twitter.com/SourcifyEth). 

Chat with us on [Gitter](https://gitter.im/ethereum/source-verify). 

Check out the Sourcify code on [Github](https://github.com/ethereum/sourcify). 

Browse the verified contracts [here](https://contractrepo.komputing.org/). 
