# Blockchain Introduction

<img src="https://github.com/anshu109/Blockchain-Developer-TECH-IS-Bootcamp/blob/main/Images/Blockchain_intro.jpg" width="1800" height="400"/>

## How it all started?

Habour and Scott Serenata in 1991 published a paper called "How to Timestamp a Digital Document" 

you can find that paper in this <a href="http://www.anf.es/pdf/Haber_Stornetta.pdf">Link<a/>

If you look through the papers, you read through it, you'll find that the concepts of what now we call a block chain and all of the or most of the features and ideas behind the blockchain actually are present in that paper.

## So what actually is a Blockchain?
  
  In layman term, A blockchain is a growing chain of Blocks. 
  
According to wikipedia A blockchain is a growing list of records, called blocks, that are linked together using cryptography. Each block contains a cryptographic hash of the previous block, a timestamp, and transaction data.
  
 A blockchain network can track orders, payments, accounts, production and much more. And because members share a single view of the truth, you can see all details of a transaction end to end, giving you greater confidence, as well as new efficiencies and opportunities.
Today the most popular cryptocurrency  “Bitcoin” runs on blockchain technology and that is one reason why this cryptocurrency is becoming popular with each passing day. Because of the transparency in each step of a transaction in blockchain technology, now most of the businesses are adopting it globally.

  
 ### let's have a look at one of these records or so-called Bloks.
  
As the name suggests a blockchain is nothing but a chain of blocks with some information.
  
Each Block has
- Data
- Hash
- Hash of the previous block
  
<img src="https://github.com/anshu109/Blockchain-Developer-TECH-IS-Bootcamp/blob/main/Images/Block.png" width="800" height="400"/>
  
**We know about the data but what is Hash?**
  
A digital hash is like a fingerprint of some amount of data  which is unique to each block just like a fingerprint. So once a block is created, any change inside the block will cause the hash to change. If the fingerprint of a block changes, it does not remain the same block.That is a major reason why it is almost impossible to hack a blockchain network because even a minute change can change the entire network.
  
  <img src="https://github.com/anshu109/Blockchain-Developer-TECH-IS-Bootcamp/blob/main/Images/hash.png" width="800" height="400"/>
  

### Genesis Block
  The first block in a blockchain network is called the Genesis Block. Because after the block chain is initialized, that block will always stay block number one forever and ever and ever for eternity. Since this is the first block it doesnt have any previous hash and previous hash will be equal to 0
 
### But how blockchains are created ?
  
  - First block will be the Genesis block. It will have some data and a hash value but willnot have any previous hash.
  - Second Block will have again some data and its hash and hash of genesis/ first block (previous hash)
  <img src="https://github.com/anshu109/Blockchain-Developer-TECH-IS-Bootcamp/blob/main/Images/bc-network.png" width="800" height="400"/>
