---
id: 643
title: 'Ether Sale: A Statistical Overview'
date: 2014-08-08T17:23:58+00:00
author: Vitalik Buterin
layout: post
guid: https://blog.ethereum.org/?p=643
permalink: /2014/08/08/ether-sale-a-statistical-overview/
dsq_thread_id:
  - "2911282042"
---
The first two weeks of the ether sale are over, and we have to date received <a href="https://blockchain.info/address/36PrZ1KHYMpqSyAQXSG8VwbUiq2EogxLo2">over 25000 BTC</a> from selling over 50 million ETH. This marks the largest cryptographic token sale to date, and with the two endowments places ETH as being the token with the <a href="http://coinmarketcap.com/">8th highest total value</a>, even beating out the beloved Dogecoin at $17.3M USD vs $15.5M. A total of 6670 transactions have been made, with values ranging from the minimum 0.01 BTC to a high of 500 BTC, and purchases continue to come in every hour. Additionally, the ether sale marks the largest use of multisig to date; because of our sale, the <a href="http://p2sh.info/p2sh-ratio">percentage of all BTC stored in multisig</a> has shot up from 0.23% to 0.41% over the last two weeks alone - in other words, the 3-of-4 private keys split between our various sites control 45% of all BTC stored in multisig addresses in existence.

The purpose of this post will be to provide an overview of some statistics from the sale so far. Data was taken yesterday, when we had 24000 BTC, and assumes that all purchases were for 2000 ETH / BTC (an assumption that is not strictly true, but the error term is sufficiently tiny that it can safely be discounted). First we have <a href="https://docs.google.com/a/ethereum.org/spreadsheets/d/1H5w9YVp1eRoNP8N9UFFvCcz51Q5DxzJaOVliCAAT46g/edit#gid=0">this spreadsheet</a>, which shows the ether purchases over time:

<iframe height=406 width=600 src="//docs.google.com/a/ethereum.org/spreadsheets/d/1H5w9YVp1eRoNP8N9UFFvCcz51Q5DxzJaOVliCAAT46g/gviz/chartiframe?oid=342166142" seamless frameborder=0 scrolling=no></iframe>

The individual spikes are per-block; the chart shows that the distribution is heavily divided into two clusters, with one cluster  closer to the start of the sale and the other close to the end of the full-discount period. Purchases drop off sharply once the new price level of 1970 ETH/BTC (now 1910 ETH/BTC) kicked in. Theoretically, purchasing near the end of the full-discount period is the more optimal strategy from a naive game-theoretic model; if you purchase near the end of the full-discount period then you get the same price as people who purchased at the start, but also gain the benefit of having more information - namely, a better idea of the exact percentage of all ETH that you are going to get. Thus, the fact that the majority of purchases occurred at the end shows that ether purchasers are generally a rather sophisticated audience - which I suppose you should be if you managed to be convinced to trade your hard-earned BTC for some cryptographic tokens backed by a concept of "generalized consensus computing".

Of course, it is important to note that there are reasons to buy at the start too. Some people are participating in the sale out of a desire to support the project, and some large purchasers may have perhaps had the <a href="https://www.quora.com/Whats-the-psychology-behind-tip-jars-Are-people-more-or-less-likely-to-tip-if-the-jar-is-empty-or-if-its-full">priming effect</a> in mind, where putting larger sums of money (eg. bills) into a tipping jar at the very beginning increases the total amount received because it creates the impression that the recipient is significant and merits more and larger contributions.

At this point, we can expect to see a declining flow that will stabilize over the next few days, and then a smaller final spike on day 42. The chart below shows the cumulative ether sold up until this point:

<iframe height=407 width=600 src="//docs.google.com/a/ethereum.org/spreadsheets/d/1H5w9YVp1eRoNP8N9UFFvCcz51Q5DxzJaOVliCAAT46g/gviz/chartiframe?oid=831527247" seamless frameborder=0 scrolling=no></iframe>

The other interesting thing to analyze is the distribution of purchases. <a href="https://docs.google.com/spreadsheets/d/1GS9pzSdMx9lK0XGSKEDr_aoi02riq3MPRyvEntVUm68/edit#gid=0">This spreadsheet</a> contains a list of purchases arranged by purchase size. The largest single purchase was 500 BTC (1 million ether), followed by one at 466 BTC (933,580 ETH) and 330 BTC (660,360 ETH). We have not received any requests at largepurchases@ethereum.org. If we arrange purchases by size, we get the following two graphs, one for the quantity of purchases and one for the amount of ETH purchased, by purchase size:

<iframe height=371 width=600 src="//docs.google.com/a/ethereum.org/spreadsheets/d/1GS9pzSdMx9lK0XGSKEDr_aoi02riq3MPRyvEntVUm68/gviz/chartiframe?oid=168457404" seamless frameborder=0 scrolling=no></iframe>

<iframe height=371 width=600 src="//docs.google.com/a/ethereum.org/spreadsheets/d/1GS9pzSdMx9lK0XGSKEDr_aoi02riq3MPRyvEntVUm68/gviz/chartiframe?oid=846945325" seamless frameborder=0 scrolling=no></iframe>

Note that this only applies to purchases. There is also another slice of ether which will soon be distributed, which is the endowment. The portions in which the endowment is planned to be distributed are on <a href="https://docs.google.com/spreadsheets/d/1GS9pzSdMx9lK0XGSKEDr_aoi02riq3MPRyvEntVUm68/edit#gid=0">the spreadsheet</a>; the largest is equal to 0.922% of all ether purchased (ie. 0.369% of the total supply after five years) and the smallest is 0.004%, with 81 people total receiving a share. If you are one of the recipients, you will be contacted shortly; if you are not then there is still a second slice whose distribution has not been decided.

### Distribution and Gini Indices

As a final set of interesting statistics, we have calculated three Gini indices:

* Gini index of ether purchasers: 0.832207
* Gini index of endowment: 0.599638
* Gini index of entire set: 0.836251

A Gini index is a common measure of inequality; the way the Gini index is calculated is by drawing a chart, with both axes going from 0% to 100%, and drawing a line where the Y coordinate at a particular X coordinate is calculated as the portion of all income (or wealth) which is owned by the bottom X percent of the population. The area between this curve and a diagonal line, as a portion of the area of the entire triangle under the diagonal line, is the Gini index:

<img src="https://upload.wikimedia.org/wikipedia/commons/5/59/Economics_Gini_coefficient2.svg"></img>

In an ideal society of perfect equality, the coefficient would be zero; the bottom X% of the population would obviously have X% of the wealth, just like any other X% of the population, so the cumulative wealth distribution graph would be exactly the diagonal line and thus the area between the graph and the diagonal line would be zero. In the opposite scenario, an ultimate dictatorship where one person controls everything, the bottom X% would have exactly nothing all the way up until the last person, who would have everything; hence, the area between that curve and the diagonal line would be equal to the entire area under the diagonal line, and the coefficient would be exactly one. Most real-world scenarios are in between the two.

Note that Gini coefficients of wealth and Gini coefficients of income are different things; one measures how much people have and one measures the rate at which people receive. Because <a href="https://en.wikipedia.org/wiki/Marginal_propensity_to_consume">savings are superlinear in income</a>, coefficients of wealth tend to be higher; the Gini coefficient of wealth in the US, for example, <a href="https://en.wikipedia.org/wiki/List_of_countries_by_distribution_of_wealth">is 0.801</a>, and the coefficient of the world is 0.804. Given that Gini coefficients in the real world measure inequality of access to resources, and Gini coefficients in cryptocurrency distribution arise from both inequality of resources and inequality of interest (some people care about Ethereum slightly, some care about it a whole lot), 0.836 is a pretty decent result - as a point of comparison, the Gini coefficient of Bitcoin has been <a href="http://bitcoin.stackexchange.com/questions/86/is-it-possible-to-estimate-the-gini-coefficient-for-bitcoins-and-if-the-trend-is">measured at 0.877</a>. The top 100 current ETH holders are responsible for 45.7% of all ETH, a lower percentage than the top 100 holders <a href="http://bitinfocharts.com/">of the mainstream altcoins</a>, where that statistic tends to be between 55% and 70%.

Of course, these last two comparisons are misleading - the Ethereum ecosystem has not even started to actually run, and services like exchanges which centralize control over currency units into a few wallets without centralizing legal ownership do end up artificially inflating both the Gini index and the top-100 score of cryptocurrency networks that are actually live. Once Ethereum launches, the Gini index may well prove to be impossible to accurately estimate, since large quantities of ether will be stored inside decentralized applications running arbitrary, Turing-complete, and thus <a href="https://en.wikipedia.org/wiki/Halting_problem">in many cases mathematically inscrutable</a>, rulesets for how the ether can be withdrawn.

The sale still has 28 days left to go; although we are not expecting much out of this remaining period, anything is possible. With organizational issues being wrapped up, the organization is getting ready to substantially scale up development, putting us on the fast track to finally completing the Ethereum code and launching the genesis block; ETA winter 2014-2015.