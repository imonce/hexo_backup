---
title: 'Blockchain Tutorial III: How bitcoins are transferred'
date: 2020-01-10 22:16:06
tags: [Blockchain, Bitcoin]
categories: [Learn X in Y minutes, Learn blockchain in Y minutes]
---


# The Process of a Bitcoin Transaction

Suppose that Alice is transferring 8 bitcoins to Bob, the process goes like this:

1. To initiate a Bitcoin transaction, Alice needs to have: address, private key, wallet
2. Alice signs her bitcoins in the wallet using the private key, transfers to Bob to initiate a transaction
3. Through the Internet, the transaction information start to broadcast to nodes on bitcoin-net
4. A node packs the transaction into the candidate block and starts hash calculation, which is called mining, to win the bookkeeping right
5. A node successfully mines, broadcasts to the whole network, generates new blocks and adds them to the end of the chain
6. Each node acknowledges that it will continue to add blocks after former ones. Mining nodes receive bitcoin rewards. Generally, the transaction is permanently retained after 6 blocks are added
7. Bob gets transferred bitcoins (expressed as UTXO of the transaction)

# Five Key Technologies

In order to achieve a successful bitcoin transaction, the following five key technologies are needed:

- Distributed ledger and decentralized network
- Unused transaction output (UTXO)
- The data structure of bitcoin blockchain
- Proof of workload consensus mechanism
- Bitcoin mining mechanism and generation mechanism

## Distributed ledger and decentralized network

Bitcoin network does not have a central server. It is composed of many full nodes and light nodes. Among them:

- **Full nodes** contain block data of all bitcoin blockchains;
- **Light nodes** only include data related to them.

Compared to traditional centralized transaction system, the Bitcoin uses a distributed ledger in which users open "accounts," strictly speaking, addresses. Everyone can set up an "account" on the bitcoin blockchain and get a pair of a public key and a private key. The address is the hash value of the public key. We interact with the address through the private key.

Each of us has a wallet, which stores a private key. When two people transfer bitcoin to each other, they can do it directly through their wallet software.

Here, the decentralization of bitcoin is reflected in the fact that there is no longer a centralized organization for centralized management of ledgers. The account books are stored in the decentralized network composed of many nodes; there is no longer a centralized organization to help us manage accounts and deal with transactions. Everyone manages their own wallets, and the transactions are recorded by the distributed account books.

Some people will ask whether the bitcoin in our address is recorded in the account book or whether there seems to be a "centre" to store our assets. In fact, this ledger is stored in the decentralized network in a distributed way, so from this perspective, it can be seen as decentralized.

In contrast, for centralized online payment systems, centralized servers usually manage centralized ledgers. For the bitcoin system, the system behind it is a decentralized network, and network nodes jointly maintain a distributed ledger.

(to be continued)