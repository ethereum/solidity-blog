---
id: 3516
title: 'Security alert [11/24/2016]: Consensus bug in geth v1.4.19 and v1.5.2'
date: 2016-11-25T01:13:58+00:00
author: Vitalik Buterin
layout: post
guid: https://blog.ethereum.org/?p=3516
permalink: /2016/11/25/security-alert-11242016-consensus-bug-geth-v1-4-19-v1-5-2/
category: Security
---
<b>Security Alert</b><b>
</b><b>
</b><span style="font-weight: 400;">Affected configurations: Geth</span><span style="font-weight: 400;">
</span><span style="font-weight: 400;">
</span><span style="font-weight: 400;">Severity: High</span><span style="font-weight: 400;">
</span><span style="font-weight: 400;">
</span><span style="font-weight: 400;">Summary:  An issue has been identified with Geth's journaling mechanism. This caused a network fork at block #2686351 (Nov-24-2016 14:12:07 UTC). The new Geth release 1.5.3 fixes the journaling issue and repairs the fork.</span>

<span style="font-weight: 400;">Details: Geth was failing to revert empty account deletions when the transaction causing the deletions of empty accounts ended with an an out-of-gas exception. An additional issue was found in Parity, where the Parity client incorrectly failed to revert empty account deletions in a more limited set of contexts involving out-of-gas calls to precompiled contracts; the new Geth behavior matches Parity's, and empty accounts will cease to be a source of concern in general in about one week once the state clearing process finishes.</span>

<span style="font-weight: 400;">The chain that was created from block #2686351 by the old Geth client, which both Parity and the new Geth release consider invalid, seems to have been mostly abandoned around block #2686516, meaning that ~165 blocks were mined on the now abandoned chain. Transactions are broadcast across the network so most transactions are likely present on both the old Geth chain and the current chain, although mining rewards and transaction fees on the old Geth chain are lost. </span><b>No transactions or blocks on the chain that both clients will now accept will be reverted.</b>

<span style="font-weight: 400;">The latest geth release will update the blockchain from the point of the fork, even if it has synced past the point of the fork.</span>

<span style="font-weight: 400;">Solution: Geth 1.5.3 was released. </span>

<span style="font-weight: 400;">If you are using Geth: Download the latest client here: </span><a href="https://github.com/ethereum/go-ethereum/releases/tag/v1.5.3"><span style="font-weight: 400;">https://github.com/ethereum/go-ethereum/releases/tag/v1.5.3</span></a>

<span style="font-weight: 400;">If you are using Mist: Restart Mist and the auto-update feature will prompt you to update the Geth client that Mist uses to geth 1.5.3.</span>

<span style="font-weight: 400;">If you do not update, please be aware you will be on an invalid chain that is not supported.</span>

<span style="font-weight: 400;">We continue to recommend that exchanges and other high-value users run multiple clients and automatically halt operations or otherwise enter safe mode if they go out of sync by more than ~10 blocks.</span>

<span style="font-weight: 400;">Ethereum websites and mobile applications that allow you to store ether and/or make transactions are run by third party web based or mobile Ethereum providers ("Third Party Providers"). Third Party Providers run their own Ethereum client infrastructure to facilitate their services. Generally, you do not need to do anything if you use a Third Party Provider such as MetaMask, Jaxx, and MyEtherWallet. However, they may have instructions for you. You should check with your Ethereum Third Party Provider to see what actions, if any, they are recommending for their users.</span>

<span style="font-weight: 400;">-----------------------------</span>

<b>DISCLAIMER</b><span style="font-weight: 400;">
</span><span style="font-weight: 400;">This is an emergent and evolving highly technical space. If you choose to participate, you should know there are many risks involved including but not limited to risks like unexpected bugs and other technical complications that could result in loss of ether and other consequences. In addition, if you do not update to Geth 1.5.3, you will be on an unsupported network. By choosing to use the Ethereum platform, you assume the risks of this emergent platform.</span>