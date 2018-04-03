---
id: 37
title: The Issuance Model in Ethereum
date: 2014-04-10T09:56:56+00:00
author: Joseph Lubin
layout: post
guid: http://blog.ethereum.org/?p=37
permalink: /2014/04/10/the-issuance-model-in-ethereum/
dsq_thread_id:
  - "2790050106"
---
Ether (ETH), the cryptofuel that powers distributed applications on the Ethereum platform, will be issued at a constant annual linear rate via the block mining process. This rate is 0.3 times the total amount of ETH that will be purchased in the pre-sale.

While the best metaphor for ETH is “fuel for running the contract processing engine,” for the purposes of this post, we will treat ETH purely as a currency.

There are two common definitions of “inflation.”  The first relates to prices and the second relates to the total amount of money in a system – the monetary base or supply.  Similarly for the term “deflation.”  In this post we will distinguish between “price inflation,” the rise in the general price level of goods and services in an economy, and “monetary inflation,” the growth in the supply of money in an economy due to some sort of issuance mechanism.  Often, but not always, monetary inflation is a cause of price inflation.

Though the issuance of ETH is in a fixed amount each year, the rate of growth of the monetary base (monetary inflation) is not constant.  This monetary inflation rate decreases every year making ETH a <b>disinflationary</b> currency (in terms of monetary base).  Disinflation is a special case of inflation in which the amount of inflation shrinks over time.

It is expected that the amount of ETH that will be lost each year caused by transmissions to addresses which are no longer accessible is estimated to be on the order of 1% of the monetary base. ETH may be lost due to loss of private keys, death of owner without transmission of private keys, or purposeful destruction by sending to an address that never had an associated private key generated.

If we assume that Ethereum sells 40,000 BTC worth of ETH in the pre-sale, and if we assume that the average price is 1500 ETH/ BTC, 60,000,000 ETH will be created in the genesis block and assigned to purchasers. Every year, in perpetuity, 18,000,000 ETH will be issued though the mining process.  Taking into account both creation of new ETH and loss of existing ETH, in the first year, this represents a monetary inflation rate of 22.4%.  In the second year the rate drops to 18.1%.  By the tenth year, the rate is 7.0%.  In year 38, it hits 1.9%. And in the 64th year, the level of 1.0% is reached.

<a href="http://blog.ethereum.org/wp-content/uploads/2013/12/issuance-model.jpg"><img class="size-full wp-image-85" src="https://blog.ethereum.org/wp-content/uploads/2013/12/issuance-model.jpg" alt="Figure 1.  Amount of ETH in existence (dark green curve) on the left axis.  Monetary base inflation rate (light green curve) on the right axis.  Years on the horizontal axis.  (Adapted from Arun Mittal with thanks.)" /></a>

Figure 1. Amount of ETH in existence (dark green curve) on the left axis. Monetary base inflation rate (light green curve) on the right axis. Years on the horizontal axis. (Adapted from Arun Mittal with thanks.)

By approximately the year 2140, the issuance of BTC ceases and since some BTC will likely be lost each year, the monetary base of Bitcoin is expected to start shrinking at that point.

At approximately the same time, the expected rate of annual loss and destruction of ETH will balance the rate of issuance.  Under this dynamic, a quasi-steady state is reached and the amount of extant ETH no longer grows. If the demand for ETH is still growing at that point due to an expanding economy, prices will be in a deflationary regime.  This is not an existential problem for the system since ETH is theoretically infinitely divisible. As long as the rate of price deflation is not too rapid, pricing mechanisms will adjust and the system will operate smoothly.  The traditional main objection to deflationary economies, wage stickiness, is likely not to be an issue since all payments systems will be fluid.  Another frequent objection, borrowers forced to repay loans with a currency that grows in purchasing power over time, will also not be a problem if this regime is persistent, since terms of lending will be defined to account for this.

Note that while the monetary inflation remains greater than zero for many years, price levels (tracked as price inflation and deflation) are dependent on supply and demand, so are related to, but not totally controlled by the rate of issuance (supply).  Over time it is anticipated that growth of the Ethereum economy will significantly outpace growth of the supply of ETH, which could lead to an increase in the value of ETH with respect to legacy currencies and BTC.

One of Bitcoin’s great value propositions was the algorithmically fixed total issuance of the currency which mandated that only 21,000,000 BTC will ever be created.  In a time of profligate legacy currency printing in an exponentially doomed attempt to patch over the fact that there is too much debt in the global economic system (with more debt), the prospect of a universally accepted cryptocurrency that can serve eventually as a relatively stable store of value is attractive.  Ethereum recognizes this and seeks to emulate this core value proposition.

Ethereum also recognizes that a system intended to serve as a distributed, consensus-based application platform for global economic and social systems, must strongly emphasize inclusiveness. One of the many ways we intend to foster inclusiveness is by maintaining an issuance system which possesses some churn.  New participants in the system will be able to purchase new ETH or mine for new ETH whether they are living in the year 2015 or 2115. We believe we have a achieved a good balance between the two goals of fostering inclusiveness and maintaining a stable store of value. And the constant issuance, especially in the early years, will likely make using ETH to build businesses in the Ethereum economy more lucrative than hoarding speculatively.