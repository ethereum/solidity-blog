---
layout: post
published: true
title: 'Announcing Ethereum Foundation and Co-Funded Grants'
date: '2019-08-26'
author: Ethereum Team
---

We are today unveiling over $2M USD in Foundation-led and co-funded grant funding aimed at furthering Serenity (Eth2.0) development as we move nearer to the launch of the Beacon Chain.

## Eth2.0 Client Grants

In recent months, the Ethereum Foundation granted substantial awards to leading client development teams currently tasked with delivering long-standing multi-client testnets as development approaches the launch of Phase 0.

We are proud to announce the recipients of those grants.

### Harmony

_Award: $189k for Harmony beacon chain development_

[Github](https://github.com/harmony-dev/beacon-chain-java)

### Prysmatic Labs

_Award: $725k for Prysm client development_

[Website](https://prysmaticlabs.com/) | [Github](https://github.com/prysmaticlabs/prysm)

### Sigma Prime

_Award: $485k for Lighthouse client development_

\*Co-funded by: Ethereum Foundation & ConsenSys

[Website](http://sigmaprime.io/) | [Github](https://github.com/sigp/lighthouse/)

### Status

_Award: $500k for Nimbus client development_

[Website](https://status.im/) | [Github](https://github.com/status-im/nimbus)

## Networking

### Status

_Award: $150K for nim-libp2p development_

\*Co-funded by: Ethereum Foundation & Protocol Labs 

[Website](https://status.im/) | [Github](https://github.com/status-im/nim-libp2p)

Status' Nimbus team is building a native nim-language implementation of libp2p for use in Nimbus and other projects. Further, the team aims to create an implementation suitable for resource-restricted devices.


### Whiteblock

_Award: $184K_

\*Co-funded by: Ethereum Foundation & ConsenSys

[Website](https://whiteblock.io/)

Whiteblock is funded to continue Eth2.0 network testing and testnet/interoperability support. Whiteblock will analyze the libp2p gossipsub implementation, continue to work with implementors to help refine the networking stack and specification, and advance interoperability efforts. 

## Research and Development

### Dmitry Khovratovich 

_Award: $10K_

Dmitry Khovratovich, an expert cryptanalyst, is funded to produce a report regarding the security of the Legendre PRF (as described by Grassi et al.) and the proof-of-custody scheme intended to be used in Phase 1 of Eth2.0.

### Chainsafe

_Award: $217.5K for Lodestar light-client and javascript development_

[Website](https://chainsafe.io/) | [Github](https://github.com/ChainSafe/lodestar)

Chainsafe is working on Eth2.0 light-client R&D within their Lodestar javascript client along with continuing to provide support to the Eth2.0 JS/web-browser ecosystem by developing libraries, tooling, and educational resources.

## Bounties

The following bounties are open to help further research and development efforts for the future of Ethereum.

### Phase 0 Consensus

The Ethereum Foundation is offering 5 ETH bounties (or 1000 DAI, whichever larger) for any recommendations for substantive changes (bug fixes, optimizations, etc) that make it into the Phase 0 Eth 2.0 spec prior to the Eth 2.0 genesis. These include changes for the state transition function, fork choice rule, or deposit contract. For more information, check out [the open bounty issue on the specs repo](https://github.com/ethereum/eth2.0-specs/issues/1345).

### Legendre PRF

The Legendre pseudo-random function is an extremely MPC-friendly one bit PRF. This PRF is currently slated to be used in the proof of custody scheme in Phase 1 of Eth 2.0, as it is the only known method to make the proof of custody possible in a shared secret setting. To encourage more research about this PRF, the Ethereum Foundation has set out a series of bounties here: legendreprf.org/bounties

### STARK-Friendly Hash Challenge

The Ethereum Foundation has asked StarkWare to recommend a STARK-Friendly Hash (SFH). The StarkWare hash challenge is a public competition aimed at evaluating the security of current proposed SFH candidates. The challenge is proposed at four security levels: low-security, medium-security, target-security, and high-security in multiple scenarios. See https://starkware.co/hash-challenge/ for more details about the competition and how to get started.
