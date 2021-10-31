---
id: 550
title: Introduction to Blockchains
date: 2017-11-26T07:00:43+00:00
author: Marcel Jurtz
layout: post
guid: https://blog.mjurtz.com/?p=550
permalink: /2017/11/introduction-to-blockchains/
categories:
  - cryptocurrency
tags:
  - bitcoin
  - cryptocurrency
  - p2p
---
What if there was a technology that could entirely change the basic elements of our society and our understanding of economy? This technology already exists and it is called crypto currency. Many people think of bitcoin when this buzzword comes up, but if you take a closer look, you will find out that the monetary point of view of the technology behind is only the tip of the iceberg.

Bitcoin and crypto currencies are currently a highly controversial issue. Prices are rising and rising and the hype seems to be unstoppable, but influential personalities of the global economy are warning against putting too much trust into this technology. Whether they're right about that or whether these antiquated idiots just want to hide their fear of computers with these statements is a mystery, which I won't try to unveil here.

What I would like to achieve with this article is the communication of the basic functioning of Blockchain technology and the associated possibilities and risks, so that you can see this technology not only as an investment, but also understand how it works.

## State of the art

Money exists for trading. In the last few centuries, trade has become complex - everybody trades with everyone, worldwide. In order to keep track of the transactions carried out, many different techniques of accounting. We use _trusted third parties_ for this purpose, banks, for example.

Bitcoin uses a network of computers for accounting. The transactions that have been made are not closed to the public, so the whole transaction history is publicly visible.

As already mentioned, this article does not specifically refer to Bitcoin. However, since Bitcoin is a great example to explain the technology behind it, I will explain the functionality of a blockchain with this example in the following.

Transactions in the form of bank transfers are normally carried out via banks. This third instance regulates that the recipient actually receives the money and the sender owns what he wants to send. Banks act as arbitrators for potential disputes, which increases the transaction costs for the participants.

Another approach to handle transactions is peer-to-peer networks, which relinquish this third instance. Users communicate directly with each other, which results into a distributed network of equal nodes.

The problem of such networks arises from possible fraud attempts by one of the participants, as there is no control authority as it exists in centralized systems.

To conclude, you can say that the execution of a transaction in a peer-to-peer network is based on trust, which makes it easy to exploit.

## What Bitcoin does differently

Bitcoin is now trying to replace the requirement of trust with cryptographic proof. This combines the benefits of peer-to-peer networks (no/low transaction costs) with those of engaging a third party (non-fraud transactions).

Each user possesses a certain number of coins (which can be divided into smaller parts). The interesting thing about this system, however, is that the Bitcoins themselves do not exist, only records of completed transactions. In order to be able to carry out a transaction, a user needs to own a wallet which can be considered a digital banking account. When a wallet is created, a private as well as a public key is generated. An address represents a shortened notation of the public key (several addresses can be derived from one public key). In order to receive Bitcoins, other users must send them to such an address. To receive Bitcoins on the other hand, you need to be in possession of the matching private key. There are various wallet providers that also offer different types of wallets, the website [bitcoin.org](https://bitcoin.org) offers a comprehensive overview regarding this topic.

Once the user has set up a wallet, transfers can be sent and received. A transfer in the network works as follows: User A sends bitcoins to user B. A signs the transaction with his private key, allowing all network participants to verify the origin of the transaction using As public key. The transaction is executed publicly, so all nodes of the network are informed about them and can add them to their _unconfirmed transactions_.

At this point, I have to distinct between active and passive network nodes. Active participants collaborate in the administration of the blockchain, bundle confirmed transactions and try to publish them based on the calculation of cryptographic evidence. Passive participants simply use the network to send and receive bitcoins. To confirm a transaction, the senders account balance must be known, which is possible for any account by knowing the senders address. For example, the website [blockchain.info](https://blockchain.info/) lists all transactions in real time, which can be filtered and searched. Also, this website can be used to check any wallets balance.

To commit a transaction, the sender must reference other transactions, in which he received at minimum the same amount of bitcoins he wants to send. The active network participants, the so-called _miners_, then validate the referenced transactions for whether they have not already been issued and either accept or reject them. This mechanism makes it possible to establish an exact history of where the money comes from and where it goes.

![Example of a blockchain transaction](/assets/2017/blockchain_transaction_example.png)

The example in the picture shows that a transaction can have several inputs. Inputs represent references to transactions the owner has received Bitcoins from, which he now wants to spend. However, transactions can also have multiple outputs. For example, if the output does not exactly match the input, the owner can transfer the difference to himself.

Based on this principle, transactions can already be transparently traced back. However, a temporal component is missing. Miners are used for this purpose, which combine transactions from the network into blocks. All transactions within a block are considered to be processed at the same time.

![Block bundled transactions](/assets/2017/blockchain_transactions.png)

However, once a block has been created, miners must solve a task before sharing it to the network, which prevents individual miners to flood the network. This task is based on the computation of cryptographic evidence, which must be difficult to solve but easy to verify. Published blocks are checked by the receiving nodes for validity of the task solution, and if correct, they are added to the nodes local blockchain. The node then starts creating the next block in the same way. I won't go into the details on the algorithm which is responsible for the cryptographic evidence, but it is automatically updated in a way that a block takes about ten minutes to be discovered.

Miners are allowed to add a transaction out of nowhere to themselves to their block, which motivates the miners and ensures that more bitcoins come into circulation. The amount of bitcoins in these transaction is halved every four years and currently amounts to 12.5 Bitcoin.

## Problems and Drawbacks

Now that the basics of blockchain technology should be clear, it's time to discuss the problems behind it. The basis of the network is the presence of many equal participants. The problem is, therefore, when a network subscriber has a large part of the total computing power of the network, which can cause fake transactions to circulate.

The so-called double-spending attacks are the spending of money that one does not possess at all. These attacks fall into this sector. Imagine you want to buy a car for Bitcoin. You agree on a price and transfer Bitcoins in this amount to the car dealer. The transaction will initially land in the unconfirmed transactions and you will receive the car. Now you create a second transaction in which you transfer the Bitcoins you have just paid for the car to yourself. If you are in possession of a major part of the network's overall computing power, you are quite likely to find the next block. Now make sure that your second transaction is first recorded in a block and accepted by the network. If the other transaction is then considered by a miner for bundling, it will be rejected because you have already issued the referenced inputs.

This problem has to be taken into account if the networks are rather small. However, there are also so-called mining pools in which miners join forces and try to find blocks together. In the event of success, the reward will be distributed to all participants. This may sound good, but depending on the size of the pool, it can also be used by its operators for double-spending attacks.

A further problem, especially with Bitcoin, is the basis on computing power to confirm a block. Due to the high number of nodes, immense computing power is used for the generation of blocks, which does not achieve a direct result and is therefore often regarded as a waste of energy.

Bitcoin technology is also poorly scalable because blocks have a limitation on the size of the transactions they can contain. since a block is found every ten minutes, the amount of unconfirmed transactions accumulates if many network participants want to carry out transactions. By designing transactions with greater input than output, transaction fees are generated, which are treated preferentially by the Miners, as they are allowed to assign the difference to themselves. However, this contradicts the principle of lower transaction costs as explained above.

## Conclusion

Bitcoin is by far not the only blockchain technology, but certainly the best known. The solution to the problem of high energy demand has been taken up by various other technologies and is trying to attract attention with appropriate publications. The same goes for the other problems described above.

A large number of crypto currencies are now in circulation, [coinmarketcap](https://coinmarketcap.com/) provides an overview of market capitalization. In addition, there are various projects for different niche areas, with the idea of a currency rarely being its main objective. Each of these technologies has its own advantages and disadvantages, and each of them could easily fill the length of this article. If I have been able to arouse your interest with this article, I therefore advise you to take a look at the overview list and look for interesting entries yourself.