---
id: 2818
title: 'Taylor&#8217;s Summer Update'
date: 2016-07-11T16:05:58+00:00
author: Taylor Gerring
layout: post
guid: https://blog.ethereum.org/?p=2818
permalink: /2016/07/11/taylors-summer-update/
dsq_thread_id:
  - "4977048869"
---
Hey everyone, I spent some time with our Canadian friends in Toronto after presenting "Ethereum: The World Computer" at Blockchain Training Conference last month and I wanted to provide a quick update on some of the exciting happenings in the Ethereum dev ecosystem. Lots of things are brewing behinds the scenes, so let's jump in!

## Projects

### Mist

Ethereum wallet has been refined significantly over the last several months expanding support to arbitrary contract interaction via the "custom contracts" tab. This is a massive improvement over sending transactions on the command line, as was often required in Frontier. With several new team members doing work with the Ethereum Foundation, new versions of Mist are now rolling out and will incorporate improvements to Geth and Eth as they are integrated. If you're using an old version, update to <a href="https://github.com/ethereum/mist/releases/tag/0.8.0">Mist 0.8.0</a> for the latest goodies.

[caption id="attachment_2884" align="aligncenter" width="2442"]<img src="https://blog.ethereum.org/wp-content/uploads/2016/07/23353356-446f-11e6-8fda-7f5546645449.png" alt="Screenshot Mist 0.8.0" width="2442" height="1714" class="size-full wp-image-2884" /> Screenshot Mist 0.8.0[/caption]

### Remix

After all the hard work spent reorganizing the C++ codebase, the CPP team has shifted gears from Mix to Remix, as the IDE now targets the web. Remix has hit it first alpha, and published with a demo online. Check out its repository at [https://github.com/ethereum/remix](https://github.com/ethereum/remix) for more info. In general, EVM IDEs aren't yet popular, so this could be a major step forward in approachability, like the [online Solidity compiler](https://ethereum.github.io/browser-solidity/) has been. New security tools, such as <a href="https://github.com/arachnid/evmdis#example">EVMDIS</a> as expected to be released to the community. Work is also being done so that formal verification can detect recursive call patterns. More details can be found in Christian's <a href="https://blog.ethereum.org/2016/07/08/c-dev-update-summer-edition/">C++ DEV Update</a>.

### ENS

[caption id="attachment_2824" align="aligncenter" width="1282"]<img src="https://blog.ethereum.org/wp-content/uploads/2016/07/Screen-Shot-2016-07-06-at-17.30.40.png" alt="ENS example diagram" width="1282" height="716" class="size-full wp-image-2824" /> Diagram of example ENS hierarchy with multiple records[/caption]

Not an official project, but closely related to many projects, Nick Johnson has started work on the Ethereum Name Service. The encompasses smart contract-based resolvers with standard API. The specification supports delegation by name component (separated by “.”) and multiple types of records. It’s possible to build gateways to plug-in to existing systems, like DNS. Currently, a draft specification with [a reference implementation is available in solidity](https://github.com/arachnid/ens) and discussion is ongoing on [Gitter](https://gitter.im/ethereum/go-ethereum/name-registry). This could have large implications for general usability of many services on Ethereum network from wallet aliases to friendly Swarm node names.

## Protocols

### Light client

Something to watch closely is light-client functionality entering [public testing phase](https://github.com/zsfelfoldi/go-ethereum/wiki/Light-Client-Public-Test). Zsolt has been working on this code for months and the team looks poised to merge it into the main repo soon. The new code decreases chain sync time to under 30 mins from genesis block and under a minute when used week-to-week. This builds on top of work that Peter did for the fast sync functionality that is now the default in Mist (`--fast`). More work is being done to improve log/receipt retrieval, and several iterations to the protocol is expected over time, so this is not the last you'll hear about LES!

[caption id="attachment_2828" align="aligncenter" width="988"]<a href="https://twitter.com/vgrade/status/748257427481583616/photo/1"><img src="https://blog.ethereum.org/wp-content/uploads/2016/07/Screen-Shot-2016-07-06-at-18.18.05.png" alt="Light client running on embedded device" width="988" height="1026" class="size-full wp-image-2828" /></a> Tweet of light client running on embedded device[/caption]

### Swarm

Swarm is also being vigorously tested as PoC2 demonstrating economically self sustaining storage and distribution layers. Viktor, Dani, and Aaron have been working hard to ensure the code is as bug-free as possible before launching it on testnet. Nick has developed a simulation script for network and communication verification which will go a long way towards this effort. One of the exciting features of their work is the accounting &amp; incentive systems allowing for long-term storage and retrieval of data. If you want to know more, the Swarm team has been featured in several [YouTube videos](https://www.youtube.com/watch?v=BAAAhZI7qRQ) which explains how everything works.

[caption id="attachment_2826" align="aligncenter" width="1276"]<img src="https://blog.ethereum.org/wp-content/uploads/2016/07/Screen-Shot-2016-07-06-at-18.14.16.png" alt="Data storage &amp; retrieval on Swarm" width="1276" height="718" class="size-full wp-image-2826" /> Data storage & retrieval on Swarm[/caption]

Though called many other names, this enables a vision for decentralized storage where you own your own data. Currently, while there are a few blockchain-based apps, full availability of Swarm will allow for those blockchain-backed apps to become full DApps and run 100% decentralized inside Mist.

### IPFS &amp; Raiden

On a collaborative note, continued discussion between the IPFS and Ethereum teams may allow them to share a protocol in the future, allowing both ecosystems to benefit from their respective network effects. Further, work going towards Raiden and Swarm turned out to be complementary as solutions were working towards solving similar problems. I’m told that the Raiden Network can be used as a payment channel for the Swarm incentive system, which would help address some privacy and scalability concerns while also multiplying their own efforts. Knowing that they are reinforcing makes me even more excited for what's in store for the future!

### Wrap-up

Devcon2 is right around the corner, with sessions scheduled for September 19, 20, 21 during the Global Blockchain Summit in Shanghai, China. Ethereum Foundation's information website is now online at [ethereumfoundation.org/devcon](https://ethereumfoundation.org/devcon/) and more information about the Summit can be found at [blockchainweek2016.org](http://www.blockchainweek2016.org/). Despite having a larger venue, space will still be limited, so register now!

I hope 2016 will prove to be a pivotal year for blockchains. UI/UX is improving rapidly over what was available just a couple years ago and with solutions like Raiden for state/payment-channels, some of the privacy and scalability problems of blockchains are eased. Combined with research advancements targeted for Serenity, we are on pace to overcome many of the obstacles Vitalik once listed as <a href="https://github.com/ethereum/wiki/wiki/Problems">hard problems</a>.

[caption id="attachment_2837" align="aligncenter" width="800"]<img src="https://blog.ethereum.org/wp-content/uploads/2016/07/ethereum-keep-calm-and-code-on.png" alt="Keep Calm and Code On" width="800" height="1200" class="size-full wp-image-2837" /> Keep Calm and Code On[/caption]
&nbsp;

<div id="disqus_link_container"></div>
<script>jQuery(document).ready(function() { EthBlogUtils.display_disqus_link();});</script>