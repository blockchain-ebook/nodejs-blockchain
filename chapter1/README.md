#Chapter 1 Introduction

Bitcoin on Nodejs

Note: The chinese book is at: http://bitcoin-on-nodejs.ebookchain.org

Yishu official site: http://ebookchain.org (Wechat group: chainclub)

Translation Cooperation: http://chainmap.org + https://elementus.io/ (thanks to anbei ..link)



##About

This book can be used as an introductory book of the application of Node. Js to develop encryption, as well as the official document for Yishu(and Lisk,with Crypti core platform).

The source code that this book shares is Ebookcoin (Yishu coin), which is a powerful motivation of Yishu. Just like Lisk (raising more than 6.5 million), Yishu is an independent branch of Crypti (no longer under maintenance) and has sidechain function similar to Ethereum, which can carry a variety of decentralized applications. Therefore, no matter if you are studying Crypti, Lisk, Ebookcoin, or node. js, this book is worthy of reference.

The goal of Yishu is to create decentralized software for everyone and promote the sharing of human knowledge.Yishu is driven by Crypto, and unlike other competing currency products, we hope to be the first "zero-threshold" [[??]]Crypto product based on practical software. Please refer to hyperlink for details.

Writing this book is also the best practice for Yishu. Yishu is simple and fast in writing and publishing. The article is currently free on github and http://bitcoin-on-nodejs.ebookchain.org.

##Part 1: Understand Crypto

###Crypto is currency

This part is a introduction of Crypto, as well as a quick guide for anyone without knowledge towards bitcoin or Crypto.

###Preface

"Crypto is currency" sounds quite silly. But if you think of its implication,  "Crypto may not be currency," you may find it interesting. In fact, many of my friends, including myself, held the latter opinion at the very beginning, and it took me some time to reach this conclusion.

The law of inertia exists not only in the material world, but also in the cognitive world. The greater our experience is, the greater the resistance to understanding new things especially towards brand-new stuffs without eye-catching name and novel technology. It could be quite   confusing to imagine that technology is a disruptive innovation and it will change the world.

I am trying to write a simple, entry-level article for those who have not been exposed to cryptocurrencies to bridge human thinking and cryptocurrencies. If you get a little excited and want to keep exploring, this article is a success.

This article covers some basic concepts including: what is Crypto? How is it different from digital coins that people use every day? What are the advantages? Why are they so popular? Basically, it is about the most basic understanding I have as a programmer.

###The simplest history of Crypto

Crypto is a kind of digital currency. Before bitcoin appeared, "digital currency", "virtual currency", "electronic money" had already emerged, especially "virtual currency", which refers to  digitization or virtualization of “real”, “legal” currency, such as the US dollar and RMB, through online banking, alipay, etc, enabling people to pay without money.

Later, on the game platform, the concept of game currency was first put forward. Players used game currency to buy equipments. Then, websites launched various currencies to attract users. The most intuitive interpretation of those game currency, is actually "token".

In recent years, bitcoin, a real digital currency that can be called money, came out. However, people are reluctant to distinguish it from other forms of digital coins. On the one hand, the law of inertia still exists: people around the world have rich experience in using digital currency, why is it different?

On the other hand, the resistance of acceptance is huge: to understand Crypto, people need to know about P2P network, encryption, decryption, and maybe database technology, neither of which [[which==>these]] is not commonly understood by people.

Even if all of these technologies are understood, it is not easy to implement all functions and features like a real currency. In people's thinking, the "digital currency" is a “token” rather than a currency, so the statement "Crypto is currency" could be confusing indeed.

###What is Crypto?

"Crypto" can be interpreted as an encrypted electronic money (or digital currency), a typical example is the bitcoin. So let's use bitcoin to define Crypto:

Crypto is a kind of cryptographic electronic currency based on point-to-point network (P2P network), without issuing institutions and with a fixed total amount.

###Notes:

** (1) P2P network: ** this is not a new concept. The first BitTorrent protoco we used was based on P2P network, which is supported by many download tools. Its advantage is "decentralization", that is, there is no central server, the files downloaded are all on the user's own computer, and the download speed is faster;

** (2) No issuing institutions: ** that is to say, it is not controlled by certain company, bank or state. This requires very complex mechanisms and rules in programming.

** (3) Fixed total amount: ** this is a strategy to ensure the value of Crypto.  "Scarcity equals to preciousness" - anything without limit will lose its attraction. This is different from online community credits.

** (4) Encryption: ** encryption mentioned here is not access control such as user name or password, but trading and transmission encryption. Cryptology theory is complex, but it's not complicated to use.

** (5) Electronic currency: ** Encrypted currency is money. Like legal currency, it can be freely traded. So what about community credits? Are they currency? The answer is no, which will be explained in detail below.

###Crypto is Currency
 
Crypto-currency is currency, which is a bizarre conclusion that is close to "nonsense," and it's much easier to understand when we understand the history and concept definitions above. But if you're not a technologist and have trouble to understand it, please compare it with legal currency and find some common ground.

** (1) Quantitative: ** the legal currency is usually tied to the gold behind it or the so-called GDP, which is relatively fixed (We do not discuss excess issuance here [[==>(We do not want to discuss the excessive quantities insurance here)]]. Crypto can be counted as an absolute fixed amount, or a small amount of additional issuance (which can make up for some lost currency, etc.) to prevent inflation.

However, there is no fixed amount of digital coins provided by some websites.

** (2) Encryption: ** legal currency has physical security of highest level of technology and can be verified through the counterfeit detector. Encryption technology of Crypto is the equivalent of anti-counterfeiting technology, and every transaction will be strictly encryped[[==>encrypted]]. Cracking this encryption is theoretically possible, but not feasible financially, so it is even more difficult to counterfeit Crypto. At the same time, the verification mechanism of Crypto is much simpler, quicker and more accurate than legal currency.

However, the digital currency of some individual websites is only a series of numbers, and the administrator can modify and freeze information, and their encryption is permission control when the user logs in.

** (3) Transaction: ** legal currency, also known as currency or general equivalent, can be exchanged with any merchant and bought anything, which is the most basic attribute of currency. The same is true for encrypted money, which you can pay to any party, and the encrypted money will arrive safely without fear of being hijacked, hacked, or reduced.

However, this is not true for digital currency of some websites: they have no such function at all, and can only be traded within the website. Some stores can trade digital currency between many websites, but those websites belong to the same owner. Of course, a website can trade with other websites if it has an agreement with a third party to set the value of its digital currency, but the essence of this transaction is still legal currency. No one is likely to sign this agreement, at least not me, because there is no oversight, no control, no guarantee of absolute trust.

###Is Crypto Reliable? 

From discussion above, we understand how Crypto works, but we may still cast doubt on the usage of Crypto. Is it reliable? This is the first question that many people ask. The answer is yes, of course, but there are many techniques and theories involved to explain why. Fortunately, these technologies and theories are mature technologies. If you think they're reliable, the following explanation is easy to understand. Otherwise, it's hard to convince yourself that crypto-currencies are more reliable than the digital ones of some websites.

#### (1) Decentralization
It is easier to start with "centralization". Currently, all the major websites we visit are centralized, since we must have one or more servers to sort out the content we browse. If the server breaks down, no one will have access to either websites. In other words, centralization means that everything is controlled by an organization or company.

Decentralization, on the other hand, is based on P2P network, without any machine as the centralized server: every computer in the network is equal, and if anyone dropped down, the network will still continue. If everyone trusts the network, it will last forever, just like bitcoin networks. Specifically, please refer to the chapter with a sophisticated p2p network implementation".

This is an Crypto trading channel [[==>a Crypto trading channel]]. Network foundation, barrier-free transaction… [[==>a network foundation with barrier-free transactions.]] Anytime, anywhere, as long as you can access the Internet, you can get involved in this trading network and pay Crypto  to any part of the world.

#### (2) Encryption and Decryption


We have free channels, but are we safe? How about if the channel get hacked?

This requires advanced encryption and decryption technology. Fortunately, encryption and decryption technology have been  used in the Internet communities for years, safe and sound [[==>solid]]. Theoretically, it is meaningless to decrypt and crack a single node and difficult to decrypt all nodes. Moreover, the number of P2P network nodes is huge, so the security level of Crypto should be the highest for now. [[==>so the security level of Crypto currency system is very high]]

This is the foundation of Crypto. With this tech, we can safely pay out the Crypto  without fear of losing or getting stolen, and the buyer can have the basic motivation to pay [[+Crypto]] for transactions. For encryption and decryption technology, please refer to [[+chapters in ]] "Encryption and decryption technology in node.js" and "three graphs to give you a comprehensive grasp of encryption and decryption technology".

#### (3) Blockchain

We can make the payment safely, but another concern comes. How can we guarantee that the seller has received it or not? What if the seller doesn't get paid? Who will guarantee this sense of trust?

The answer is Blockchain. This is the invention of the Crypto currency. It is the simple database technology. The essence of blockchain is that  transaction data is stored in a database, and its structure is that each record will record the hash value of previous record, so that we can trace back to the first block.


More importantly, the database, which is distributed across P2P networks, holds a copy of each node, and everyone can access it publicly and view the transaction records. In other words, not only can both parties see the transaction result, but the entire network can see it. It is open, transparent and traceable, so you have to believe it.

This is the credit guarantee of Crypto. No economic actions can be achieved without trust. This invention of Crypto opens a door for open, transparent and traceable credit system, and provide infinite possibilities for imagination by companies, organizations and individuals. Later, I will continue to share the code of the blockchain of Yishu, <<the mysterious blockchain>> (to be completed).


#### (4) Consensus Mechanism


Is it enough to only have security and credibility? Another problem is, since there are so many nodes in the same database, how can we decide which node’s writing should be accepted to maintain the blockchain data? Moreover, Crypto has a fixed amount of total currency, what if a node misbehave itself and increase the total amount?


The solution is "Consensus Mechanism", i.e. an algorithms mechanisms, including Proof of Work Mechanism (POW), Proof of Stack (POS), Delegated Proof of Stack (DPOS) etc, which is like the principle of collective decisions and rules in a discussion. This is where the coding focus for Crypto, and it is also the difficult part of Crypto development.

In particular, the DPOS mechanism is basically the shareholder voting mechanism of a joint-stock company, which is the algorithm mechanism used by Yishu. Later, I will continue to share articles, including Yishu <<implementation of DPOS mechanism >>

This is the operation rule of Crypto. It is the place where the previous common technologies are integrated and innovated. Without understanding this part, it is impossible to talk about Crypto development.

Finally, the above techniques are interdependent and mutual supported through consensus mechanism as a whole, and this ensures the absolute safety of payments and trades without issuing institutions.

###Conclusion

Imagine, what a highly autonomous network would bring us(future trend)? What can we do with Crypto (applications)? If you invest or start a business in this industry, what should you pay attention to (risk and methods)? Read the following passage: interests, the common goal of devil and angel. For answers.



.

