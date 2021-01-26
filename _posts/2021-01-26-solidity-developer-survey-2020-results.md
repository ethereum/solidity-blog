---
layout: post
published: true
title: 'Solidity Developer Survey 2020 Results'
date: '2021-01-26'
author: Franziska Heintel
category: Announcements
---

Before we dive into the results we want to extend a big thank you to all of the Solidity developers that participated in the very first Solidity Developer Survey, which we [conducted at the end of last year](https://blog.soliditylang.org/2020/12/09/solidity-developer-survey-2020/)! 

We were overwhelmed by the high quality of the submissions and are happy to extract important insights from your input.

In this post, we'll be summarizing and commenting on the results of the survey. 

Please note that none of the questions in the survey were mandatory. Hence, the amount of replies may vary per question.

### Summary & Notable Insights

- **Survey Audience**: 193 developers from all over the world replied to this survey, calling 48 different countries their current home. With roughly 20%, the big majority of respondents live in the United States of America, followed by India (8%) and Germany (7%). The majority of respondents speaks English at work.
- **Developer Profiles**: Roughly 65% of respondents have been using Solidity for more than a year, with 43% being "Solidity pros" with 2 or more years of experience with the language. Looking at the overall coding seniority of the respondents it appears that a significant number of Solidity users are experienced in other programming languages, as opposed to learning to program for the first time using Solidity.
- **Solidity Experience**: 43% of respondents rate themselves as Solidity experts with an expertise level of 8 (out of 10) or higher.
- **Solidity Developer Experience**: While noting many things that could still improve, there was an overall positive/good sentiment towards Solidity. Most respondents believed that the Solidity developer experience had some or much improvement over the last year.
- **Solidity's Challenges**: The biggest challenge in developer experience for most respondents is the availability and quality of tooling. Also seen as critical aspects in order to foster adoption are error handling, IDE integrations and better documentation.
- **Most Liked, Dreaded and Anticipated**: Topics with room for improvement that came up a lot: Error handling, debugging, the need for more tools and libraries, and breaking dependencies.
- **Contributing and Language Design**: There was surprisingly little interest and engagement in language design topics, partly because developers didn't know how to get involved, or because they were not interested. (We will work on this! Efforts to foster collaborative language design are under way, we'll share more soon.)
- **Community**: Almost 17% of participants in this survey do not interact with other Solidity developers at all!

### (1) Survey Audience and Developer Profiles

First, let's have a look at the developers that participated in the survey. In total, we received **193 responses from 48 different countries**. 

![Survey Participants World Map](/img/2021/01/World.png)

#### Residency

⚠️ _Note that the fact that this survey has only been shared in English is an important factor to consider when interpreting results regarding language and country of residency._

The respondents stated to live in Algeria, Argentina, Australia, Austria, Belgium, Brazil, Bulgaria, Canada, China, Costa Rica, Denmark, Finland, France, Germany, Ghana, Greece, Honduras, Hong Kong, India, Indonesia, Iran, Ireland, Israel, Italy, Japan, Lithuania, Malaysia, Mexico, Netherlands, New Zealand, Nigeria, Pakistan, Philippines, Poland, Portugal, Romania, Russia, Serbia, Singapore, South Africa, Spain, Switzerland, Taiwan, Thailand, Timor-Leste, Ukraine, United Kingdom and United States of America. 

With roughly 20%, the big majority of respondents live in the United States of America, followed by India (8%) and Germany (7%).

![Survey Participants List of Countries](/img/2021/01/Residency.png)

#### Language

The developers speak a wide variety of different languages as their primary language, with English being by far the most popular one. German, Spanish, Russian and French follow with similar levels.

![Primary Language](/img/2021/01/LangPrimary.png)

With 86%, the vast majority of respondents speak English at work. 

Almost 90% of respondents are okay with reading the Solidity documentation in English while roughly 10% would prefer it to be translated into their native language. Some of the respondents who chose "native language" stated English as their primary spoken language though, which results in 95% of participants preferring English for the documentation.

Bear in mind that this survey has only been conducted in English, which heavily influences the outcome of this question. This result will hence not have too much influence on the revamped translation efforts for the Solidity documentation and we will further investigate this topic.

![Language at Work](/img/2021/01/LangSpeak.png)


![Documentation Language](/img/2021/01/DocumentationLang.png)

#### Employment

82% of all respondents stated that they are currently employed. 4% identified as students, while 14% stated to not work at the moment. 

![Employment Status](/img/2021/01/Employment.png)

#### Industry

The majority of respondents (68%) said they work in the technology industry. 14% work in the financial services industry. Less dominant, but also selected several times, were the energy sector, health care, creative, media & gaming and transportation.

![Industry Overview](/img/2021/01/Industry.png)

#### Coding Experience

The overall, non-Solidity-related, professional coding experience of respondents was impressive. Around 48% of respondents said they coded as part of their jobs for 6 years or more. Only roughly 7% stated that they have less than 1 year of coding experience. 

![Professional Coding Experience](/img/2021/01/CodingExperience.png)

#### Operating System

Interestingly, Linux and macOS seem equally popular, both getting roughly 40% of the repondents' votes. Around 18% use Windows, although a part of these users also state to use another OS in addition to Windows. There are also several developers using both Linux and macOS, or even all three operating systems.

![Operating System](/img/2021/01/OperatingSystem.png)

#### Solidity Usage

Almost all of the respondents use Solidity for personal projects outside of work (93%). More than half of the respondents (68%) also say they use Solidity at work and/or code in another programming language at work. 

27% of respondents stated to be managing a programming team while 8% are students.

![Solidity Usage](/img/2021/01/SolidityUse.png)

#### Open-Source Code

Strikingly, the majority of respondents (61%) only infrequently or even never contribute to open-source projects written in Solidity. 21% claim to do it as part of their daily routine.

![Open-Source Coding](/img/2021/01/OpenSource.png)

#### Coding Preferences

We've asked the participants what is most important to them when choosing a programming language. The question had a free text reply field, hence we clustered the responses into overarching categories. 

The most important aspect, stated by many respondents, was the fact that the programming language has to fit the task, and it has to "get the job done".

Also ranked very highly were tooling, documentation and community, followed by security of the language, libraries, availability of good resources and learning material as well as a clear or familiar syntax. 

Less frequently mentioned, but still relevant, were descriptive error messages / good debugging, readability, performance and strong types.

Other aspects that were brought up several times were usability, productivity/efficiency, convenience, expressiveness, maintenance & support, simplicity, good developer experience and robustness (at scale).

![Coding Preferences](/img/2021/01/ChooseProgrammingLang.png)

#### Favorite Programming Language

When asked what their favorite programming language was, most respondents said JavaScript (40), followed by Python (33) and Solidity (30). Other popular programming languages were TypeScript (19), Go (15) and Rust (14).

![Favorite Programming Language](/img/2021/01/FavProgrammingLang.png)

#### Most Used Programming Language

The majority of respondents use JavaScript (36%) the most, followed by Solidity (21%) and TypeScript (11.5%). Also mentioned several times: Python (9%), Go (5%), C++ (3%) and Java (3%).

![Most Used Programming Language](/img/2021/01/ProgrammingLanguageUsed.png)

### (2) Solidity Learning Journeys

As the next step, we wanted to learn more about the individual learning journey of each respondent. While most respondents managed to feel productive in less than half a year, a quite big share (17%) does not feel productive at this point yet.

![Productivity Learning](/img/2021/01/Productivity.png)

#### Learning 

The [Solidity documentation](https://docs.soliditylang.org/en/latest/) is the most prominent learning material and primary resource for most respondents to learn Solidity. 

The practices of reading the open-source Solidity code of big projects as well as watching coding tutorial videos were frequently mentioned.

Another prominent way to get started learning Solidity is the [CryptoZombies](https://cryptozombies.io/) coding game. 

Many respondents used the [Ethereum StackExchange](https://ethereum.stackexchange.com/) to find answers to their questions. 

Massive open online courses (MOOCs) like Udacity, Udemy and university courses were mentioned by several respondents followed by active coding approaches like "learning by coding" using [Remix](https://remix.ethereum.org/) and the [OpenZeppelin contracts](https://openzeppelin.com/contracts/) as well as by going through the [ConsenSys Academy](https://consensys.net/academy). 

![Primary Learning Resources](/img/2021/01/PrimaryResource.png)

#### Problem Solving 

When getting stuck on a Solidity problem, most Solidity developers visit Ethereum StackExchange for help. 

Other popular solutions are asking friends or coworkers for help, watching tutorial videos or doing other work and trying to solve the problem later.

![Problem Solving](/img/2021/01/SolidityProblem.png)

When asked if developers encounter similar issues repeatedly, most responded with "no" or explained that with experience and usage, once they knew how to solve the problem they did not encounter the same issues repeatedly.

The following issues have been mentioned to still pop up regularly or to be annoying:
- Stack too deep errors.
- Error messages that don't point to the cause of the issue.
- Out of gas errors.
- Compilation errors.
- Tooling problems. 
- Verification difficulties.
- Breaking changes and broken dependencies.

### (3) Understanding the Solidity Developer Experience

#### Solidity Usage 

Roughly 65% of respondents have been using Solidity for more than a year, with 43% being real Solidity pros with more than 2 years of experience with the language.

It's interesting to see that a fair share of 35% is just getting started with Solidity and has only been using it for under 12 months. 

Looking at the overall coding seniority of the respondents, this data shows us that a majority of Solidity developers are developers with experience in other programming languages, rather than someone learning to program for the first time in Solidity.

![Solidity Usage](/img/2021/01/UsingSol.png)

43% of respondents rate themselves Solidity experts with an expertise level of 8 (out of 10) or higher. Only 10% rank themselves at the beginner stages with an expertise level of 3 or lower.

![Expertise](/img/2021/01/SolExpertise.png)

The majority of respondents use Solidity on a weekly (31%) or even daily basis (41%). Almost 18% use it monthly, while the remaining 9% stated that they rarely code in Solidity.

![Frequency of Usage](/img/2021/01/HowOftenSol.png)

Most respondents are currently using 0.6.x or 0.7.x versions of Solidity. A smaller share still uses 0.5.x versions or has been trying out the recently released 0.8.0 already.

A few people are not aware of the version they are using or are still using very old versions from the 0.4.x series. 

🚨 Please make sure to frequently update your code since [several important bug fixes](https://github.com/ethereum/solidity/blob/develop/docs/bugs_by_version.json#L922) and security improvements have been added since 0.4.x!

![Versions Used](/img/2021/01/SolidityVersion.png)


#### IDEs / Tooling

50% of respondents stated they are using VSCode as a preferred editor to write Solidity. The second favorite editor was Vim (12.6%), followed by Remix (9.2%), IntelliJ (8.8%) and Atom (7.1%).

![IDEs / Tooling](/img/2021/01/IDE.png)

When asked if they are using an Ethereum-specific development environment, only 3% say that they are not using one. [Truffle](https://www.trufflesuite.com/truffle) is the most prominent with 35%, closely followed by [Remix](https://remix.ethereum.org/) (29%) and [Hardhat](https://hardhat.org/) (23%).

![Ethereum-specific IDEs / Tooling](/img/2021/01/EthIDE.png)

#### Overall Solidity Developer Experience

Around 62% believe that the Solidity developer experience had some or even a lot of improvement in the last year. 15% saw a slight improvement while only 9% thought that there was no change or that the Solidity developer experience got worse.

![Overall Developer Experience](/img/2021/01/DevExperience.png)

The biggest challenge in developer experience for most respondents is the availability and quality of tooling. 

Other aspects seen as critical in fostering language adoption are: Error handling, IDE integrations and better documentation. 

Less often voted for but still relevant are the increasing complexity of the language, availability of generics, improvements in compilation time and debugging as well as improvements in gas cost, storage handling and security.

![Solidity's Challenges](/img/2021/01/Challenges.png)

#### Importance Ranking of Future Features

We've asked the participants of the survey to rank selected features under discussion according to their importance.

Gas optimizations were ranked as most important, followed by interfaces to external debugging tools. The SMTChecker, generics and fixed point types were ranked similarly important. 

Explicitly copying semantics, enums with data, functions at file level and WebAssembly output were evaluated as less critical by the participants.

![Future Features Ranking](/img/2021/01/FutureFeatures.png)

### (4) Most Liked, Dreaded and Anticipated Features

We've asked survey participants what they like and dislike most about Solidity and what they would like to see the most in upcoming Solidity versions. Those questions had free-form reply fields so we've summarized the replies and are listing the most mentioned features and aspects below.

#### Most Liked Features

Simplicity and cleanliness has been the number one aspect that the participants value about Solidity. 

Furthermore, the developers appreciated the improvements in available tooling, especially highlighting Remix and Hardhat several times. 

Another aspect the respondents appreciate is that Solidity is "easy to learn" and "simple to code".

Several people stated that Solidity is the right tool for what they are working on and that they like the focus on smart contracts and Solidity being a "special purpose tool" for it.

Other features and aspects that have been mentioned repeatedly as cherished and appreciated are:
- Mappings.
- Human-readability.
- Structs.
- Modifiers.
- Strong-typing.
- JS-likeness.
- Assembly access.
- SafeMath by default.
- Good library support.
- C++-likeness.
- SMTChecker.
- Composability.
- Immutability.
- ABI Encoder V2.
- Explicitness.
- Inheritance.
- Compact code size.
- Syntax.
- Steady improvement of the language.


#### Most Dreaded or Disliked Features

The currently most dreaded aspect of Solidity is clearly debugging, followed by a lack of Solidity libraries. 

Respondents also complain about inheritance conflicts and the need to write more specific code in assembly / low-level code. 

Other aspects and features that have been mentioned repeatedly as dreaded or disliked are:
- Error handling.
- Memory management.
- Frequency of breaking changes and dependencies breaking.
- Contract & stack size limits.
- Inconsistency with other languages.
- ABI encoding.
- Unclear gas usage / gas inefficiency.
- Tooling.
- Complexity.
- Recompiles failing due to metadata.
- Verbosity.
- Dynamic array handling.
- Testing.

#### Most Anticipated Features

The most anticipated feature by far was "no more SafeMath". SafeMath by default has been [introduced with 0.8.0](https://blog.soliditylang.org/2020/12/16/solidity-v0.8.0-release-announcement/) and hence is not an issue anymore. 🥳

The second most anticipated feature was better debugging and more debugging tools. 

Also ranked high was better documentation, especially with a focus on more tutorials and code examples.

Other anticipated and frequently mentioned features were:
- Gas efficiency optimization features.
- Generics.
- console.log().
- Dynamic arrays.
- Better tooling.
- Better IDEs.
- LSP support.
- Rich revert reasons / error codes.
- Standard library.
- Custom optimization features.
- Release v1.0.0.
- SMTChecker.
- Stable ABIEncoderV2.
- No more stack too deep.
- Decimals / fixed point types.
- WASM.

### (5) Community

More than 55% of respondents interact with other Solidity developers on a regular basis, while 27% claim to do so only rarely. Almost 17% do not interact with other Solidity developers at all.

![Developer Interaction](/img/2021/01/DevInteraction.png)

The majority of respondents feels welcome in the Solidity community and is confident in the work of the Soldity core team. 

71% agree or somewhat agree to feeling welcome to contributing to Solidity.

Only 22% fully agree that the Solidity team understands their needs. 14% state that they do not agree and don't feel understood by the Solidity team with regards to their needs.

The process of how to contribute to Solidity is clear or somewhat clear to 41% of respondents while 32% do not agree and hence say that the contribution process is unclear to them. 

⚠️ _Note that there was a comparatively high share of "I don't know" replies to the last three statements._

![Community Statements](/img/2021/01/CommunityStatements.png)

#### Language Design

Almost 85% of respondents state that they have never participated in any Solidity language design related activity. Around two thirds of them say they didn't do so because they don't know how, while one third is simply not interested in it.

7-8% said that they are part of the language design mailing list, have joined one or more language design discussion calls and/or have proposed features or changes to the language by filing GitHub issues.

![Language Design](/img/2021/01/LangDesignParticipation.png)

#### Staying Up-To-Date

57% prefer Twitter to stay up-to-date about things. 21% like mailing lists, 14% prefer Reddit and 6% are in favor of forums.

![Staying Up-To-Date](/img/2021/01/UpToDate.png)

To stay in the loop with Solidity updates like new releases, security alerts and announcements, most respondents said they follow [Solidity on Twitter](https://twitter.com/solidity_lang), follow the [Solidity GitHub release page](https://github.com/ethereum/solidity/releases) and/or the [Solidity blog](https://blog.soliditylang.org/). Also mentioned but less popular are the [r/ethdev Reddit](https://www.reddit.com/r/ethdev/), the [Solidity documentation](https://docs.soliditylang.org/en/latest/) and the [Week in Ethereum newsletter](https://weekinethereumnews.com/).

Some of the participants claiming to not do any of the above explained that they mostly get informed by colleagues or friends or simply don't stay up-to-date at all.

![Staying Up-To-Date 2](/img/2021/01/UpToDateSol.png)

### THANK YOU! 💙

As a wrap up question we asked participants if there was anything else that they’d like to tell us. We did not expect that amount of good vibes and want to say thank you to each of you! Reading your uplifting and warm comments was pure joy.

We will continue our quest to further enhance and shape Solidity and are looking forward to the next survey to check in with you again!

Feel free to reach out to us [via email](mailto:solidity@ethereum.org) or [chat with us](https://gitter.im/ethereum/solidity-dev) should you have any further questions about details of the survey results. 
