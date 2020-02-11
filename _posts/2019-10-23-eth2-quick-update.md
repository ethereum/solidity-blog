---
layout: post
published: true
title: 'eth2 quick update'
date: '2019-10-23'
author: Danny Ryan
category: 'Research & Development'
---

Although the internet has been more quiet than usual, we've been super busy hacking away on eth2! Between Devcon5 and keeping our heads down to work, it seems we've left the community in the dark on a couple of items. Here's a quick update to fill in the gaps.

## Deposit Contract

Although the deposit contract has been [written](https://github.com/ethereum/eth2.0-specs/blob/dev/deposit_contract/contracts/validator_registration.v.py), [tested](https://github.com/ethereum/eth2.0-specs/blob/dev/deposit_contract/tests/contracts/test_deposit.py), and [formally verified](https://github.com/runtimeverification/verified-smart-contracts/tree/deposit/deposit), we are working to allow the BLS standardization to stablize prior to launch. One goal of eth2 is to be easily interoperable with other blockchains and systems in general, and to that end, we do not want our BLS signatures to go the way of keccak (whoops!).

The BLS Standard ([BLS Signature](https://tools.ietf.org/html/draft-irtf-cfrg-bls-signature-00), [Hash to Curve](https://tools.ietf.org/html/draft-irtf-cfrg-hash-to-curve-04)) has reached a point of stability recently with a number of blockchain teams on board (Eth2, Chia, Filecoin, Algorand, etc). There is an IETF meeting in November at which we expect the draft to be even more cemented. That said, official standards can take quite a while so those involved plan to signal public support for the draft and have a "blockchain agreement" to use the standard as drafted regardless of its final form in IETF. That way, if it becomes the keccak of signatures, we won't be there alone. :)

Fortunately, the deposit contract does not need to be put into production until we near Phase 0 launch, so this focus on standardization is not expected to have any effect on the Phase 0 launch date.

## Eth2 Testnets

If you follow ethresearch, the specs repo, or any of the many workshops at Devcon, we have [altered the sharding proposal](https://notes.ethereum.org/KbEyHiaSRQW_KS7dDK0OFw) in such a way to greatly improve developer and user experience -- cross-shard communication between all shards at every slot. To facilitate this improved design, we have to modify the Phase 0 spec a bit. To do this with limited disruption to Phase 0 development and testnets, we've gone the simplifying route -- the removal of crosslinks entirely from Phase 0 (they were stubbed anyway). This change is coded and under final review [here](https://github.com/ethereum/eth2.0-specs/pull/1428) and is expected to be released for development within the week.

We expect multi-client public testnets to launch soon after this simplifying change is completed, for this update to aid Phase 0’s progress to mainnet, and ultimately to make Phases 1 and 2 easier to ship.

Eth2 testnets are coming! Individual clients are in the process of spinning on some nets for both private and public consumption. Many clients are just getting their `eth1-to-eth2` machinery in place so these single-client testnets are useful in initially testing that component. On these nets there will be some limited cross-client testing, but will be largely stable due to having a majority single-client.

Once clients adequately test larger single-client nets and once they have time to incorporate the Phase 0 changes, we will be full speed ahead on public multi-client nets. We're just as excited about this as you are and will be publishing more info on participation (_staking your eth_) in both testnets and mainnet shortly. Casper is indeed coming.
