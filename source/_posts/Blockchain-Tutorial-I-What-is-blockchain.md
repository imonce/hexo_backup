---
title: 'Blockchain Tutorial I: What is blockchain'
date: 2020-01-06 21:47:36
tags: [Blockchain, Bitcoin]
categories: [Learn X in Y minutes, Learn blockchain in Y minutes]
---

# Brief History

- May/2007, Nakamoto Satoshi started the Bitcoin project
- August/2008, Nakamoto Satoshi registered domain name bitcoin.org
- 31/Obtober/2008, Nakamoto Satoshi sent an email to all members of a cryptography mailing list entitled "bitcoin: peer-to-peer e-cash thesis."
- 16/November/2008, Nakamoto Satoshi announced the source code of bitcoin system
- 3/January/2009, Nakamoto Satoshi launched Bitcoin network on the Internet
- 22/May/2010, Bitcoin pizza Festival, one programmer traded 10,000 bitcoins for two great John pizza coupons. For the first time, bitcoin had a fair price: 10000 bitcoins cost $25
- November/2011, Nakamoto Satoshi disappeared

# Why Bitcoin

1. Disintermediation: E-cash between individuals with no intervention of a trusted third-party intermediary
2. Decentralization: This e-cash currency issuance does not need a centralized institution, but is completed by the code and community consensus

# Why dose Bit coin need Block Chain

In the digital world, if we want to create a disintermediated and decentralized "e-cash", we also need to design a complete financial system. 

This system should be able to solve a series of problems as follows:

- How can this "cash" be issued fairly and impartially without being controlled by any centralized institution or individual?
- How to realize that just like in the physical world, one person can hand the cash directly to another person without any intermediary assistance?
- How to "prevent counterfeiting" this kind of e-cash? Or how can an e-cash not be spent twice?

To solve the problems, Nakamoto developed Bitcoin system, which consists of 3 layers:

1. Application layer. The top layer is bitcoin. This is the application layer of the whole system.
2. Application protocol layer. The function of the middle layer is to issue bitcoin and handle the bitcoin transfer between users. This layer, also known as bitcoin protocol, is the application protocol layer of the whole system.
   1. Application layer. Transfer and bookkeeping functions
   2. Incentive layer. Issuance mechanism and distribution mechanism
   3. Consensus layer. POW(Proof Of Work)
3. General protocol layer. At the bottom are bitcoin's distributed ledgers and decentralized networks. This layer, also known as bitcoin blockchain, is the general protocol layer of the whole system.
   1. Network layer. P2P mechanism, broadcast mechanism, and verification mechanism
   2. Data layer. Block data(Hash), chain structure(Merkle Tree), and digital signature(Asymmetric encryption)

In the design of bitcoin system, Nakamoto creatively combines computer computing power competition with economic incentives to form a proof of work (POW) consensus mechanism, which enables mining computer nodes to complete the function of currency issuance and accounting in the calculation competition, as well as the operation and maintenance of blockchain ledger and decentralized network. 

This forms a complete cycle: the mining machine mining (calculation power competition), the completion of decentralized accounting (operation system), and the economic incentive (economic reward) in the form of bitcoin.

Bitcoin's workload proof consensus mechanism is a connecting layer, connecting the upper application and the lower technology: the upper layer is the issuance, transfer and anti-counterfeiting of e-cash; the lower layer is the node to the central network to reach an agreement and update the distributed ledger.

# Definition of Blockchain

Blockchain is the technology of "value representation" and "value transfer" in the digital world. One side of blockchain coin is the encrypted digital currency or token representing value, and the other side is the distributed ledger and decentralized network for value transfer.

Blockchain is an underlying technology derived from bitcoin. In other words, bitcoin is the first successful application of blockchain technology.

When people talk about Blockchain, what do they mean:

1. Blockchain refers to the data structure of bitcoin, that is, the chain formed by the connection of data blocks, which is also known as "distributed ledger". In the bitcoin white paper, Nakamoto mentioned block and chain respectively, but later they were combined into the new term block chain.
2. Blockchain refers to the combination of bitcoin's distributed ledger and decentralized network. Corresponding to bitcoin system, it refers to the whole third layer of bitcoin blockchain.
3. Blockchain refers to the combination of the second layer (bitcoin protocol) and the third layer (bitcoin blockchain) of bitcoin system. It includes distributed ledgers, decentralized networks and bitcoin protocols.
4. Blockchain refers to the whole bitcoin system, including all three layers, including bitcoin with value representation and the whole system behind it. From this perspective, blockchain is regarded as a complete system including both technical and economic parts.

When referring to blockchain, ordinary people often refers to the fourth largest scope, namely "account book + Network + protocol + currency". In the industry, when people refer to blockchain, they usually refer to the third scope, namely "account book + Network + Protocol". When talking about blockchain, many software developers usually refer to the second range of "ledger + network".

> reference:
> http://c.biancheng.net/view/1884.html