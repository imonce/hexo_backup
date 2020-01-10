---
title: 'Blockchain Tutorial II: How does blockchain decentralize bitcoin'
date: 2020-01-07 20:29:57
tags: [Blockchain, Bitcoin]
categories: [Learn X in Y minutes, Learn blockchain in Y minutes]
---

# What is decentralization

Here we illustrate it by means of comparison. 

There have always been three forms of "currency" in the digital world:

- Centralized online payment: Paypal, Alipay, Apple Pay...
- Centralized computer points or Internet points: Game coins...
- Decentralized e-cash: Bitcoin, ETH...

Comparison of three forms and cash in the physical world:

||Cash in physical world|Centralized e-cash|Centralized computer points|Decentralized e-cash|
|:--|:--|:--|:--|:--|
|Issuance|Centralized|Centralized|Centralized|Decentralized|
|Transaction|Decentralized|Centralized|Centralized|Decentralized|

The relationship between three forms and cash in the physical world:

||Cash in physical world|Centralized e-cash|Centralized computer points|Decentralized e-cash|
|:--|:--|:--|:--|:--|
|Map physical currency?|/|Yes|No|No|
|Self-issue?|/|No|Yes|Yes|

# What is the decentralization of Bitcoin

From the shallower to the deeper, it has the following aspects:

1. Decentralization of transactions (Automatic)
2. Decentralization of transactions (Autonomous)
3. Decentralization of issuance (Automatic)
4. Decentralization of issuance (Autonomous)
5. Partial decentralization of network (Distributed network)
6. Decentralization of network (Fully open, non-trust-based)
7. Coordinated community (Coordinated and managed by people)
8. Completely decentralized community (Autonomous achieved by mechanism)

Later, in the process of developing and applying blockchain technology, we have to adjust from the most extreme ideal state to the practical direction.

Most blockchain projects are now managed by foundations. For example, Ethereum is co-ordinated between founder Vitalik Butlin and the Ethereum foundation, rather than being fully autonomous as the bitcoin community.

# How does blockchain decentralize bitcoin

Main design principles of blockchain system:

- A true point-to-point e-cash should allow direct online payments from the originator to the other party without the need to go through a third party financial institution.
- Although the existing digital signature technology provides some solutions, if a trusted third-party organization is needed to prevent "double payment", the main benefits (Brought by e-cash) will be lost.
- To solve the problem of "double payment" in e-cash, we provide a solution with point-to-point network technology.
- The network stamps the transaction records, hashes the transaction records, and merges them into a growing chain, which is composed of hash based proof of work. If we don't redo the proof of work, the records can't be changed.
- The longest chain is not just proof of the sequence of events observed, but also proof that it is generated by the largest pool of CPU processing power. As long as the computer nodes that control most CPU processing power don't attack the network itself (with the attacker), they will generate the longest chain, leaving the attacker behind.
- The network itself needs only the simplest structure. The information can be broadcast in the whole network as much as possible. The node can leave and rejoin the network at any time, only need (when rejoining) take the longest workload proof chain as the proof of the transaction occurred during the offline period of the node.

Four key features by William Mougayar:

1. Point to point electronic transactions;
2. No need for financial institutions;
3. Encrypting evidence rather than centralized credit;
4. Credit exists in the network, not in a central institution.

# Five key points of bitcoin system design:

## Decentralized point-to-point e-cash system

What bitcoin needs to do is a "point-to-point e-cash system", in which the sender and the receiver deal directly without the intervention of intermediaries.

In order to remove the trusted third party and other intermediaries, we need to solve the "double blossom problem". In the summary, Nakamoto presents a point-to-point network solution, and introduces the core of the solution - blockchain. He didn't mention the word block chain, but in the paper he mentioned the two concepts of block and chain respectively.

## Distributed ledger

The blockchain of bitcoin is a data block with time stamp and data storage and a chain connected by hash pointer based on workload proof.

This chain, or ledger, is stored on nodes of bitcoin network in a distributed way, so it is also called distributed ledger.

## Proof of workload

Nodes in bitcoin network perform encryption hash calculation according to rules to compete for the right to generate new blocks. After the node wins the competition, it gets the bookkeeping right. When it generates a block and becomes the latest block, it gets the mining reward corresponding to the new block.

Workload proof is also the security mechanism of blockchain account book. This chain cannot be modified without redoing the large amount of calculation required by "proof of workload", which ensures the reliability of the data on the blockchain.

## Longest chain principle

At any moment, the longest chain is the final record accepted by all.

Since the longest chain is completed by the main computing power in the network, as long as they do not cooperate with attackers, the longest chain they generate is reliable. This principle is called the "longest chain principle".

## Decentralized network

Bitcoin's decentralized network architecture is very simple and requires very little infrastructure. It can run on the Internet network. Computer nodes can leave or join the decentralized network at any time. When they join, they only need to follow the longest chain principle.

> reference:
> http://c.biancheng.net/view/1889.html