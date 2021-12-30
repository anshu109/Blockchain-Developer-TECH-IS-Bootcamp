## 4 · Sending Ether & Events

Now let's write a function that accepts Ether. To demonstrate this, I'll create an ico-like contract with a `buyToken()` function. This will allow an account to send ether to pay for tokens with Ether. The smart contract will be responsible for issuing the tokens, tracking the balance of the account, and also transferring the Ether funds to another wallet address. Again, this is a pseudo-ico example. If you'd like to see a complete ICO tutorial, you can check out my other tutorials [Code Your Own Cryptocurrency On Ethereum](https://www.dappuniversity.com/articles/code-your-own-cryptocurrency-on-ethereum) and [Real World ICO](https://www.dappuniversity.com/articles/real-world-ico).

First we'll create a mapping to track the token balances like this:

```
mapping(address => uint256) public balances;
```

Now I'll create the `buyToken()` function that will increment the balance like this:

```
function buyToken() public {
    balances[msg.sender] += 1;
}
```

Now I'll declare a wallet where the ether funds will be sent whenever an account buys tokens:

```
address wallet;
```

Now we can transfer funds to the wallet whenever the `buyToken()` function is called like this:

```
function buyToken() public payable {
    balances[msg.sender] += 1;
    wallet.transfer(msg.value);
}
```

Now let me explain this code:

- We can transfer ether directly to the wallet by calling `wallet.transfer()`
- We can get the value of the Ether sent in by function caller with `msg.value`, just like `msg.sender`
- We also must use the `payable` modifier so that accounts can send Ether when calling the function.

Likewise, to update to Solidity 0.5.1, we must explicitly declare the wallet `payable` as well:

```
address payable wallet;
```

Now let's set the wallet address inside the constructor function of the contract (we'll use the payable modifier here, too):

```
constructor(address payable _wallet) public {
    wallet = _wallet;
}
```

Awesome! Now watch the video above as I demonstrate how to send Ether with this function inside of Remix. You complete smart contract code should look like this up to this point:

```
contract MyContract {
    mapping(address => uint256) public balances;
    address payable wallet;

    constructor(address payable _wallet) public {
        wallet = _wallet;
    }

    function buyToken() public payable {
        balances[msg.sender] += 1;
        wallet.transfer(msg.value);
    }
}
```

Now I'll show you how to create a default or "fallback" function inside Solidity. This is a function that will get called anytime an account sends Ether to a smart contract. This is a very common pattern for ICO smart contracts, where you simply send Ether to a smart contract and it executes a function. That's exactly what we'll do here. We'll declare a fallback function that wraps the `buyTokens()` function. It will purchase tokens any time an account sends Ether to the smart contract. We can do that like this:

```
function() external payable {
    buyToken();
}
```

Awesome! Now watch the video above as I demonstrate how to send Ether to the smart contract inside Remix.

Now let's talk about Events. Events are a way of dealing with the asynchronous nature of the blockchain. We can declare events inside smart contracts that can be subscribed to by external consumers. These consumers will be able to listen for these events to know that something happened inside the smart contract. We'll declare a `Purchase` event at the top of the smart contract like this:

```
event Purchase(
    address _buyer,
    uint256 _amount
);
```

This event will log the buyer and the token amount. We'll be able to trigger this event inside the `buyToken()` function like this:

```
function buyToken() public payable {
    balances[msg.sender] += 1;
    wallet.transfer(msg.value);
    emit Purchase(msg.sender, 1);
}
```

This event will allow us to know any time a token is purchased inside this smart contract. But what if we only wanted to listen for events that were relevant to our account? Good news! Solidity allows us to subscribe to filter events by indexed values. We can index the `_buyer` inside the event like this:

```
event Purchase(
    address indexed _buyer,
    uint256 _amount
);
```

Awesome! Now you know how to work with events inside Solidity. At this point your complete smart contract code should look like this:

```
contract MyContract {
    mapping(address => uint256) public balances

    event Purchase(
        address indexed _buyer,
        uint256 _amount
    );

    constructor(address payable _wallet) public {
        wallet = _wallet;
    }

    function() external payable {
        buyToken();
    }

    function buyToken() public payable {
        balances[msg.sender] += 1;
        wallet.transfer(msg.value);
        emit Purchase(msg.sender, 1);
    }
}
```
