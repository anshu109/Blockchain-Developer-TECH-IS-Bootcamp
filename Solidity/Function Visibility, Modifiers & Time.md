## 3 · Function Visibility, Modifiers & Time

Now let's talk about function visibility. In the last sections, we used the `public` function visibility several times so that the smart contract functions can be called outside the smart contract by accounts connected to the network. What if we wanted to create functions that are only used inside the smart contract? We can do this with the `internal` visibility. Let's move accomplish this by the business logic that increments the `peopleCount` to its own internal function like this:

```
function incrementCount() internal {
    peopleCount += 1;
}
```

This function will only be accessible inside the smart contract, not by the public interface for other accounts. Now we can call this function inside the `addPerson()` function like this:

```
function addPerson(string memory _firstName, string memory _lastName) public {
    incrementCount()
    people[peopleCount] = Person(peopleCount, _firstName, _lastName);
}
```

Now that's an example of other types of function visibly. Let's talk about function modifiers. These will allow us to add special behavior to our functions, like add permissions. Let's create a modifier that only allows an admin or "owner" to call the `addPerson()` function, and restrict everyone else from calling this function.

First, we'll create a way to store the owner of this smart contract. We can do that with a state variable like this:

```
address owner;
```

Now let's create a modifier that checks if the person calling the function is the owner:

```
modifier onlyOwner() {
    require(msg.sender == owner);
    _;
}
```

Let me explain this code:

- First, we declare the modifier with the `modifier` keyword, followed by the modifier name `onlyOwner`.
- Next, we access the current account that's calling the function with `msg.sender`. Solidity provides this value inside the `msg` global variable, just like the current ether of the transaction like `msg.sender` which we saw in previous sections.
- Next, we use `require()` to check that the account calling the function is the owner. If the result of the expression passed into `require()` evaluates to `true`, then code resumes execution. If not, it throws an error. In this case, if the account calling the function is not the owner, then Solidity will throw an error.

Now we can set the owner inside the smart contract as the account that deploys the smart contract like this:

```
constructor() public {
    owner = msg.sender;
}
```

Now we can add the modifier to the `addPerson()` function so that only the owner can call it. While I'm here, I'll show you how I format my Solidity code once the functions start getting big, with lots of arguments, modifiers, visibility, etc...

```
function addPerson(
    string memory _firstName,
    string memory _lastName
)
    public
    onlyOwner
{
    incrementCount()
    people[peopleCount] = Person(peopleCount, _firstName, _lastName);
}
```

Now try to run your code to see if only the owner can call the function successfully without any errors! At th is point your the complete smart contract code should look like this:

```
pragma solidity 0.5.1;

contract MyContract {
    uint256 public peopleCount = 0;
    mapping(uint => Person) public people;

    address owner;

    modifier onlyOwner() {
        require(msg.sender == owner);
        _;
    }

    struct Person {
        uint _id;
        string _firstName;
        string _lastName;
    }

    constructor() public {
        owner = msg.sender;
    }

    function addPerson(
        string memory _firstName,
        string memory _lastName
    )
        public
        onlyOwner
    {
        incrementCount()
        people[peopleCount] = Person(peopleCount, _firstName, _lastName);
    }

    function incrementCount() internal {
        peopleCount += 1;
    }
}
```

Now let's talk about time. We can create a new modifier that uses time to illustrate a use case for time in Solidity. We'll create a new modifier called `onlyWhileOpen` that will check that the current time on the blockchain is past a time that we specify. This is a common practice when creating ICO crowdsale smart contracts that have an "opening time". Often times, these smart contracts will reject investor contributions that are made before the crowdsale has started. We can model similar behavior by restricting access to the `addPerson()` function until after the opening time has passed. First let's create a state variable to store the opening time:

```
uint256 startTime;
```

We can store the `startTime` with this state variable in seconds, as that's how we express timestamps in Solidity. In fact, the time stamps are expressed in [unix time](https://en.wikipedia.org/wiki/Unix_time) which is the number of seconds that have passed since Thursday, 1 January 1970. You can read more about this time convention on [Wikipedia](https://en.wikipedia.org/wiki/Unix_time).

Now I'll set the start time in the constructor like this:

```
constructor() public {
    startTime = 1544668513;
}
```

By the time you read this article, that timestamp will have passed. You can use [this website](https://www.epochconverter.com/) to generate a unix timestamp in the future for your own purposes! Now let's create a modifier to check that the start time has passed like this:

```
modifier onlyWhileOpen() {
    require(block.timestamp >= startTime);
    _;
}
```

Let me explain this code:

- First, we declare the modifier, just like we did in the last example.
- Next, we require the specific condition, just like the last example.
- Now, we compare the start time to "now", by checking the current block's timestamp with `block.timestamp`. This is how we get "now" in Solidity!

Now we can add the modifier to the `addPerson()` function. Try to run this code and see if you can restrict the access to some time in the future! Again, you can use [this website](https://www.epochconverter.com/) to generate a timestamp in the future. Make sure you update this value in the constructor function! The complete smart contract code should look like this:

```
pragma solidity 0.5.1;

contract MyContract {
    uint256 public peopleCount = 0;
    mapping(uint => Person) public people;

    uint256 startTime;

    modifier onlyWhileOpen() {
        require(block.timestamp >= startTime);
        _;
    }

    struct Person {
        uint _id;
        string _firstName;
        string _lastName;
    }

    constructor() public {
        startTime = 1544668513; // Update this value
    }

    function addPerson(
        string memory _firstName,
        string memory _lastName
    )
        public
        onlyWhileOpen
    {
        people[peopleCount] = Person(peopleCount, _firstName, _lastName);
    }

    function incrementCount() internal {
        peopleCount += 1;
    }
}
```
