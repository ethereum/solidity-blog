---
id: 2190
title: 'Security Advisory [eth (cpp-ethereum) potentially vulnerable if running with UPnP enabled]'
date: 2015-10-10T11:51:20+00:00
author: Gustav Simonsson
layout: post
guid: https://blog.ethereum.org/?p=2190
permalink: /2015/10/10/security-advisory-eth-cpp-ethereum-potentially-vulnerable-if-running-with-upnp-enabled/
dsq_thread_id:
  - "4211657449"
category: Security
---
<section class="postbody"><b>Affected configurations: </b>Issue reported for eth (cpp-ethereum).</section><section class="postbody"></section><section class="postbody"></section><section class="postbody"></section><section class="postbody"></section><section class="postbody"></section><section class="postbody"></section><section class="postbody"><b>Likelihood: </b>Medium</section><section class="postbody"></section><section class="postbody"></section><section class="postbody"></section><section class="postbody"></section><section class="postbody"></section><section class="postbody"><b>Severity: </b>High</section><section class="postbody"></section><section class="postbody"></section><section class="postbody"></section><section class="postbody"></section><section class="postbody"><b>Impact: </b>Potentially achieve remote code execution on a machine running eth (cpp-ethereum)</section><section class="postbody"></section><section class="postbody"></section><section class="postbody"><b>Details:</b></section>A <a href="http://talosintel.com/reports/TALOS-2015-0035/">vulnerability found in the MiniUPnP library</a> can potentially affect eth clients running with UPnP enabled.

<section class="postbody"></section><section class="postbody"><b>Effects on expected chain reorganisation depth: </b>none</section><section class="postbody"></section><section class="postbody"></section><section class="postbody"></section><section class="postbody"></section><section class="postbody"></section><section class="postbody"><b>Remedial action taken by Ethereum</b>: We are verifying whether this can indeed affect cpp-ethereum and will post an update shortly.</section><section class="postbody"></section><section class="postbody"></section><section class="postbody"></section><section class="postbody"></section><section class="postbody"></section><section class="postbody"><b>Proposed temporary workaround:</b> Only run eth (cpp-ethereum) with UPNP disabledby passing <strong>--upnp off </strong>to eth.</section><section class="postbody"></section><section class="postbody"></section><section class="postbody"></section><section class="postbody"></section><section class="postbody"><b>ADVISORY: Disable UPnP if running the eth client (cpp-ethereum).
</b></section>