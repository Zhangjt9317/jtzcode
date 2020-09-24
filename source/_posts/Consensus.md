---
title: Consensus
date: 2020-09-16 23:10:19
tags:
- Consensus Algorithm
categories: 
- Blockchain
---

# Introduction
**refer to [Binance Academy](https://academy.binance.com/blockchain/delegated-proof-of-stake-explaine])**

A consensus algorithms is a mechanism that allows users or machines to coordinate in a distributed setting. It needs to ensure that all agents in the system can agree on a single source of truth, even if some agents fail. It is a fault-tolearnt mechanism to address the [Byzantine Fault Tolerance](https://academy.binance.com/blockchain/byzantine-fault-tolerance-explained)


Using the powerful consensus algorithm can efficiently protect users' assets. For example, using the Proof-of-Stake (PoS) or Delegated Proof-of-Stake (DPoS) as consensus algorithm can make render Sybil attacks effortless by simply making it impractical to succeed since attackers need to pay more than it gets financially.

# Types of consensus algorithms 

## Proof of Work (PoW)

Proof of Work was proposed by Satoshi Nakamoto, the creator of Bitcoin to coordinate participants. 

In PoW, validators (also called as miners) hash the data they want to add until they produce a specific solution for that puzzle. 

A hash is a random string of letters (the length depens on your selection of hashing algorithm) that's created when you pass your data through a hash function. No matter how many times you pass, it always end up with the same output, and changes even one number, your hash will be totally different.

In PoW, the protocol sets conditions for what solves the puzzle, for example, only a block whose hash begins with 00 will be valid. Thus the only way for miners to create one that matches all conditions is to brute-force inputs by continuously tweeking parameters until they get the right hash.

This is a very computation-intensive work to do, and it costs a lot of resources, especially for major blockchains, the bar is set very high, and the amount of resoruces needed is unparalled. 


## Proof of Stack (PoS)
PoS was proposed in the early days of Bitcoin as an alternative of PoW. It does not have the concept of miners, specialized harware for mining, or massive electricity consumption. 

In PoS, you need to put forward your cryptocurrencies 


## Delegated Proof of Stack (DPoS)
Both PoS and DPoS are alternatives to the computation-intensive PoW. 

A DPoS-based blockchain counts with a voting system where stakeholders entrust others to represent on their behalf. The delegates may also be referred to as witnesses and they are responsible for acheiving consensus during the generation and validation of new blocks. The voting power depends on the number of coins each user holds. The voting system differs from project to project, and usually each delegate only represent one individual proposal when asking for votes. The reward will be collected by delegates and shared with their electors in proportions.

This voting system is directly dependent to delegates' reputation. If an elected node misbehaves or mulfunctions, it will be removed and replaced by another one. 

DPoS blockchains are more scalable and powerful in processing transactions compared to both PoW and PoS.


# Other consensus algorithms 

* Delayed Proof of Work
* Leased Proof of Stack Concensus
* Proof of Authority
* Proof of Burn 
* Hybrid PoW/PoS Consensus

# Closing thoughts