## 3 · Inside Ethereum Transactions



In order to broadcast transactions to the network, we'll need to sign them first. I'm going to use an additional JavaScript library to do this called [ethereumjs-tx](https://github.com/ethereumjs/ethereumjs-tx). You can install this dependency from the command line like this:

```
$ npm install ethereumjs-tx
```

The reason we're going to use this library is that we want to sign all of the transactions locally. If we were running our own Ethereum node locally, we could unlock an account that was stored locally and sign all of our transactions locally. If that were the case, we would not necessarily need to use this library. However, we're using a remote node hosted by Infura in this tutorial. While Infura is a trustworthy service, we still want to sign the transactions locally rather than giving the remote node manage our private keys.

That's exactly what we'll do in this lesson. I'll show you how to create the raw transaction, sign it, then send the transaction and broadcast it to the network! In order to do this, I'm going to create a simple `app.js` file to run the code in this lesson, rather than doing everything in the console.

Inside the `app.js` file, we'll first require the newly installed library like this:

```
var Tx = require('ethereumjs-tx')
```

Next, we'll set up a Web3 connection like we did in the previous lessons:

```
const Web3 = require('web3')
const web3 = new Web3('https://ropsten.infura.io/YOUR_INFURA_API_KEY')
```

Notice, that we're using the Ropsten test network, which is different from the Ethereum main net that we used in the previous lesson. We want to use a test network because all transactions cost gas in the form of Ether. We can use fake Ether on the Ropsten test net without worrying about spending any money. You can obtain fake Ether from a faucet on the Ropsten test network with a faucet. Here are two faucets you can use:

http://faucet.ropsten.be:3001/

https://faucet.metamask.io/

In this lesson, we're going to create a transaction that sends fake Ether from one account to another. In order to do this, we'll need two accounts and their private keys. You can actually create new accounts with Web3.js like this:

```
web3.eth.accounts.create()
// > {
//    address: "0xb8CE9ab6943e0eCED004cDe8e3bBed6568B2Fa01",
//    privateKey: "0x348ce564d427a3311b6536bbcff9390d69395b06ed6c486954e971d960fe8709",
//    signTransaction: function(tx){...},
//    sign: function(data){...},
//    encrypt: function(password){...}
// }
```

Once you have created both of these accounts, make sure you load them up with fake Ether from a faucet. Now, we'll assign them to variables in our script like this:

```
const account1 = '0xb8CE9ab6943e0eCED004cDe8e3bBed6568B2Fa01'
const account2 = '0xb8CE9ab6943e0eCED004cDe8e3bBed6568B2Fa02'
```

Be sure to use the accounts you generated, as these accounts won't work for this lesson. Now, let's save the private keys to the environment like this:

```
export PRIVATE_KEY_1='your private key 1 here'
export PRIVATE_KEY_1='your private key 2 here'
```

We want to save these private keys to our environment so that we don't hard code them into our file. It's bad practice to expose private keys like that. What if we accidentally committed them to source in a real project? Someone could steal our Ether! Now we want to read these private keys from our environment and store them to variables. We can do this with the `process` global object in NodeJS like this:

```
const privateKey1 = process.env.PRIVATE_KEY_1
const privateKey2 = process.env.PRIVATE_KEY_2
```

In order to sign transactions with the private keys, we must convert them to a string of binary data with a Buffer, a globally available module in NodeJS. We can do that like this:

```
const privateKey1 = Buffer.from(process.env.PRIVATE_KEY_1)
const privateKey1 = Buffer.from(process.env.PRIVATE_KEY_2)
```

Alright, now we've got all of our variables set up! I know some of this might be a little confusing at this point. Stick with me; it will all make sense shortly. :) You can also reference the video above if you get stuck.

From this point, we want to do a few things:

1. Build a transaction object
2. Sign the transaction
3. Broadcast the transaction to the network

We can build the transaction object like this:

```
const txObject = {
    nonce:    web3.utils.toHex(txCount),
    to:       account2,
    value:    web3.utils.toHex(web3.utils.toWei('0.1', 'ether')),
    gasLimit: web3.utils.toHex(21000),
    gasPrice: web3.utils.toHex(web3.utils.toWei('10', 'gwei'))
  }
```

Let me explain this code. We're building an object that has all the values needed to generate a transaction, like `nonce`, `to`, `value`, `gasLimit`, and `gasPrice`. Let's break down each of these values:

- `nonce` - this is the previous transaction count for the given account. We'll assign the value of this variable momentarily. We also must convert this value to hexidecimal. We can do this with the Web3.js utilitly `web3.utils.toHex()`
- `to` - the account we're sending Ether to.
- `value` - the amount of Ether we want to send. This value must be expressed in Wei and converted to hexidecimal. We can convert the value to we with the Web3.js utility `web3.utils.toWei()`.
- `gasLimit` - this is the maximum amount of gas consumed by the transaction. A basic transaction like this always costs 21000 units of gas, so we'll use that for the value here.
- `gasPrice` - this is the amount we want to pay for each unit of gas. I'll use 10 Gwei here.

Note, that there is no `from` field in this transaction object. That will be inferred whenever we sign this transaction with `account1`'s private key.

Now let's get assign the value for the `nonce` variable. We can get the transaction nonce with `web3.eth.getTransactionCount()` function. We'll wrap all of our code inside a callback function like this:

```
web3.eth.getTransactionCount(account1, (err, txCount) => {
  const txObject = {
    nonce:    web3.utils.toHex(txCount),
    to:       account2,
    value:    web3.utils.toHex(web3.utils.toWei('0.1', 'ether')),
    gasLimit: web3.utils.toHex(21000),
    gasPrice: web3.utils.toHex(web3.utils.toWei('10', 'gwei'))
  }
})
```

And there is the completed transaction object! We've completed step 1. :) Now we must move on to step 2 where we sign the transaction. We can do that like this:

```
const tx = new Tx(txObject)
tx.sign(privateKey1)

const serializedTx = tx.serialize()
const raw = '0x' + serializedTx.toString('hex')
```

Here we're using the `etheremjs-tx` library to create a new Tx object. We also use this library to sign the transaction with `privateKey1`. Next, we serialize the transaction and convert it to a hexidecimal string so that it can be passed to Web3.

Finally, we send this signed serialized transaction to the test network with the `web3.eth.sendSignedTransaction()` function like this:

```
web3.eth.sendSignedTransaction(raw, (err, txHash) => {
  console.log('txHash:', txHash)
})
```

And there you go! That's the final step of this lesson that sends the transaction and broadcasts it to the network. At this point, your completed `app.js` file should look like this:

```
var Tx     = require('ethereumjs-tx')
const Web3 = require('web3')
const web3 = new Web3('https://ropsten.infura.io/YOUR_INFURA_API_KEY')

const account1 = '' // Your account address 1
const account2 = '' // Your account address 2

const privateKey1 = Buffer.from('YOUR_PRIVATE_KEY_1', 'hex')
const privateKey2 = Buffer.from('YOUR_PRIVATE_KEY_2', 'hex')

web3.eth.getTransactionCount(account1, (err, txCount) => {
  // Build the transaction
  const txObject = {
    nonce:    web3.utils.toHex(txCount),
    to:       account2,
    value:    web3.utils.toHex(web3.utils.toWei('0.1', 'ether')),
    gasLimit: web3.utils.toHex(21000),
    gasPrice: web3.utils.toHex(web3.utils.toWei('10', 'gwei'))
  }

  // Sign the transaction
  const tx = new Tx(txObject)
  tx.sign(privateKey1)

  const serializedTx = tx.serialize()
  const raw = '0x' + serializedTx.toString('hex')

  // Broadcast the transaction
  web3.eth.sendSignedTransaction(raw, (err, txHash) => {
    console.log('txHash:', txHash)
    // Now go check etherscan to see the transaction!
  })
})
```

You can run the `app.js` file from your terminal with NodeJS like this:

```
$ node app.js
```

Or simply:

```
$ node app
```
