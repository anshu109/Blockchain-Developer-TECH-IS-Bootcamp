## #1 · What is Web3.js?

The above video is the first in this 8-part tutorial series. In this lesson, I'll give you an overview of the Web3.js library, and then I'll show you how to check the balance of an Ethereum account.

There are a few different aspects to developing blockchain applications with Ethereum:

1. Smart contract development - writing code that gets deployed to the blockchain with the Solidity programming language.
2. Developing websites or clients that interact with the blockchain - writing code that reads and writes data from the blockchain with smart contracts.

[Web3.js](https://web3js.readthedocs.io/en/1.0/) enables you to fulfil the second responsibility: developing clients that interact with The Etherem Blockchain. It is a collection of libraries that allow you to perform actions like sending Ether from one account to another, read and write data from smart contracts, creating smart contracts, and so much more!

If you have a web development background, you might have used jQuery to make Ajax calls to a web server. That's a good starting point for understanding the function of Web3.js. Instead of using a jQuery to read and write data from a web server, you can use Web3.js to read and write to The Ethereum Blockchain.

Let me explain how you can use Web3.js to talk to The Ethereum Blockchain. Here is a diagram of how a client talks to Ethereum:

<img src="https://www.dappuniversity.com/web3-js-diagram.png" alt="web3-js-diagram" style="zoom:67%;" />

*Image credit: [iotbl](https://iotbl.blogspot.com/2017/03/ethereum-and-blockchain-2.html).*

Web3.js talks to The Ethereum Blockchain with [JSON RPC](https://en.wikipedia.org/wiki/Remote_procedure_call), which stands for "Remote Procedure Call" protocol. Ethereum is a peer-to-peer network of nodes that stores a copy of all the data and code on the blockchain. Web3.js allows us to make requests to an individual Ethereum node with JSON RPC in order to read and write data to the network. It's kind of like using jQuery with a JSON API to read and write data with a web server.



### Dependencies

There are a few dependencies that will help you start developing with Web3.js.

#### Node Package Manager (NPM)

The first dependency we need is [Node Package Manager](https://nodejs.org/en/), or NPM, which comes with Node.js. You can see if you have node already installed by going to your termial and typing:

```
$ node -v
```

#### Web3.js Library

You can install the Web3.js library with NPM in your terminal like this:

```
$ npm install web3
```

#### Infura RPC URL

In order to connect to an Ethereum node with JSON RPC on the Main Net, we need access to an Ethereum node. There are a few ways you could do this. For one, you could run your own Ethereum node with [Geth](https://github.com/ethereum/go-ethereum/wiki/geth) or [Parity](https://www.parity.io/). But this requires you to download a lot of data from the blockchain and keep it in sync. This is a huge headache if you've ever tried to do this before.

Mostly for convenience, you can use [Infura](https://infura.io/) to access an Ethereum node without having to run one yourself. Infura is a service that provides a remote Ethereum node for free. All you need to do is sign up and obtain an API key and the RPC URL for the network you want to connect to.

Once you've signed up, your Infura RPC URL should look like this:

```
https://mainnet.infura.io/YOUR_INFURA_API_KEY
```

### Checking Account Balances

Now that all of your dependencies are installed, you can start developing with Web3.js! First, you should fire up the Node console in your terminal like this:

```
$ node
```

Now you've got the Node console open! Inside the Node console, you can require Web3.js like this:

```
const Web3 = require('web3')
```

Now you have access to a variable where you can create a new Web3 connection! Before we generate a Web3 connection, we must first assign the Infura URL to a variable like this:

```
const rpcURL = "https://mainnet.infura.io/YOUR_INFURA_API_KEY"
```

Make sure that you replace `YOUR_INFURA_API_KEY` with your actual Infura API key that you obtained earlier. Now you can instantiate a Web3 connection like this:

```
const web3 = new Web3(rpcURL)
```

Now you have a live Web3 connection that will allow you to talk to the Ethereum main net! Let's use this connection to check the account balance for this account: [0x90e63c3d53E0Ea496845b7a03ec7548B70014A91](https://etherscan.io/address/0x90e63c3d53e0ea496845b7a03ec7548b70014a91). We can see how much Ether this account holds by checking its balance with `web3.eth.getBalance()`.

First, let's assign the address to a variable:

```
const account = "0x90e63c3d53E0Ea496845b7a03ec7548B70014A91"
```

Now, let's check the account balance like this:

```
web3.eth.getBalance(address, (err, wei) => {
  balance = web3.utils.fromWei(wei, 'ether')
})
```

Let me explain this code. First, we use check the balance by calling `web3.eth.getBalance()`, which accepts a callback function with two arguments, an error and the balance itself. We'll ignore the error argument for now, and reference the balance with the `wei` argument. Ethereum expresses balances in Wei, which is the smallest subdivision of Ether, kind of like a tiny penny. We can convert this balance to Ether with `web3.utils.fromWei(wei, 'ether')`.

And that's it! That's the conclusion to the first part of this tutorial. Now you've seen what the Web3.js library is and you can get started using it to check Ethereum account balances. Here is a summary of the code we wrote in this tutorial:

```
const Web3 = require('web3')
const rpcURL = '' // Your RPC URL goes here
const web3 = new Web3(rpcURL)
const address = '' // Your account address goes here
web3.eth.getBalance(address, (err, wei) => {
  balance = web3.utils.fromWei(wei, 'ether')
})
```
