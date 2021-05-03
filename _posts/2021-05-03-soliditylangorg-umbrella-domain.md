---
layout: post
published: true
title: 'What Happened with Solidity-related Domains?'
date: '2021-05-03'
author: Franziska Heintel
category: Explainers
---

Some time ago we decided to get a domain that the Solidity team has easy access to in
order to streamline efforts and initiatives that were hosted on other domains before.

And so ``soliditylang.org`` was born! 🎉

We announced most of these domain changes individually [on Twitter](https://twitter.com/solidity_lang/status/1329031139495964673?s=20), but we want to take a moment to
also officially announce it here on the blog
and explain the various subdomains we have now, what they are for and which older
domains may be deprecated in the future.

In short, we currently maintain the following sites under the soliditylang.org umbrella:

- Solidity Portal: [soliditylang.org](https://soliditylang.org/)
- Documentation: [docs.soliditylang.org](https://docs.soliditylang.org/)
- Blog: [blog.soliditylang.org](https://blog.soliditylang.org/)
- Compiler Binaries: [binaries.soliditylang.org](https://binaries.soliditylang.org/)
- Forum: [forum.soliditylang.org](https://forum.soliditylang.org/)
- Solidity Summit: [summit.soliditylang.org](https://summit.soliditylang.org/)
- Solidity Underhanded Contest: [underhanded.soliditylang.org](https://underhanded.soliditylang.org/)


## Solidity Portal

The Solidity language portal at [soliditylang.org](https://soliditylang.org/) is a one-stop-shop for all important links and high-level information around Solidity. It includes information on the latest Solidity release, how to get started, information on language design and features recent blog posts and news.

## Documentation

The Solidity documentation was originally available at ``solidity.readthedocs.io``. Now, the readthedocs domain redirects to its new home: [docs.soliditylang.org](https://docs.soliditylang.org/). Since it's a permanent redirection ([HTTP 301](https://en.wikipedia.org/wiki/HTTP_301)), all old links are still working and will take you directly to the correct section in the docs on the new domain.

## Blog

The Solidity blog was originally available at ``solidity.ethereum.org``. Now, the blog is at home at [blog.soliditylang.org](https://blog.soliditylang.org/) and similarly to how it is  handled with the docs, the transition should have felt seamless to users. All old links still work and redirect to the corresponding equivalent on the new domain.

## Compiler Binaries / Solc-Bin

We maintain a repository containing static builds of past and current compiler versions for all supported platforms at ``solc-bin``. This is also the location where you can find the nightly builds. 

``solc-bin`` is available at two locations:
- [binaries.soliditylang.org](http://binaries.soliditylang.org/)
- [solc-bin.ethereum.org](http://solc-bin.ethereum.org/)

Originally, the Solidity binaries were hosted at ``ethereum.github.io/solc-bin/`` and ``solc-bin.ethereum.org`` was pointing at it. Since we didn't communicate clearly enough at the time that we recommend users to use the ``solc-bin.ethereum.org`` domain, some users referred to the ``github.io`` page directly instead.

The ``github.io`` hosting has a 10 GB size limit and we exceeded it so we had to switch to a different one. For people who were using the ``ethereum.org`` domain the transition should have been seamless but there was nothing we could do about ``github.io`` and we had to just deprecate it. 

We left it up but it hosts binaries only up to 0.7.2 and does not have the native binaries.

Around the same time we got the ``soliditylang.org`` domain and started moving all Solidity-related stuff under it. Both [binaries.soliditylang.org](https://binaries.soliditylang.org) as well as [solc-bin.ethereum.org](https://solc-bin.ethereum.org) are okay to use and are fully supported. The new URL is recommended but no one has to change it just for the sake of it.

You can find all information on static binaries in our documentation [here](https://docs.soliditylang.org/en/latest/installing-solidity.html#static-binaries).


## Forum

In our effort to foster exchange of information, encourage more developers to give feedback about Solidity and join the discussions on language design and future direction of the compiler, we launched the Solidity forum in February of this year. The forum is available at [forum.soliditylang.org](https://forum.soliditylang.org/). You can read all details about the forum in [this announcement](https://blog.soliditylang.org/2021/02/01/launching-the-solidity-forum/).

## Special Purpose Websites (e.g. Solidity Summit and Solidity Underhanded Contest)

We regularly host Solidity-related events like conferences or hackathons. Event-related websites are also part of the ``soliditylang.org`` umbrella now.

For example, you can find the Solidity Summit at [summit.soliditylang.org](https://summit.soliditylang.org/). _(The Solidity Summit is a free interactive forum for people involved and interested in the Solidity language and the ecosystem around it.)_

Similarly, information on the Underhanded Contest is available at [underhanded.soliditylang.org](https://underhanded.soliditylang.org/). _(The goal of the Solidity Underhanded Contest is to write innocent-looking Solidity code, which pretends to be clear and straightforward, but actually contains malicious behavior or backdoors.)_

Other initiatives and events may find their home under the ``soliditylang.org`` domain umbrella in the future. 

Feel free to reach out to us with any remaining questions you may have. 😊
