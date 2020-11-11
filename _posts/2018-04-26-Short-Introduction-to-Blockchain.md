---
title: Short Introduction to Blockchain
tags: [IT, Cryptography]
redirect_from:
    - /blog/introduction-to-blockchain/
---

I was not trying to know about blockchains even after a boom of bitcoin and other cryptocurrencies. I did think that they were bound to supersede the regime of physical currencies, but I anticipated that it should take place in universal economies in ten years or more.

However, [recent events](https://github.com/sikaozhe1997/Xin-Yue) spurred me to think about the significance of blockchain again. Shen Yang, a former Peking University professor, was found to be involved in the sexual harassment toward a female student. Inspired by the #MeToo movement, a number of students and professors in Peking University have been demanding greater information transparency in the case. Yue Xin, a Peking University senior, is one of the participants who submitted materials on information disclosure requests. She and her family were pressured by the school to withdraw from the case, which was posted in an open letter on her social media, whereas both the case and the posts were completely censored.

Unexpectedly, the open letter was uploaded along with [a transaction](https://etherscan.io/tx/0x2d6a7b0f6adeff38423d4c62cd8b6ccb708ddad85da5d3d06756ad4d8a04a6a2) on the ethereum blockchain by another participant to evade censorship. This is the first time that blockchain is utilized to document cersored public events, which could be regarded as a milestone in net neutrality protection. It is also the reason why I am writing this article to introduce blockchains.

## What is a blockchain

According to [Blockchain - Wikipedia](https://en.wikipedia.org/wiki/Blockchain),

> A blockchain, originally block chain, is a continuously growing list of records, called blocks, which are linked and secured using cryptography.

![Blockchain](https://upload.wikimedia.org/wikipedia/commons/thumb/9/98/Blockchain.svg/102px-Blockchain.svg.png)

*From https://upload.wikimedia.org/wikipedia/commons/thumb/9/98/Blockchain.svg/102px-Blockchain.svg.png*

As the name implies, *blockchain* is just a chain of blocks. Consider a tree data structure. The green block above is a *genesis block*, which is the original block (root node) in the whole chain. Black blocks together form the main chain, and this is the reason why it is called 'blockchain'. A few purple blocks are so-called *orphan blocks* branching from the main chain.

## Why we need a blockchain

In traditional systems, one or several central servers is necessary for the storage of records, which are under the control of an individual or organization. In contrast, a blockchain is decentralized on many computers and is no longer vulnerable to a single point of failure (SPOF).

The main idea is that the main chain is stored highly redundantly. Regardless of the orphan blocks, once a new block is added to the main chain, it will be broadcast all over blockchain network and everyone will update their database with the new main chain. The main chain is selected among all the possible chains with a highest score.

Therefore, if someone would like to overwrite an existing block, he or she has to reach a consensus with the whole network. Since blocks will be validated through cryptographic hashes, all subsequent blocks should be altered at the same time as well. Hence the mutability of blocks diminishes along with the growth of peers in the network and blocks in the chain, and the persistence of blocks is ensured with an extremely high probability.

## How can we use a blockchain

The most popular application of blockchains is cryptocurrency. In this respect, part of the participants, called miners, are provided rewards as incentives to validate transactions and attach new blocks including them to the chain. The process of building a block is computationally difficult to restrict the production of new currencies.

Certainly it is not the only use of blockchains. As what I have mentioned above, blocks are used to **record** something, including both cryptocurrency trading, certain events and so forth. In the example above, it is feasible to attach arbitrarily long texts to a transaction, which will be almost unchangeably recorded in the ethereum network.

## Finally to Internet citizens

At the same time, in the United States, the FCC voted in favor of repealing net neutrality pocilies and has published new regulations. Internet freedom is being severely threatened.

While blockchains have demonstrated their great significance in monetary systems, the impact should be far beyond economies. It is an important method of realizing true freedom of speech in public spaces.

I would like to end by quoting words from Oliver Wendell Holmes Jr.:

> If there is any principle of the Constitution that more imperatively calls for attachment than any other, it is the principle of free thought—not free thought for those who agree with us but freedom for the thought that we hate.
