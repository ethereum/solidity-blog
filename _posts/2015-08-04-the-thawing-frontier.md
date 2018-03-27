---
id: 1891
title: The Thawing Frontier
date: 2015-08-04T01:19:26+00:00
author: Stephan Tual
layout: post
guid: https://blog.ethereum.org/?p=1891
permalink: /2015/08/04/the-thawing-frontier/
dsq_thread_id:
  - "4000482018"
---
After a smooth launch, please find a few updates of note below. These probably will be of interest only to the more advanced users amongst you - if you’re not a miner, there is nothing to action.


# Epoch switch in 2 hours

Let’s start with a quick heads up to miners, the network will be switching epoch in roughly 2 hours. If you are using the geth/ethminer combo, you should be all set if you have opted to use the ```--autodag``` flag on geth and ```--no-precompute``` on ethminer. If not, keep an eye on your miners as they generate the <a href="https://github.com/ethereum/wiki/wiki/Ethash">dag</a>. Report all issues to either the <a href="https://github.com/ethereum/go-ethereum/issues">Go</a> or <a href="https://github.com/ethereum/cpp-ethereum/issues">C++</a> issue repositories, depending on which client you’re using. 

Note that some AMD cards will require GPU_MAX_ALLOC_PERCENT=95 to <a href="http://forum.ethereum.org/discussion/2737/some-cards-need-export-gpu-max-alloc-percent-95?new=1">support</a> the <a href="https://github.com/ethereum/ethash/blob/master/src/libethash/data_sizes.h#L51">larger DAG size</a>. 


# Client updates on the horizon. 

Within the next few days, we’ll see the 5k gas limit per block lifted. We’ll be operating with a 3M (3141592, or pi million as it's affectionately called) gas target per block going forward.  

Remember that it’s the protocol that defines the limits by how much the gas limit can increase (or decrease) per block, and that miners can only adjust their parameters within these limits. 

The max increase (or decrease) is set by the protocol at the parent gas limit /1024. With the default parameter unaffected by miners, it will take a minimum of 6 hours to climb from the current 5k gas per block to a limit allowing transactions (21,000 gas being required for a simple transaction on Ethereum). 

Of course, statistically speaking, the more miners update their clients when the update is released, the faster the network will reach the 3M target (the minimum being 28 hours). So if you are mining, keep an eye out for releases of both the <a href="https://github.com/ethereum/cpp-ethereum">C++</a> and <a href="https://github.com/ethereum/go-ethereum/releases">Go</a> clients and make sure to update as soon as they hit.


# Default Gas price is set at 50 shannon

The default gas price is set at 50 shannon (<a href="http://ether.fund/tool/converter">0.05 szabos</a>, or 0.00000005 ether). Remember that the gas price is ultimately always defined by the users calling a contract, while the miners can set a parameter on their machines to accept or ignore transactions that have a gas price within a certain range. 

On the user side of things, we also have a built-in oracle defining the optimal gas price, determined by the previous block gas usage. For example, if the previous block was less than 80% full, the gas price oracle will adjust to a lower gas price recommendation.


# Update on the Olympic Rewards

We are currently compiling the list of Olympic winners and will be processing them during the coming weeks. 

If you are expecting an Olympic reward and were not able to do the A->B transaction I covered in one of my <a href="https://blog.ethereum.org/2015/07/30/a-few-last-minute-notes/">previous blog posts</a>, do not  panic - you can still execute that transaction on the Olympic network. 

Some of you reported issues with catching up to the Olympic blockchain - in this instance, try the C++ client which synchronize in less than 5 hours. Note that you may have to mine a few blocks for your transaction to go through, as miners have now quite understandably moved on to Frontier.


# Update on namereg

Once thawing has ended, we'll be looking in adding the name registrar contract. We haven't finalized its behaviour just yet, and its behaviour (similar to an auction house) will probably deserve its own blog post. You can look at its current code published at <a href="https://github.com/ethereum/dapp-bin/blob/master/registrar/registrar.sol">https://github.com/ethereum/dapp-bin/blob/master/registrar/registrar.sol</a>.



# Announcements will always be made on our blog

We thank you for your amazing support and participation on social media, including <a href="https://twitter.com/ethereumproject">Twitter</a> and of course <a href="https://www.reddit.com/r/ethereum">reddit</a>. Note that because so many of you are tracking a few sites for information on Ethereum, we decided to release all announcements going forward - including change to the protocol or clients - on this blog (the one you are reading now). Finally, note that if you need answers to technical questions, you'll always be better served by our forums at <a href="http://forum.ethereum.org/">http://forum.ethereum.org/</a>.