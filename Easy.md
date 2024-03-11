## Easy

###  What is the difference between private, internal, public, and external functions?

`public` means anyone can access the function, other smart contracts, addresses that interact with this smart contract or the smart contract itself.
`private` means that only this smart contract is allowed to use the function.
`internal` means that only this smart contract and smart contracts that inherit from it can use this function.
`external` means anyone can access the function unless the smart contract itself. So only other smart contracts and addresses that interact with this smart contract.

### Approximately, how large can a smart contract be?

24,576 bytes

### What is the difference between create and create2?

Each one offers a different way to deploy contracts, with “create” being straightforward, “create2” providing predictable contract addresses with the help of salt.

### What major change with arithmetic happened with Solidity 0.8.0?

Starting with version 0.8.0, Solidity automatically includes checks for arithmetic underflows and overflows as a built-in feature of the language. Solidity will revert if an expression causes an integer underflow or overflow.

### What special CALL is required for proxies to work?

`DELEGATECALL` is required for proxy contracts to work because this type of call will preserve the msg (msg.sender, msg.value, etc…) and run in the context of the calling contract instead of the called contract. This enables the proxy contract to call an implementation contract to modify the proxy contract’s state. 
By using DELEGATECALL, the implementation contract can be upgraded without the smart contract system losing any information or having to change the address of the proxy.

### Prior to EIP-1559, how do you calculate the dollar cost of an Ethereum transaction?

Prior to EIP-1559, the dollar cost calculation of an Ethereum transaction was ((GAS USED * GAS PRICE / 10^9) * CURRENT ETHER PRICE. In this era, the miner received 100% of the gas cost. After EIP-1559, the dollar cost calculation of an Ethereum transaction is (((BASEFEE + PRIORITY FEE) * GAS USED)) / 10^9) * CURRENT ETHER PRICE.
Now, the miner receives a portion of the gas cost (PRIORITY FEE) and the other portion, the protocol fee (BASEFEE), is burned.

### What are the challenges of creating a random number on the blockchain?

Generating true random numbers in a blockchain is challenging. This is because a blockchain is a decentralized and trustless environment where the random number generation can easily be manipulated or biased by any party in the network. Chainlink is the best alternative for the random number.

### What is the difference between a Dutch Auction and an English Auction?

The Dutch auction is like an English auction, except that prices start high and are successively dropped until a bidder accepts the going price, at which point the auction ends.

### What is the difference between transfer and transferFrom in ERC20?

In ERC20, the transfer function is called to transfer a token amount from msg.sender to a destination address. The transferFrom function is called to transfer a token amount from a specified source address to a destination address. 
For the transferFrom call to be successful, the source address must first give an allowance for msg.sender to be able to spend (at least) the transfer amount.

### Which is better to use for an address allowlist: a mapping or an array? Why?

A mapping is better for an address allowlist because it is more gas efficient. With a mapping, it is possible to check if an address is on the allowlist by directly accessing its value. 
Using an array, verifying an address could be costly because it would require looping through each element.

### Why shouldn’t tx.origin be used for authentication?

tx.origin shouldn’t be used for authentication because an attacker could execute a phishing attack and authenticate as tx.origin. Since tx.origin is the address that initiated the transaction, an attacker could influence a user to execute a malicious transaction on the attacker’s contract and then call a function to authenticate on the target contract. 
In this scenario, tx.origin is the victim’s address and the target contract would authenticate the request and allow the attacker to execute transactions as if the attacker were tx.origin.

### What hash function does Ethereum primarily use?

Keccak-256

### How much is 1 gwei of Ether?

1 Gwei is one billionth (10^-9) of an Ether. 1 Gwei is 10⁹ Wei.

### How much is 1 wei of Ether?

1 Wei = 10^-18 Ether

### What is the difference between assert and require?

Both assert and require are used for error handling, but they behave differently. For example, if a require statement fails, it will revert the transaction and refund the remaining gas to the caller, whereas, assert will revert the transaction but will NOT refund the gas 
(This is true if using a Solidity version prior to version 0.8.0… Version 0.8.0 changed the opcode that assert compiles to from invalid to revert, which refunds all gas). Also, if the condition fails, require allows for an optional error message that could be returned to the user, but assert does not.

### What is a flash loan?

A flash loan is a type of loan in the decentralized finance (DeFi) ecosystem that allows users to borrow assets without having to provide collateral.

### What is the check-effects pattern?

The pattern involves separating the validation of inputs (checks), the changes to the contract state (effects), and the interactions with external contracts or entities (interactions).

### What is the minimum amount of Ether required to run a solo staking node?

32 ETH

### What is the difference between fallback and receive?

receive is a new keyword in Solidity 0.6.x that is used as a fallback function that is only able to receive ether.

`receive()` external payable — for empty calldata (and any value)
`fallback()` external payable — when no other function matches (not even the receive function). Optionally payable.

### What is reentrancy?

Reentrancy attacks occur when a smart contract function temporarily gives up control flow of the transaction by making an external call to a contract that is sometimes written by unknown or possibly hostile actors. This permits the latter contract to make a recursive call back to the primary smart contract function to drain its funds.

### As of the Shanghai upgrade, what is the gas limit per block?

30 million

### What prevents infinite loops from running forever?

Transaction gas limit.

### What is the difference between tx.origin and msg.sender?

tx. origin always refers to the address that initially initiated the transaction and remains constant throughout the transaction chain. On the other hand, msg.sender represents the sender of the current message or contract interaction and changes with each call.

### How do you send Ether to a contract that does not have payable functions, or a receive or fallback?

If a contract does not have any payable functions, a receive function, or a fallback function, it means that the contract is not designed to accept ether directly. In this case, you cannot send ether to the contract directly from an external account or contract.

### What is the difference between view and pure?

`view` function called to query the state of the contract. It cannot emit events and call other non-view and non-pure functions. 
`pure` function can not read and modify the state of the contract. This function can only perform computation solely based on the input parameters. As view function, it cannot emit events and call other non-pure and non-view functions. 

### What is the difference between transferFrom and safeTransferFrom in ERC721?

`transferFrom`: A standard ERC721 function used to transfer tokens from one address to another. It does not include additional safety checks on the recipient.
`safeTransferFrom`: Similar to transferFrom, but includes additional safety checks to ensure that the recipient can handle receiving ERC721 tokens, reducing the risk of accidental loss of tokens.

### How can an ERC1155 token be made into a non-fungible token?

An ERC1155 token can be made into a non-fungible token by ensuring that it’s token id has a maximum supply of one, which represents a unique asset. 
Also, a url can be specified for the token id, which should contain the metadata for the token, such as, image url, attributes, and other properties. This will allow the token to function as a non-fungible token.

### What is access control and why is it important?

Access control is an essential element of security that determines who is allowed to access certain functions of the smart contracts. Contract should'nt allow to everyuser to call the `withdraw` function. Only the admins.

### What does a modifier do?

In Solidity, modifiers are a powerful feature that allows you to add additional behavior to functions. They provide a way to enforce access control, validate inputs, and perform other checks before executing a function.

### What is the largest value a uint256 can store?

uint256 can store is 2²⁵⁶ — 1.

### What is variable and fixed interest rate?

A fixed rate loan has the same interest rate for the entirety of the borrowing period, while variable rate loans have an interest rate that changes over time depending on the market.


## Below questions are not the part of RareSkills interview questions, however a good addition.

### What is the difference between call, staticCall and delegatecall?

`call` function is used to invoke functions in other contracts and send ether along with the call. It has a broader scope of capabilities compared to staticcall as it allows for both reading and writing to the state variables of the target contract.

`Staticcall` is a method similar to call, but it does not allow changing the state of the blockchain. This means that we cannot use staticcall if the called function changes some state variable.

It is also possible to execute a function in another contract, but in such a way that it changes the state variables of the calling contract. For this purpose, the method `delegatecall` is used.


