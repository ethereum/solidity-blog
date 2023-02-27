---
layout: post
published: true
title: 'Solidity Developer Survey 2022 Results'
date: '2023-02-26'
author: Franziska Heintel
category: Announcements
---

In this post, we will be summarizing and analyzing the results of the [2022 Solidity Developer Survey](https://blog.soliditylang.org/2022/12/07/solidity-developer-survey-2022-announcement/).

First of all, a big thank you to everybody who took the time and participated and to everybody who helped us spread the word about it!

This year, we received a smashing 1401 responses. That is more than a 3x in responses compared to the previous survey and we couldn't be happier with the turnout.

Your input is invaluable to us and plays a crucial role in helping to continuously improve the Solidity developer experience as a whole.

Before we get started, a few useful links:
* In the spirit of open source, you can find all raw data of the survey results [here](https://docs.google.com/spreadsheets/d/1A5iF3aKhFv9wTTJ10ko_uxgoflc8oEpVe3gqAAWoC2w/edit?usp=sharing) and all graphs [here](https://docs.google.com/presentation/d/1xH5pGZ6rrAP_jzRQobf0Mn1XYquyg8bD17DBQyrErMo/edit?usp=sharing).
* Since this is already our third time conducting a yearly survey, it may be interesting for you to compare the outcome to the previous surveys. The results from the 2021 developer survey are available [here](https://blog.soliditylang.org/2022/02/07/solidity-developer-survey-2021-results/) and from 2020 [here](https://blog.soliditylang.org/2021/01/26/solidity-developer-survey-2020-results/).

Without further ado, let’s dig into the 2022 results!

## Summary & Notable Insights

- **Survey Audience**: In total, 1401 developers from 100 different countries participated in the 2022 survey. That is more than a 222% increase in responses compared to the previous survey (435 respondents)! The coverage of different geographies also continuously increased from 48 countries in 2020 to 73 countries in 2021 to 100 countries in 2022. Roughly 18% stated to be residing in the US, followed by India (10%) and France (5%).
- **Developer Profiles**: The level of coding experience remains at a medium to high level with the majority of respondents having coded professionally 3 or more years, 12.5% even more than 15 years.
- **Solidity Experience**: More than half of all respondents have been using Solidity less than a year, while 13.8% have been using it more than 3 years. 41% use Solidity daily, 37.3% weekly.
- **Solidity Expertise**: Many deem themselves Solidity experts, with a self rating in expertise of 7 or higher (scale of 10). 4.6% rate their expertise as a 10 out of 10. 70% of those have been using Solidity for 2-3 years, or longer.
- **Developer Experience**: The majority (+75%) believes that the Solidity developer experience improved in the last year. 0.9% are of the opinion it got worse. Debugging issues are encountered most frequently, followed by stack-too-deep errors and bytecode size limitations.
- **Future Features**: Support for decimal numbers and generics were mentioned most often as the “most anticipated Solidity feature”.
- **Liked & Dreaded Features**: Respondents most like Solidity's syntax, the simplicity with regards to learning, reading, coding and compiling and the static typing. The biggest pain point is “stack-too-deep” with 33.6% of all votes, followed by missing memory optimizations (waste of memory) (24.4%) and redundant checks (e.g. in checked arithmetic) (11.8%). 9.9% say that compiler performance is their biggest issue.

![Demographics Header](/img/2023/02/header1.png)

## Demographics of the Survey Audience

_⚠️ Be aware that this survey has only been shared in English when interpreting results regarding the distribution of countries of residency and language preferences._

As usual, we begin by looking at the developers who participated in this survey: In this first chapter we cover general information on the survey audience, which includes residency, and spoken languages.

In total, 1401 developers from 100 different countries participated in the 2022 survey. Compared to the previous survey, this represents a 222% increase in responses.

The coverage of different geographies increased from 73 countries in 2021, to 100 countries in 2022.

#### Residency 

Roughly 18% stated to be residing in the US, followed by India (10%) and France (5%). Nigeria made it to rank 4, being the current residence of 4.5% of respondents.

![Survey Participants World Map](/img/2023/02/geo.png)

![Survey Participants List of Countries with 20+](/img/2023/02/geo2.png)

#### Language

The diversity of respondents did not only increase in terms of countries of residency, but also in terms of native language. In total, 70 different languages were mentioned as native language. That’s a 40% increase compared to the previous year.

31.5% state English as their native language, followed by Indian languages (12.4%), Spanish (8.4%), French (6.9%), Russian (6.4%) and German (4.7%).

_ℹ️Hindi, Urdu, Telugu, Bengali, Tamil, Malayalam, Gujarati, Marathi, Kannada and Odia were clustered as “Indian languages”. Chinese, Cantonese and Mandarin were clustered as “Chinese languages”. Persian, Pashto and Ossetian were clustered as “Iranian languages”._

![Survey Participants Native Language](/img/2023/02/ang.png)

With almost 80%, the majority of respondents predominantly speaks English at work. 

Other languages that are spoken at work: French (3.2%), Russian (3.1%) and Chinese languages (2.3%).

![Survey Participants Work Language](/img/2023/02/lang_work.png)

Of the respondents who didn't name English as their native language, 87% are okay with reading the Solidity documentation in English. 12.9% would prefer to read it in their native language, the most mentioned ones being Spanish, Indian languages and Russian. 

![Preferred Documentation Language](/img/2023/02/lang_docs.png)

![Preferred Documentation Language Breakdown](/img/2023/02/lang_docs_integrated.png)

_ℹ️ Note: This survey has only been conducted in English, which may have impacted the outcome of this question. We still believe internationalization of resources like the Solidity documentation is a crucial factor to lower the barriers of entry and we aim to support by helping coordinate the community-driven [translation efforts](https://github.com/solidity-docs)._

![Developer Profile Header](/img/2023/02/header2.png)

### Developer Profile

In the second section of the Solidity Developer Survey we learn more about professional experience and coding preferences of the survey audience.

#### Work Experience & Employment

Roughly 71% of respondents were employed at the time of the survey, while roughly 12% stated they were students and 17% said they were currently not working professionally.

Compared to the previous survey, there is a slight increase in both the number of students, and currently unemployed developers.

![Employment Status](/img/2023/02/employment.png)

The employed respondents predominantly work in the “crypto” (58.2%) and technology (21.6%) and financial services (5.4%) sector. 

![Industry Sector Breakdown](/img/2023/02/sector.png)

37,1% of all respondents are seniors and have been coding professionally for 6 years or more, 12.5% of them even for 15+ years.

On the other side, roughly 12% are coding newbies and have only coded professionally for less than a year. 

With approximately 22%, the biggest group sits in the middle of the distribution and has professional coding experience of 3-5 years.

Overall, the level of coding experience is medium to high with the majority of respondents (59.2%) having coded professionally for 3 or more years.

7.7% have never coded as part of their job, 37% of which are students.

![Professional Coding Experience](/img/2023/02/coding_xp.png)

#### Touch Points with Solidity

Like in the previous survey, the majority of respondents (75.7%) still use Solidity for their personal projects. Roughly 64% of all respondents use Solidity at work.

More than 20% state they are leading a programming team.

![Survey Participants Touch Points with Solidity](/img/2023/02/touchpoints.png)

Only 23.4% of respondents contribute to open-source projects written in Solidity on a daily or weekly basis. The rest states to do so monthly (27.4%) or never (47.1%). 

![Survey Participants Open Source Contributions](/img/2023/02/open_source.png)

#### Programming Language Preferences

Solidity marks the most used programming language for the survey audience (28.6%), closely followed by JavaScript (25.6%) and TypeScript (20.5%). Other less frequently mentioned languages are Python (8.7%), Rust (2.7%) and Go (2.5%).

![Most Used Programming Language](/img/2023/02/progr_lang_use.png)

Similar to the previous year, the respondents' favorite programming languages are distributed more evenly across various languages. Solidity is most popular, scoring 18.8% of all entries, followed by JavaScript (17.3%), Python (15.2%), TypeScript (15.0%), and Rust (8.6%).

![Favorite Programming Language](/img/2023/02/progr_lang_fav.png)

#### Operating System

Most respondents use MacOS as their primary Operating System (41.8%). Windows and Linux seem comparatively popular with 30.5 and 27.7%.

![Operating System](/img/2023/02/os.png)

![Solidity User Profile Header](/img/2023/02/header3.png)

### Solidity User Profile

In this section of the survey, we asked respondents about their Solidity-specific development experience and usage habits.

#### Solidity Experience

Almost 50% of all respondents deem themselves Solidity experts, with a self rating in expertise of 7 or higher (scale of 10).

4.6% rate their expertise as a 10 out of 10. 70% of those have been using Solidity for 2-3 years, or longer.

Roughly 23% can be considered beginners or low-frequency users with a self-rated expertise level of 4 or lower.

The distribution of self rating remained similar to the previous survey, even though the survey audience tripled in size.

![Solidity Expertise Level](/img/2023/02/sol_expertise.png)

Roughly 50% of all respondents have been using Solidity for less than a year, with 13% having just started their Solidity journey (less than three months of experience). 

13.83% have been using Solidity for more than 3 years and can thus be considered “Solidity seniors''. To put years into perspective: [“Version 0.1.1”](https://github.com/ethereum/solidity/commits/15dc5954c3a2e2a9ce96f2f77d41adef98a4cced), the oldest version of Solidity on ``solc-bin``, is from August 2015 and thus roughly 7.5 year old. The language is still relatively young and continues to evolve. We may add more granular selection options for “more than 3 years” of Solidity experience to distinguish this better in the next survey.

![Solidity Experience Level](/img/2023/02/sol_experience.png)

As in previous years, Solidity remains to appear rather easy to learn with 21.2% of respondents feeling productive in less than a month and 39.3% in less than half a year.

8.1% needed more than a year to feel comfortable with the language. 

17.8% don't feel productive yet, out of which more than 74.2% are beginners and have been using Solidity for 6 months or less, 47% even less than three months.

![Time to Productiveness](/img/2023/02/sol_productivity.png)

#### Solidity User Profile and Usage Habits

With regards to usage frequency, more than 40% of respondents use Solidity on a daily basis! 37.3% use it weekly and 13.9% on a monthly basis. 

Roughly 8% indicated to be using Solidity "rarely" or "never", most of which indicated before that they use Solidity for personal projects and code in a different programming language at work.

![Solidity Usage Frequency](/img/2023/02/sol_frequency.png)

A striking 81.8% of all respondents use Visual Studio Code as their editor when writing Solidity code. Vim and IntelliJ follow on the second and third rank with 3.7 and 3.4% usage respectively.

Compared to the previous survey in 2021, Visual Studio code gained significantly in popularity (from roughly 50% to 81.8%).

![Editor Overview](/img/2023/02/editor.png)

Depending on the chosen IDE, we also asked respondents which Solidity-related plugins they use, if any. “HardHat VSCode” by Nomic Foundation and the “Solidity” extension by Juan Blanco (both for Visual Studio Code) are the most popular.

![Editor Plugins Overview](/img/2023/02/editor_plugins.png)

Hardhat remains the most popular Ethereum-specific development environment with roughly 75% of all respondents using Hardhat.

Remix follows with 42%. Foundry has significantly increased its share from 1.6% in 2021 to 30% in 2022.

Truffle continues to move more into the background, with 17% of respondents indicating that they use it.

Rather "niche" Ethereum-specific development environments are Brownie (6.7%), Ape (3.3%), Dapptools (2.3%) and Embark (0.8%). 

4.4% of respondents are not using any Ethereum-specific development environment.

It’s worth noting that this question was a checkbox question, allowing respondents to select multiple answers.

_⚠️ Comparing the results from 2020, 2021 to 2022 may offer some insights like Truffle losing a significant share (2020: 59.3% -> 2021: 26.2% -> 2022: 17%), while Hardhat, and newcomers like Foundry increased their share in users. However, it's important to take into account that the previous surveys had significantly fewer responses (2020: 194, 2021: 435, 2022: 1401). A year-on-year comparison can only be interpreted as a loose trend and it’s not the intent of this study to analyze user splits between IDEs in detail._

![Ethereum IDE Overview](/img/2023/02/eth_ide.png)

With roughly 90%, 0.8.x Solidity versions remain to be the by far most used ones. The usage share of both the 0.7.x (10.2%) and the 0.6.x (7.7%) series continues to decrease since the previous survey. Everything older than that is hardly in use anymore.

![Used Solidity Versions](/img/2023/02/versions.png)

_*⚠️ Reminder: Please make sure to frequently update your code. [Several important bug fixes and security improvements](https://github.com/ethereum/solidity/blob/develop/docs/bugs_by_version.json) are added in the newer versions!*_

#### Solidity Usage Details

This year, we also asked specific questions about Solidity usage habits.

For charts and figures on those, please refer to the [presentation with all graphs]() and the [raw data file]().

To summarize:
Command line: Roughly two thirds of respondents do not use the Solidity compiler directly via the command line. 37.5% do.
Command line: When using the compiler on the command line, 61.3% still use Standard JSON.
Compiler support for old EVM versions: The vast majority of 82.6% of respondents does not rely on compiler support for older EVM versions. While 17.4% indicated they do require compiler support for older EVM versions, when asked which one specifically, only seven were able to point to an actual EVM version. All other responses were referring to Solidity versions. The answers indicate a clear misunderstanding of the question and the response can likely be interpreted as "no major support for older EVM versions needed".
Optimizer: 93.6% do not disable the optimizer. The 6.4% that enabled the optimizer, stated that they would do so due to contract size limits, slow compilation, in order to pass EtherScan verification, for gas testing purposes or because of security concerns.
Gas estimator: 23.4% use the gas estimator that is built into the compiler. 25% have tried it, but don’t use it regularly, while 41.5% never use it.
SMTChecker: 81% of all respondents never use the SMTChecker. 13.7% have tried it and 5.4% use it frequently. You can learn more about the SMTChecker [here]().
`via-IR` complication pipeline: 70.8% do not know what `via-IR` is. 18.6% use the `via-IR` pipeline already. In the following weeks we will share more context about why you should switch from the legacy compilation pipeline to `via-IR` and what this means.
Metadata publication: 53.5% publish the metadata of their smart contracts. 27.8% don’t, while 18.7% don’t know what this means.
Sourcify: 11% of all respondents use [Sourcify]() for smart contract verification, while 21.2% claim to not need it. 67.8% don’t know what Sourcify is. If you want to learn more about Sourcify, visit [sourcify.dev](https://sourcify.dev/).

#### Fixed-Point Types

Approximately 91% of all survey respondents don’t use fixed-point types.

The 9% (100 people) that do primarily use [PRB Math](https://github.com/PaulRBerg/prb-math), [solmate’s FixedPointMathLib](https://github.com/transmissions11/solmate/blob/main/src/utils/FixedPointMathLib.sol), and custom implementations.

![Fixed-Point Types Usage](/img/2023/02/fixed_point_types.png)

![Fixed-Point Types Libraries](/img/2023/02/fixed_point_types_libs.png)

#### Other EVM Networks

More than half of all respondents use Solidity outside of Ethereum mainnet and testnets. When asked which other networks they deploy their smart contracts on, the most popular chain was by far Polygon (formerly Matic Network). Other often mentioned blockchains include Binance Smart Chain, Arbitrum, Avalanche and Optimism.

![Deployment To Other Chains](/img/2023/02/solidity_other_chains.png)

![Deployment To Other Chains Breakdown](/img/2023/02/solidity_other_chains_breakdown.png)

#### Other Smart Contract Languages

Half of all respondents use other smart contract languages alongside Solidity. The most used other smart contract language is [Yul](https://docs.soliditylang.org/en/latest/yul.html), an intermediate language for Solidity, with 17.2%, followed by [Vyper](https://docs.vyperlang.org/en/latest/index.html), a pythonic EVM language, with 10.5%. [Cairo](https://www.cairo-lang.org/docs/) (7.1%), a STARK based language targeting StarkNet, and [Huff](https://docs.huff.sh/) (6.2%), a low level assembly language for the EVM, are also mentioned several times. Other “newcomers” like [Sway](https://fuellabs.github.io/sway/v0.35.3/book/) (2.4%) and [Fe](https://fe-lang.org/docs/index.html) (1.5%) also make it into the chart.

![Other Smart Contract Languages](/img/2023/02/smart_contr_langs.png)

![Solidity Developer Experience Header](/img/2023/02/header4.png)

## Solidity Developer Experience

76.5% of all respondents believe that the Solidity developer experience generally improved throughout the last year. 25.1% are of the opinion that they noticed a big improvement compared to the previous year.

7.8% say that nothing has changed in their experience, while 0.9% of the respondents think it got worse.

Compared to the previous year’s results, the share of “got worse” and “I don’t know” decreased, while “no change” slightly increased. Overall, the picture is very comparable.

![Solidity Developer Experience](/img/2023/02/dev_ex.png)

When getting stuck on a Solidity problem, most respondents visit Ethereum StackExchange / StackOverflow for help or search for a solution on the internet. Many also ask their coworkers for help or watch tutorial videos. 

![Solving Problems](/img/2023/02/solidity_problem.png)

### Recurring Issues

60% of respondents don't encounter the same or similar issues multiple times when developing in Solidity.

Amongst the 40% that do,debugging issues are encountered most frequently, followed by stack-too-deep errors and bytecode size limitations.

_ℹ️On the topic of debugging issues, we'd like to use the opportunity to highlight a new initiative aimed at defining a common debugging data format for languages built on top of the EVM: [ethdebug](​​https://github.com/ethdebug/format/). The end result will be a specification that will allow debuggers, analyzers and other tools to reliably map between the EVM bytecode produced by compilers and the high-level language features. This has been a common pain point across the ecosystem for years and is becoming more pressing with the introduction of the new IR-based code generator in Solidity, which often breaks implicit assumptions tools made based on how the legacy pipeline worked.

We encourage all developers working on such tools to join the working group. The groups has regular bi-weekly meetings and coordinates via the [ethdebug channel](https://matrix.to/#/#ethdebug:matrix.org) on Matrix._

![Recurring Issues](/img/2023/02/recurring_issues.png)

![Recurring Issues](/img/2023/02/recurring_issues_breakdown.png)

### Getting Started & Documentation

Most respondents considered it easy or “okay” to get started using the Solidity compiler. 4.2% (55 people) stated that it was difficult for them. When asked what made it difficult to get started, some mentioned a previous lack of technical background or development experience, others also pointed out a lack of good learning resources or outdated learning resources.

![Getting Started](/img/2023/02/getting_started.png)


Almost 64% of survey respondents consider the Solidity documentation helpful, followed by 33% who deem it “somewhat” useful. Only 3.3% don’t find it useful at all. 

Ideas for improvement most prominently ask for more code examples, but also a better high-level overview of syntax, better in-docs search, better SEO and easier wording.

![Solidity Documentation Usefulness](/img/2023/02/docs.png)

### Biggest Pain Points

Different from the previous years, this year, we tried to structure the question around the “biggest pain points” better and clustered the first step into several prominent categories: Stack-to-deep, gas related issues, compiler performance and “other”. 

The biggest pain point is “stack-too-deep” with 33.6% of all votes, followed by missing memory optimizations (waste of memory) (24.4%) and redundant checks (e.g. in checked arithmetic) (11.8%). 9.9% say that compiler performance is their biggest issue.

15.8% selected “other” and were able to specify their biggest pain point in a free text field. Most prominently mentioned: Contract size limit, error messages and issues with debugging.

![Solidity Pain Points](/img/2023/02/pain_points.png)

### High Impact Compiler Bugs

As part of this year’s study we were also curious to find out whether Solidity developers had been affected by any of the high impact compiler bugs (codegen bugs that are announced with [Security Alerts](https://blog.soliditylang.org/category/security-alerts/) on the Solidity blog).

Initially, 4.7% said yes. However, when asked which one they were affected by, only two out of the 63 people were able to point to actual Solidity vulnerabilities in the follow up question. This gives room to assume the actual number of affected developers from this survey is significantly lower than 4.7% and some respondents may have simply misunderstood the question.

![High Impact Bugs](/img/2023/02/security_bugs.png)

## Language Design & Upcoming Features

### Favorite Feature / Solidity Aspect

Respondents most like Solidity's syntax, the simplicity with regards to learning, reading, coding and compiling and the static typing.

The most mentioned liked features in descending order were:

* Syntax
* Easy to... read/code/compile/learn
* Simplicity
* Static/strong/strict typing
* Similarity to other languages (most mentioned JS/TS and object-orientedness, also mentioned: Rust, C++, Python)
* Modifiers
* Inline assembly
* Mappings
* Inheritance
* (User-defined) types
* SafeMath / checked and unchecked
* Yul

### Most Dreaded Aspect

This year, we asked the survey audience a slightly different question: “If you could change one thing about Solidity, what would it be?”

Most mentioned “change requests” in descending order were:

* Fix stack-too-deep
* Better array handling
* Gas optimizations
* Add decimal math (fixed point types / floating types)
* Better error handling, descriptions
* Better debugging
* Higher contact bytecode size limit

### Future Features

#### Most Anticipated Feature

Support for decimal numbers and generics were mentioned most often as the “most anticipated Solidity feature”.

_⚠️ Similar to the previous year we noted that respondents were using various different terms like "floats", "floating point arithmetic", "floating point number", "fixed point numbers", "fixed point math". We categorized those as "decimal numbers"._

Most mentioned anticipated features in descending order:

* Support for decimal numbers (fixed point types / floating points)
* Generics
* Better optimization
* No stack-too-deep
* Better debugging
* Transient storage
* Standard library
* Better error messages

#### EIP Support

We also wanted to know what Solidity-related EIPs the survey respondents think need support in the compiler.

It’s interesting to note that only 93 out of 1401 respondents gave meaningful answers to this question.

EIP-2335 “‘Diamonds, Multi-Facet Proxy” was mentioned most often, followed by EIP-1153 “Transient Storage” and EIP-3540 “EOF - EVM Object Format”.

![Solidity-related EIPs](/img/2023/02/eip_support.png)

### Restrictiveness

In terms of language restrictiveness, roughly 43% of respondents wish that Solidity stays “as is”. 41% tend towards more restrictive/explicit, having more checks, while approximately 16% would like Solidity to be less restrictive.

![Solidity Restrictiveness Ranking](/img/2023/02/restrictiveness.png)

![Solidity Developer Community Header](/img/2023/02/header6.png)
## Solidity Developer Community

### Language Design Community Participation

Less than 10% of all respondents ever participated in Solidity language design related efforts.

The distribution between participating in forum discussion and proposing features or language changes as a Github issue is fairly similar, while language design discussions and feedback calls have slightly less participation (all between 80 - 108 people, multiple selection possible).

Of the roughly 90% that did not participate in language design, most state they don’t know how, followed by being “too busy with work or other things”. Roughly 30% say that they are not interested in or qualified for the discussions.

![Language Design Participation](/img/2023/02/lang_design.png)

### Staying Informed

Similar to the previous years, most people like to stay up-to-date about Solidity versions, security alerts and announcements by following Solidity on Twitter or Mastodon. Other often used means for information are the Solidity blog and Solidity GitHub release page.

Interestingly, almost 30% claim to not be doing any of the above.

As part of “other”, respondents specified several community based means to stay up-to-date:

YouTube
Crypto Twitter / Community chats
Solidity docs
"Week in Ethereum" Newsletter
"Crypto influencers" / Popular Solidity developers
Updating RemixIDE / Hardhat / VS Code
Coworkers
Google
Newsletters
Conferences / Meetups
OpenZeppelin forum
Solidity website
Reddit

![Means To Stay Up-To-Date](/img/2023/02/stay_informed.png)

### Interaction with Other Solidity Developers

More than half of respondents interact with other Solidity developers. 

16.7% don’t interact with other Solidity developers at all.

![Developer Interaction](/img/2023/02/interaction.png)


Like in the previous years, as the last part of the survey, we wanted to hear how many participants agree or disagree with several statements regarding the Solidity community and the work of the Solidity team.

* 66% of respondents feel (somewhat) welcome in the Solidity developer community.
* Roughly 77% agree or somewhat agree that they feel confident in the work of the Solidity team.
* More than half feel welcome to contribute to Solidity, however only less than 40% say that they know how to contribute ideas or feedback to Solidity.
* Roughly 25% are confident that the Solidity team understands their needs as a developer. Another 35% somewhat agree, while approximately 9% disagree or strongly disagree.

The results of this “community and Solidity team confidence ranking” are very comparable to the previous year.

One can derive that while the community seems confident in the competency / qualification of the Solidity team, the communications around ways to contribute as well as understanding of the community’s needs can be improved. Those are things that we have been working on improving throughout the last years and will continue to do so.

![Community and Solidity Team Confidence Ranking](/img/2023/02/statements.png)



## Thank You & See You Next Year!

Lastly, we want to take the opportunity to thank you for all your lovely and motivating messages and the feedback received.

We were overwhelmed by the amount of survey responses and hope to be continuing this trend in the coming years!

We hope the insights from this survey were useful to you, they certainly are for us!

We will continue to collect feedback on an ongoing basis.

To not miss anything make sure to:
* Follow Solidity on [Twitter](https://twitter.com/solidity_lang) or [Mastodon](https://fosstodon.org/@solidity).
*Join the language design discussions in the [Solidity forum](https://forum.soliditylang.org/) or provide us feedback there.
* Follow announcements and security alerts on the [Solidity blog](https://blog.soliditylang.org/).
* Follow and ⭐ the [Solidity repo on Github](https://github.com/ethereum/solidity).

--- 
*All graphs can be found [here](https://docs.google.com/presentation/d/1xH5pGZ6rrAP_jzRQobf0Mn1XYquyg8bD17DBQyrErMo/edit?usp=sharing). The raw and analyzed data can be found [here](https://docs.google.com/spreadsheets/d/1A5iF3aKhFv9wTTJ10ko_uxgoflc8oEpVe3gqAAWoC2w/edit?usp=sharing).*