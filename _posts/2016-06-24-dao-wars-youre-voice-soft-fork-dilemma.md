---
id: 2773
title: 'DAO Wars: Your voice on the soft-fork dilemma'
date: 2016-06-24T17:46:38+00:00
author: Péter Szilágyi
layout: post
guid: https://blog.ethereum.org/?p=2773
permalink: /2016/06/24/dao-wars-youre-voice-soft-fork-dilemma/
dsq_thread_id:
  - "4935875482"
---
The last week was quite hectic for all of us in the Ethereum ecosystem. The DAO has shown us that it takes much more effort to write smart contracts than we originally anticipated; but also that it takes a surprising amount of debate to reach a consensus on issues of this scale.

Everybody in our community was very vocal and forthcoming about how the problem should be fixed in his/her opinion, or whether there's even a problem to fix in the first place. While many have suggested an immediate hard-fork, the implications of such action are yet to be fully understood. An alternative suggestion was the creation of a soft-fork allowing miners to temporarily put certain transactions on hold, attempting to recover the funds without any invasive action on the Ethereum protocol itself.

As there is no clear, best course of action that will satisfy all community members equally, we've decided to give the power to the people running Ethereum to decide whether they support this decision or not.

---

To this end, we've released [version 1.4.8 of Geth](https://github.com/ethereum/go-ethereum/releases/tag/v1.4.8) (codename "DAO Wars") as a small patch release to give the community a voice to decide whether to temporarily freeze TheDAOs v1.0 from releasing funds or not. If the community decides to freeze the funds, only a few whitelisted accounts can retrieve the blocked funds and return them to previous owners. A similar mechanism is provided by [version 1.2.0 of Parity](https://github.com/ethcore/parity/releases/tag/v1.2.0) too.

*Note: If the soft-fork passes, it will block all DAOs from releasing funds, not just the ones the community considers attacked. This is understandably undesired for all legitimately split DAOs. As such – if the community votes to enact the soft-fork – we propose a follow up patch to the soft-fork that will whitelist all DAOs split according to the intent upheld by the enacted soft-fork.*

### How to use this release?

Miners supporting the DAO soft-fork can do so by starting [Geth 1.4.8](https://github.com/ethereum/go-ethereum/releases/tag/v1.4.8) with `--dao-soft-fork`. This will cause the block gas limits to be lowered towards Pi million until the deciding block `1800000` (approx. 6 days from now) is reached. If the gas limit of this block is below or equal 4M, the soft-fork goes into effect and (all updating) miners will start blocking DAO transactions that release funds.

Miners not supporting the DAO soft-fork can run Geth normally without any extra arguments needed. They will try to keep the block gas limits at the current 4.7 million. If the gas limit of the decisive block will be above 4M, the soft-fork is denied and (all updating) miners will accept DAO transactions that release funds.

*Note: All updating clients will agree upon the outcome of the vote and will adhere to that decision. If the soft-fork vote passes, miners voting against it will start blocking transactions too; whereas if the soft-fork is denied, miners voting for it will also accept all transactions.*

### What if I don't update?

Miners who do not update by definition vote against the soft-fork as they will continue the current logic of keeping the gas limit above the vote threshold. If the soft-fork is accepted by the majority, non-updating miners will still accept blocked transactions. In that case, non-updating miners will either fork off their own Ethereum network, diverging from the majority, or will forfeit any blocks they mined (since it's not accepted by the majority, overruling the minority blocks).

### Should non-miners (nodes, wallets, mist, etc) update?

From the perspective of non-miners, this update has little relevance. Either outcome of the vote is equally valid from a plain node's perspective, so plain nodes will accept the heavier chain miners decide on without having to know anything about the soft-fork mechanism or results.

### Epilogue

[This release](https://github.com/ethereum/go-ethereum/releases/tag/v1.4.8) implements a **soft-fork**. A soft-fork is perfectly compatible with all protocol rules and requires only the consensus of the majority of miners to enact. It is temporary and can be removed/amended at any point in time upon miner consensus. It does not break protocol rules; it does not roll back any executed transactions/blocks; and it does change not any blockchain state outside of the original protocol capabilities.

*Note: This release does not represent a consent to hard-fork the network. It is a means to give people more time to come up with the best solution.*