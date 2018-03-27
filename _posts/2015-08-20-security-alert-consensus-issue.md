---
id: 1963
title: 'Security alert [consensus issue]'
date: 2015-08-20T17:26:23+00:00
author: Jutta Steiner
layout: post
guid: https://blog.ethereum.org/?p=1963
permalink: /2015/08/20/security-alert-consensus-issue/
dsq_thread_id:
  - "4050696140"
---
This alert is related to a consensus issue that occurred on the Frontier network at block 116,522, mined on 2015-08-20 at 14:59:16+02:00 – <b>Issue has been fixed, see "Fix" below.</b>

<strong>Impact: </strong>High

<b>Issue description:</b><span style="font-weight: 400"> <span style="font-weight: 400">State database consensus issue in geth with deletion of account data, which could happen during SUICIDE instructions.</span></span>

<b>Affected implementations: <span style="font-weight: 400">All geth implementations up to and including versions 1.0.1, 1.1.0 and </span><i><span style="font-weight: 400">develop </span></i><span style="font-weight: 400">(</span><i><span style="font-weight: 400">“unstable 1.1.0”</span></i><span style="font-weight: 400">) were affected.</span><span style="font-weight: 400"> </span><i><span style="font-weight: 400">Eth (C++) and pyethereum (Python) are unaffected.</span></i></b>

<b>Effects on expected chain reorganisation depth: </b><span style="font-weight: 400">Increase waiting time for eventual block confirmation to 12 hours</span>

<b>Proposed temporary workaround:</b><span style="font-weight: 400"> <span style="font-weight: 400">Miners switch to eth or pyethereum asap</span></span>

<b>Remedial action taken by Ethereum</b><span style="font-weight: 400">: <span style="font-weight: 400">Provision of fixes as below. </span></span>

<b>Fix: </b>Note that the consensus  issue occured just before the announcement of the new release 1.1.0.<b> </b><span style="font-weight: 400">When upgrading, please make sure you upgrade to the version you intended as you might not want to fix </span><i><span style="font-weight: 400">and</span></i><span style="font-weight: 400"> upgrade from 1.0.1 to 1.1.0 (which has not yet been officially released) at the same time. Fixes below are for version 1.0.2 – builds are generated for v 1.0.2. </span>
<ul>
	<li>Release 1.0.2 including source and binaries can be found <a href="https://github.com/ethereum/go-ethereum/releases/tag/v1.0.2">here</a></li>
	<li>If you are building from source: git pull followed by make geth – please use the master branch commit <code>a0303ff4bdc17fba10baea4ce9ff250e5923efa2</code></li>
	<li><span style="font-weight: 400">If using the PPA: </span><span style="font-weight: 400"><code>sudo apt-get update</code></span><span style="font-weight: 400"> then </span><span style="font-weight: 400"><code>sudo apt-get upgrade</code></span></li>
	<li>We are still working on the <code>brew</code> fix</li>
</ul>
<span style="font-weight: 400">The correct version for this update on Ubuntu AND OSX is Geth/v1.0.2-</span><span style="font-weight: 400">a0303f</span>

Note that it is likely that you receive the following message alert which will resolve itself after a while once your peers have updated their clients:

<span style="font-weight: 400">I0820 </span><span style="font-weight: 400">19</span><span style="font-weight: 400">:</span><span style="font-weight: 400">00</span><span style="font-weight: 400">:</span><span style="font-weight: 400">53.368852</span><span style="font-weight: 400">    </span><span style="font-weight: 400">4539</span><span style="font-weight: 400"> chain_manager</span><span style="font-weight: 400">.</span><span style="font-weight: 400">go</span><span style="font-weight: 400">:</span><span style="font-weight: 400">776</span><span style="font-weight: 400">]</span> <span style="font-weight: 400">Bad</span><span style="font-weight: 400"> block</span><a href="https://forum.ethereum.org/search?Search=%23116522&amp;Mode=like"> <span style="font-weight: 400">#116522</span></a><span style="font-weight: 400"> (05bef30ef572270f654746da22639a7a0c97dd97a7050b9e252391996aaeb689)</span>

<span style="font-weight: 400"><span style="font-weight: 400">I0820 </span><span style="font-weight: 400">19</span><span style="font-weight: 400">:</span><span style="font-weight: 400">00</span><span style="font-weight: 400">:</span><span style="font-weight: 400">53.368891</span><span style="font-weight: 400">    </span><span style="font-weight: 400">4539</span><span style="font-weight: 400"> chain_manager</span><span style="font-weight: 400">.</span><span style="font-weight: 400">go</span><span style="font-weight: 400">:</span><span style="font-weight: 400">777</span><span style="font-weight: 400">]</span> <span style="font-weight: 400">Found</span><span style="font-weight: 400"> known bad hash </span><span style="font-weight: 400">in</span><span style="font-weight: 400"> chain </span><span style="font-weight: 400">05bef30ef572270f654746da22639a7a0c97dd97a7050b9e</span>
</span>

<b>Updates:
</b>
<ul>
	<li><b>20/08/15 17:47+02:00 </b><span style="font-weight: 400">Issue has been identified and fix for geth is underway. Geth(Go)-Miners should upgrade asap once the fixes become available and meanwhile switch to Eth (C++) or Python. Eth (C++) and Python miners are unaffected.</span></li>
	<li><b><b>20/08/15 </b>19:38+02:00 </b>Official release of fixes, see above.</li>
	<li><strong><b>20/08/15 </b>21:19+02:00 </strong>Note that the consenus  issue occured just before the announcement of the new release 1.0.2. Therefore, please make sure that you get the fix you want, i.e. remain on 1.0.1 or upgrade to 1.0.2. See "Fix" for more details.</li>
	<li><strong><b>21/08/15</b> 11:30+2:00 </strong>Since the occurrence of the consensus issue, we've been closely monitoring the chain and network. According to network statistics, most miners have upgraded to the patched 1.0.2 or switched to eth. Our expectations concerning maximum likely chain-reorganization depth is 750 blocks.</li>
</ul>