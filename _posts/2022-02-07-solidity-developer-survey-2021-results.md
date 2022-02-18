---
layout: post
published: true
title: 'Solidity Developer Survey 2021 Results'
date: '2022-02-07'
author: Franziska Heintel
category: Announcements
---

In this post, we will be summarizing and analyzing the results of the [2021 Solidity Developer Survey](https://blog.soliditylang.org/2021/11/18/solidity-developer-survey-2021/).

A big thank you goes out to everybody who took the time and participated!

Your input is invaluable to us and plays a crucial role in helping to continuously improve the Solidity developer experience as a whole.

## Summary & Notable Insights

- **Survey Audience**: In total, 435 developers from 73 different countries participated in the 2021 survey. Compared to 2020, that is more than a 100% increase in responses. The coverage of different geographies also drastically increased from 48 countries in 2020 to 73 countries in 2021. More than 20% stated to be residing in the US, followed by India (9%) and Germany (4%). Roughly 9% preferred to not share details on their location.
- **Developer Profiles**: The level of coding experience is medium to high with the majority of respondents having coded professionally 3 or more years, 36.6% even more than 6 years.
- **Solidity Experience**: More than half of all respondents have been using Solidity less than a year, while 15.5% have been using it more than 3 years. Almost 80% use Solidity daily or weekly.
- **Solidity Expertise**: Most respondents deem themselves Solidity experts, with a self rating in expertise of 7 or higher (scale of 10). 4.2% rate their expertise as a 10/10. 80% of respondents use Solidity for their personal projects, roughly 60% also use it at work. More than half of all respondents have been using Solidity for less than a year.
- **Developer Experience**: The majority (+70%) believes that the Solidity developer experience improved in the last year. Only 1.6% are of the opinion it got worse.
- **Language Restrictiveness**: More than 60% of respondents wish that Solidity becomes more restrictive/explicit (having more checks). 26% would prefer it to remain as is.
- **Future Features**: A more efficient optimizer and the ability to catch custom errors are ranked as most important future features under discussion. Moreover, support for fractional numbers, better array management and fixing stack too deep are among the most mentioned anticipated features.
- **Liked & Dreaded Features**: Respondents most like Solidity's simplicity, the "easy to learn" aspect of it, SafeMath by default and modifiers. Dreaded topics are debugging, the stack too deep error and missing support for fractional numbers.
- **Community**: Less than a third of respondents ever participated in Solidity language design related efforts.

## [1] Survey Audience

_⚠️ Note that the fact that this survey has only been shared in English is an important factor to consider when interpreting results regarding the distribution of countries of residency and language preferences._

To begin with, let us look at the developers who participated in this survey: We will elaborate on general information like location and spoken languages and also learn more about their professional experience, coding preferences and more.

In total, the 2021 survey received **435 responses from developers from 73 different countries**. Compared to 2020, that is more than a 100% increase in responses. It appears this survey also reached a geographically much more diverse audience since the number of countries climbed from 48 in 2020 to 73 in 2021.

![Survey Participants World Map](/img/2022/02/geo.png)

#### Residency 

More than 20% stated to be residing in the US, followed by India (9%) and Germany (4%). Roughly 9% preferred to not share details on their location.

![Survey Participants Country List](/img/2022/02/countries.png)

#### Language

The respondents cover a wide variety of languages with their native languages. In total, 50 different languages were mentioned as native language.

35% consider English their native language, followed by Spanish (9.4%), French (5.9%), Russian (5.9%), Portuguese (4.9%) and German (4.9%).

![Survey Participants Native Language](/img/2022/02/native_lang.png)

The big majority of more than 80% predominantly speaks English at work. 

Several respondents also speak Spanish (2.8%), French (2.1%), Russian (1.6%) or Portuguese (1.6%) at work.

![Survey Participants Work Language](/img/2022/02/work_lang.png)

Of the respondents who didn't name English as their native language, more than 90% are okay with reading the Solidity documentation in English. 8.6% would prefer to read it in their native language, the most mentioned ones being Mandarin and Traditional Chinese, Spanish and Portuguese. 

![Preferred Documentation Lanugage](/img/2022/02/docs_lang.png)

_ℹ️ Bear in mind that this survey has only been conducted in English, which may have an impact on the outcome of this question. We still believe internationalization of resources like the Solidity documentation is a crucial factor to lower the barriers of entry and we aim to support community efforts to translate with a new, clearer structured [translation guide](https://github.com/solidity-docs/translation-guide)._

### Developer Profile

#### Work Experience & Employment

77.1% of respondents are currently employed, while roughly 10% are students and 12.5% are currently not working professionally.

![Employment Status](/img/2022/02/employment.png)

The respondents predominantly work in the technology (62.8%) and financial services (15.5%) sector. The last third splits into various other sectors.

![Industry Sector](/img/2022/02/sector.png)

Roughly 10% are coding newbies and have only coded professionally for less than a year. On the other hand, a similar number of respondents has coded for more than 15 years.

With approximately 30%, the biggest group sits in the middle of the distribution and has professional coding experience of 3-5 years.

Overall, the level of coding experience is medium to high with the majority of respondents having coded professionally 3 or more years, 36.6% even more than 6 years.

![Professional Coding Experience](/img/2022/02/coding_experience.png)

Interestingly, the majority of respondents (80.4%) use Solidity for their personal projects. Roughly 60% of all respondents use Solidity at work, while 40% predominantly code in another programming language at their job.

More than 20% state they are leading a programming team.

![Survey Participants Relation to Solidity](/img/2022/02/role_sol.png)

In terms of open-source contributions, unfortunately 60% of respondents say that they never, or only infrequently contribute to open-source projects written in Solidity.

On the other hand, almost 30% do so on a daily or weekly basis.

![Survey Participants Open Source Contributions](/img/2022/02/open_source.png)

#### Programming Language Preferences

JavaScript and Solidity share rank 1 in terms of most used programming languages (both 27%), followed by TypeScript (14.7%) and Python (10.9%). 

![Most Used Programming Language](/img/2022/02/progr_lang.png)

Respondents' favorite programming languages are distributed more evenly across various languages. Python is most popular, scoring 22.7% of all entries, followed by Solidity (19.4%), JavaScript (14.5%), TypeScript (10.8%), and Rust (8.4%).

![Favorite Programming Language](/img/2022/02/fav_progr_lang.png)

#### Operating System

Similar to the 2020 survey, macOS and Linux seem equally popular. Roughly 40% use macOS, closely followed by Linux (36.6%). Windows is used by 22% of respondents, most of who state to be using either macOS or Linux in addition to Windows. There are also several developers using both Linux and macOS, or even all three operating systems.

![Operating System](/img/2022/02/os.png)

### Solidity Experience & Solidity Developer Profile

Most respondents deem themselves Solidity experts, with a self rating in expertise of 7 or higher (scale of 10). 4.2% rate their expertise as a 10/10. Roughly 23% can be considered beginners or low-frequency users with a self-rated expertise level of 4 or lower.

![Solidity Expertise Level](/img/2022/02/sol_expertise.png)

The share of beginners has increased slightly compared to last year with more than half of all respondents using Solidity for less than a year.

15.5% have been using Solidity for more than 3 years and can thus be considered Solidity seniors.

![Solidity Experience Level](/img/2022/02/sol_experience.png)

Solidity remains to appear rather easy to learn with 26.7% of respondents feeling productive in less than a month and 30.7% in less than half a year. 7.7% needed more than a year to feel comfortable with the language. 23.3% don't feel productive yet, out of which more than 75% are beginners and have been using Solidity for 6 months or less.

![Time to Productiveness](/img/2022/02/sol_productivity.png)


#### Solidity Developer Profile

The majority (roughly 80%) of respondents use Solidity on a daily or weekly basis. 8% indicated to be using Solidity "rarely" or "never", out of which almost all predominantly code in another programming language at work and most indicated they only have been using Solidity for less than 3 months.

![Solidity Usage Frequency](/img/2022/02/sol_frequency.png)

More than 50% use VSCode as an editor to write Solidity, followed by Visual Studio (14%) and Remix (11%). Vim is used by 7% of respondents, followed by IntelliJ (5.8%) and Atom (3%).

Compared to 2020, IntelliJ, Atom, Vim and Sublime are used less in 2021.

![Editor Overview](/img/2022/02/ide.png)

Hardhat becomes the most popular Ethereum-specific development environment with ~65% of all respondents using it (from 38.6% in 2020).

Truffle and Remix follow with a user share of roughly 25-26% each. Rather "niche" Ethereum-specific development environments are Brownie (10.5%), Dapptools (8.2%), Scaffold-ETH (4.7%), Foundry/Forge (1.6%) and Embark (0.7%). 3.5% of respondents are not using any Ethereum-specific IDE.

Compared to 2020, Truffle (2020: 59.3% -> 2021: 26.2%) and Remix (2020: 50.3% -> 2021: 24.8%) lost significant share, while Hardhat, Brownie, Dapptools and newcomers like Foundry increased their share in users. It's important to note that the 2020 survey had significantly fewer responses (2020: 194 -> 2021: 435), hence the year-on-year comparison can only be interpreted as a loose trend but doesn't have proper statistical relevance due to the small lot size.

![Ethereum IDE Overview](/img/2022/02/eth_ide.png)

With 86.3%, 0.8.x Solidity versions are by far the most used ones. Both 0.7.x (23%) and 0.6.x (18.3%) version series remain to be used while everything older than that is hardly in use anymore.

This is a great development compared to 2020, when the majority of users was still on the 0.6.x version series.

Luckily, only a few people are still using very old versions from the 0.4.x or 0.5.x series.

![Used Solidity Versions](/img/2022/02/versions.png)

*⚠️ Reminder: Please make sure to frequently update your code. [Several important bug fixes and security improvements](https://github.com/ethereum/solidity/blob/develop/docs/bugs_by_version.json#L922) have been added since 0.4.x!*

## [2] Solidity User Experience

The majority (+70%) believes that the Solidity developer experience improved throughout the last year. Only 1.6% are of the opinion it got worse.

![Solidity Developer Experience](/img/2022/02/dev_ex.png)

When getting stuck on a Solidity problem, 80% try to find solutions in the Ethereum StackExchange or on StackOverflow. Many also ask their coworkers for help (32.9%) or watch tutorials (38.1%). It's also quite popular to leave the problem for a bit, do other work and try solving it later again (29.2%).

![Solving Problems](/img/2022/02/solidity_problem.png)

### Recurring Issues

30% of respondents don't encounter the same or similar issues multiple times when developing in Solidity.

Amongst the people that do, stack too deep, bytecode size limit, debugging issues, uncertainties about the optimizer and array handling were mentioned most often as recurring issues.

## [3] Features

### Future Features

A more efficient optimizer and the ability to catch custom errors are ranked as most important future features under discussion.

![Future Feature Priority Ranking](/img/2022/02/future_features_rank.png)

Moreover, support for fractional numbers, better array management and fixing stack too deep are among the most mentioned anticipated features.

_⚠️ We noted that respondents were using various different terms like "floats", "floating point arithmetic", "floating point number", "fixed point numbers", "fixed point math". We categorized those as "factional numbers" and assume that all of the above ultimatively aims to describe "fixed point math"._

Most mentioned anticipated features in descending order:

* "floats"
* better array management / more functions for array & mappings
* fix stack too deep
* gas optimizations / optimizer improvements
* better debugging
* better support for strings
* easier/better gas metering while building/developing
* console.log()
* custom errors for require()
* generics
* better docs (esp. for advanced content like inline assembly, Yul, etc.)
* codegen via Yul
* custom value types
* fixed point math
* LSP

### Most Liked & Dreaded

Respondents most like Solidity's simplicity, the "easy to learn" aspect of it, SafeMath by default and modifiers.

Most mentioned liked features in descending order:

* simplicity
* easy to learn
* domain-specific language / right tool for the job / "it works"
* SafeMath by default / over- & underflow checks
* modifiers
* mappings
* clean syntax
* interfaces
* static typing
* readability
* inheritance
* good tooling
* structs
* inline assembly
* delegate call
* require and assertions
* custom errors
* memory management
* events
* libraries
* compiler safety
* ABIEncoderV2
* explicitness
* flexibility
* immutability
* language safety
* object orientation

Most dreaded topics are debugging, the stack too deep error and missing support for fractional numbers.

Dreaded features in descending order:

* debugging
* stack too deep
* missing floating numbers / fixed point numbers
* inline assembly
* ambiguous/generic (revert) error messages
* arrays
* strings
* docs difficult to read and navigate
* breaking changes in minor versions / lack of compatibility
* security
* gas cost / deploy cost
* inheritance
* testing
* gas optimization
* modifiers
* outdated resources / tutorials in community resources
* increasing complexity
* `returns`
* explicit conversions
* missing docs on inline assembly / yul
* missing console.log
* memory allocation
* missing standard library
* reentrancy
* type system
* override

### Restrictiveness

More than 60% of respondents wish that Solidity becomes more restrictive/explicit, having more checks. 26% would like it to remain as is.

![Solidity Restrictiveness Ranking](/img/2022/02/restrictiveness.png)


## [4] Solidity Community

### Language Design

Less than 20% of respondents ever participated in Solidity language design related efforts. 6.2% participated in discussions in the Solidity forum, 5.1% joined language design calls and 6.4% opened or contributed to Github issues in the Solidity repository.

Of the roughly 80% that did not participate in language design, almost 8% state not being interested, while 35% are too busy with their work and 40% don't know how they could get involved.

![Language Design Participation](/img/2022/02/lang_design.png)


### Staying Informed

Most people like to stay up-to-date about Solidity versions, security alerts and announcements by following Solidity on Twitter or Mastodon. Other often used means for information are the Solidity blog and Solidity GitHub release page.

![Means To Stay Up-To-Date](/img/2022/02/stay_informed.png)

### Interaction with Other Solidity Developers

More than half of respondents interact with other Solidity developers. Interestingly, still almost 45% state that they are only rarely or never in touch with other Solidity developers.

![Developer Interaction](/img/2022/02/interaction.png)

As the last part of the survey, we wanted to hear how many participants agree or disagree with several statements regarding the Solidity community and the work of the Solidity team.

* 75% of respondents feel welcome in the Solidity developer community.
* Roughly 80% agree or somewhat agree that they feel confident in the work of the Solidity team.
* More than half feel welcome to contribute to Solidity, however only less than half say that they know how to contribute ideas or feedback to Solidity.
* Roughly 25% are confident that the Solidity team understands their needs as a developer. Another 40% somewhat agree, while only a fraction disagree or strongly disagree.

![Community and Solidity Team Confidence Ranking](/img/2022/02/statements.png)

## Thank You & See You Next Year!

Lastly, we want to take the opportunity to thank you for all your lovely and motivating messages and the feedback received.

We hope the insights from the 2021 Solidity Developer Survey were useful for you, they certainly are for us!

We will continue to collect feedback on an ongoing basis, so please don't stop to hold your eyes and ears open!

To not miss anything make sure to...
- follow Solidity on [Twitter](https://twitter.com/solidity_lang) or [Mastodon](https://fosstodon.org/@solidity).
- join the discussions in the [Solidity forum](https://forum.soliditylang.org/).
- follow announcements on the [Solidity blog](https://blog.soliditylang.org/).

--- 
*All graphs can be found [here](https://docs.google.com/presentation/d/1B4ffyq4gJT3zxNbYmAToT4ognCkmbUjVCf8sye_yNEA/edit?usp=sharing). The raw and analyzed data can be found [here](https://docs.google.com/spreadsheets/d/1PFCv7TwP8hp3AiJHYqy3qk4FTNatJDfUKjtsZnI5SUM/edit?usp=sharing).*

