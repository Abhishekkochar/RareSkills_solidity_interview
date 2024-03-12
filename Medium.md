## Medium

### What is the difference between transfer and send? Why should they not be used?

transfer and send methods are methods used for sending ether from one address to another.

`transfer` function revert if the transcation fails (e.g. due to out of gas or if the destination contract throws an error).
`send` returns the boolean. When using the send method, transcation does not revert if the transfer failed. Additional check is required if the retrun bool is true or false.

### How do you write a gas-efficient for loop in Solidity?

Cache storage variable in a local variable,
Use ++i instead of i++,
use unchecked to update the counter i

### What is a storage collision in a proxy contract?

A storage collision in a proxy contract refers to a scenario where the storage layout of the proxy contract clashes with that of the implementation contract it is proxying. This issue arises when both the proxy contract and the implementation contract have state variables defined in the same storage slots.

Proxy contracts are used to delegate calls to an implementation contract, allowing for upgradability of smart contracts without changing their address. However, when a proxy contract and its implementation contract both define state variables in the same storage slots, any changes made to those variables in the implementation contract won't be reflected when accessed through the proxy contract. This can lead to unexpected behavior and potential data corruption.

### What is the difference between abi.encode and abi.encodePacked?

`abi.encode` when you want to encode multiple arguments into a tightly packed byte array, with each argument padded to its natural size.
`abi.encodePacked` when you want to concatenate arguments without padding, potentially saving gas costs, but resulting in a less readable or aligned byte array.

### uint8, uint32, uint64, uint128, uint256 are all valid uint sizes. Are there others?

Yes. The sizes of the uint types are typically incremented by multiples of 8 bits. 

### What changed with block.timestamp before and after proof of stake?

Now miner has to find such a "timestamp" that satisfies the "if" condition.
After ethereum merge: For each slot, a validator is selected at random and is responsible for creating the new block while a committee of validators is randomly chosen to verify the work of the validator creating the new block. The randomly selected validators receive a reward of Ether for their efforts.

### What is frontrunning?

Frontrunning occurs when an MEV bot monitors the mempool for a trader's pending transaction. Spotting a potential trade, the bot quickly duplicates the users transaction so that they can be the one to get the tokens or the opportunity that the user detected.

### What is a commit-reveal scheme and when would you use it?

The commit-reveal scheme is a technique used in blockchain-based applications to ensure the fairness, transparency, and security of various activities such as voting, auctions, lotteries, quizzes, and gift exchanges. The scheme involves two steps: commit and reveal.

During the commit phase, users submit a commitment that contains the hash of their answer along with a random seed value. The smart contract stores this commitment on the blockchain. Later, during the reveal phase, the user reveals their answer and the seed value. The smart contract then checks that the revealed answer and the hash match, and that the seed value is the same as the one submitted earlier. If everything checks out, the contract accepts the answer as valid and rewards the user accordingly.

The commit-reveal scheme is essential in blockchain-based applications as it ensures that users cannot change their answers once they have submitted them, and prevents others from knowing the answer before the deadline. It also ensures that the process is fair and transparent, providing a secure and reliable way to conduct various activities on a blockchain-based platform.

### Under what circumstances could abi.encodePacked create a vulnerability?

Hash collision: a hash collision is a random match in hash values that occurs when a hashing algorithm produces the same hash value for two distinct pieces of data

### How does Ethereum determine the BASEFEE in EIP-1559?

The base fee is calculated by a formula that compares the size of the previous block (the amount of gas used for all the transactions) with the target size. The base fee will increase by a maximum of 12.5% per block if the target block size is exceeded. This exponential growth makes it economically non-viable for block size to remain high indefinitely.

### What is the difference between a cold read and a warm read?

Cold/warm access refers always to storage, never to memory. You have cold access the first time you read a variable, warm access when you read it again.

### How does an AMM price assets?

AMMs use liquidity pools, where users can deposit cryptocurrencies to provide liquidity. These pools then use algorithms to set token prices based on the ratio of assets in the pool. When a user wants to trade, they swap one token for another directly through the AMM, with prices determined by the pool's algorithm.

### What is a function selector clash in a proxy and how does it happen?

A function selector clash in a proxy occurs when two or more functions in the target contract have the same function signature, i.e., they have the same name and the same parameters.

### What is the effect on gas of making a function payable?

It will reduce the contract bytecode size and the amount of gas required to execute the function since the compiler does not check if ethers have been send with the call (call value).
It is possible to use this trick to save gas with the constructor and admin function, but not for all functions since these specifics functions will not be called by end-user, reducing the risk to send ethers by error.
It is general not recommended to make all functions payable since it can generate unexpected behavior and it reduces the readability of the code.

[More info on payable](https://dev.to/zaryab2000/does-adding-a-payable-keyword-in-solidity-actually-save-gas-24bm#:~:text=A%20very%20simple%20answer%20to,the%20amount%20of%20gas%20consumption)

### What is a signature replay attack?

Security issue where an attacker can reuse a valid signature to execute a transaction multiple times. This vulnerability can have various impacts depending on the context in which it is exploited.

[More info on replay attack](https://neptunemutual.com/blog/understanding-signature-replay-attack/)

### What is gas griefing?

Gas griefing occurs when a user sends the amount of gas required to execute the target smart contract but not enough to execute subcalls -- calls it makes to other contracts. If the contract does not check if the required gas to execute a subcall is available, the subcall will not execute as expected.

### How would you design a game of rock-paper-scissors in a smart contract such that players cannot cheat?

Essesintally following the commit and reveal pattern.
[This repoo explains this well] (https://github.com/ojroques/ethereum-rockpaperscissors/tree/master)

### What is the free memory pointer and where is it stored?

It is stored in the slot 0x40. This pointer contains the emplacement in memory which is free. Initially, it points to the address 0x80

### What function modifiers are valid for interfaces?

pure, view, external, override, payable.

### What is the difference between memory and calldata in a function argument?

Memory is used to hold temporary variables during function execution, while Calldata is used to hold function arguments passed in from an external caller. Calldata is read-only and cannot be modified by the function, while Memory can be modified.

### Describe the three types of storage gas costs.

Calldata => temporary data storage, read-only, the cheapest

memory => temporary data storage, read and write, gas cost between calldata and storage

storage => permanent data storage on the blockchain, read and write, most costly type of storage

### Why shouldn’t upgradeable contracts use the constructor?

If variables are initialied inside the constructor, the proxy has no way to see these values since :

The constructor is not stored in the runtime bytecode, but only in the creation bytecode.
The implementation contract is not deployed in the context of the proxy.
The solution is to use a public initialize function to initialize the proxy with the different values for each variable.
One exception to this is for immutable variable. Since this value is stored in the contract bytecode instead of the contract storage, you can use and initialize an immutable inside the constructor of the implementation contract

### What is the difference between UUPS and the Transparent Upgradeable Proxy pattern?

Contrary to the Diamond or the Beacon proxy, these two interfaces use the same system to delegate call and know the implementation.
But they have a big difference in the method to upgrade the implementation.
With the Transparent Proxy, the function to perform the upgrade is contained in the proxy, not the implementation.
While in the UUPS, the function is contained in the implementation contract.
This architecture makes the proxy cheaper, but can be more riskier because :
If you upgrade your proxy with a new implementation where the upgrade function does not exist, the proxy can not longer be upgraded.
Several vulnerabilities have targeted UUPS proxy where a self destruct function was available but not protected in the implementation contract…With the next Ethereum upgrade Dencun (2024) self destruct can no longer destroy a contract but the risk remains if others critical functions are present, see Wormhole Uninitialized Proxy Bugfix Review.

### If a contract delegatecalls an empty address or an implementation that was previously self-destructed, what happens? What if it is a regular call instead of a delegatecall?

If a contract delegatecalls to a self-destructed implementation, the delegatecall will return a success.
The reason is because the address still exists, even if the storage and the funds/balance have been cleared.
If it is the zero address, I am not totally sure, but I think it returns also a success, see this question on stackoverflow.
If the call is performed to an External Owned Account (EOA), since this is not a contract, there is no code and the call will returne True
For information, DELEGATECALLpushes 1 on the stack in case of success, and pushes 0 on the stack in case of an error.

For a call with call, if it is the address 0, the call will return a success. You can test this behavior with Foundry.
For a self destructed contract, the call will also return true but will not do anything since the code has been removed.

### What danger do ERC777 tokens pose?

An attacker can perform a reentrancy attack by setting a malicious contract as a hook when the attacker sends or receives tokens. For example to re-enter a withdrawfunction, which will send tokens to the attacker address. The token ERC-777 has to protect against it by adding a nonReentrant modifier to callbacks: _callTokensToSend  and _callTokensReceived. See ERC-777 callback issue
If a target contract allows making arbitrary calls to any address with any data, an attacker can leverage this to set a malicious contract to call each time the target contract receives or send tokens (see One more problem with ERC-777). The attacker can choose for example to revert each time resulting in an attack dos.

### According to the solidity style guide, how should functions be ordered?

Functions should be grouped according to their visibility and ordered:

constructor
receive function (if exists)
fallback function (if exists)
external
public
internal
private
Within a grouping, place the view and pure functions last.

### According to the solidity style guide, how should function modifiers be ordered?

The modifier order for a function should be:

Visibility
Mutability
Virtual
Override
Custom modifiers

### What is a bonding curve?

A bonding curve is a mathematical function connecting the supply of a digital asset with its value. In summary, the price of a token change regarding the token supply.

### How does safeMint differ from mint in the OpenZeppelin ERC721 implementation? 

The safeMint function will check if the destination contract can support ERC-721 token. If not, the call will revert

### What keywords are provided in Solidity to measure time?

— Suffixes like seconds, minutes, hours, days and weeks after literal numbers can be used to specify units of time where seconds are the base unit.

block.timestamp (uint): current block timestamp as seconds since unix epoch
The current block timestamp must be strictly larger than the timestamp of the last block.

According to the documentation, the only guarantee is that it will be somewhere between the timestamps of two consecutive blocks in the canonical chain. Nevertheless, I think this point concerned Ethereum before the merge (see next question).

### What is a sandwich attack?

A sandwich attack happens when an attacker, generally a bot, sends a transaction before and after a target transaction.
In general the goal is to take advantage of price movements to make a profit to the detriment of the issuer of the target transaction.

For example, if the target transaction is a buy order of a token C. The attacker will buy tokens before the transaction, making the price to move up, then selling the bought tokens after the target transaction. Thus we have this sequence:

1) Attacker buys token X, prices moves up 
2) Target buy tokens to an higher price than expected 
3) Attacker makes a profit by selling the tokens

There are two mains methods to protect against a sandwich attack
The slippage parameter to define a range of price
Use flashbot to avoid the mempool.

### If a delegatecall is made to a function that reverts, what does the delegatecall do?

Delegatecall will return false, it does not revert.

You have to manage this case in your contract. For example, OpenZeppelin for their proxy contract reverts in case of an error

```
switch result
	// delegatecall returns 0 on error.
	case 0 {
		revert(0, returndatasize())
}
```

### What is a gas efficient alternative to multiplying and dividing by a power of two?

We can use binary shift to do this because the shift right (shr) and shift left (shl) opcodes are cheaper than the opcodes used for the multiplication and division. Respectively, the binary shift opcodes cost 3 against 5 for the mul and div opcode.

### How large a uint can be packed with an address in one slot?

An address takes 20 bytes. Thus there are 12 bytes which are still free in this slot.
It can be used to put a uint256(1 bytes) and a uint4

### Which operations give a partial refund of gas?

With EIP-3529, SSTORE is the only operation that potentially provides a gas refund.
The refund is given (added into refund counter) when the storage value is set to zero from non-zero.
Remark: Initially, the operation self-destruct offered a refund of gas but this is no longer the case since the introduction of EIP-3529(London fork) which has removed this refund.

### What is ERC165 used for?

### If a proxy makes a delegatecall to A, and A does address(this).balance, whose balance is returned, the proxy's or A?

The returned balance is the proxy balance since the call is executed in the context of the proxy.

### What is a slippage parameter useful for?

Slippage refers to the difference between the expected price of a trade and the actual executed price due to market volatility or liquidity issues.
It can p.ex happens on a DEX with a large order or if the transaction is targeted by a sandwich attack.
A slippage parameter in a function allows to indicate the minimum amount of tokens that you want to be returned from a swap or another operation.

### What does ERC721A do to reduce mint costs? What is the tradeoff?

Enable minting multiple NFTs for essentially the same cost of minting a single NFT.

Optimization 1 - Removing duplicate storage from OpenZeppelin’s (OZ) ERC721Enumerable
Optimization 2 - updating the owner’s balance once per batch mint request, instead of per minted NFT
Optimization 3 - updating the owner data once per batch mint request, instead of per minted NFT

We can see these difference by comparing the function mint from OpenZeppelin and the function mint from ERC-721A. We can see for ERC-721A there is no tokenId and a new parameter quantity has been added in the parameters of the function.

### Why doesn't Solidity support floating point arithmetic?

Ethereum blockchain is deterministic which ensures that smart contracts always produce the same output for the same input.
With floating point number, you can have a loss of precision and a difference between node computation which is not compatible with the deterministic nature of Ethereum.

### What is TWAP?

A TWAP (Time Volume Weighted Average Price) is an oracle which returns the average price of an asset over a specific period.

To compute the average price, it will perform a sum up of each price for a certain amount of block and divide it by the number of blocks used. The formula can be summarized thus:
The goal of this oracle is to make more costly a flashhloan attack / price manipulation since a flashloan attack modifies the price of an asset inside the same block.
A attacker has to manipulate the price during a long period instead that just one block.

### How does Compound Finance calculate utilization?
```
Utilization = TotalBorrows / TotalSupply
```

### If a delegatecall is made to a function that reads from an immutable variable, what will the value be?
An immutable variable is stored in the bytecode of the implementation contract and it is this value which will be read.

With a proxy, you can change/upgrade the value of an immutable variable by upgrading to a new implementation.

### What is a fee-on-transfer token?

A “Fee on Transfer” token is a token that takes a percentage of internal commission upon transfer or trade. In other words, every time the token is transfered, a portion of the transfer amount is taken, e.g. to burn or sent to another address as a fee.

### What is a rebasing token?

A rebase, or elastic, token, is a token where the supply and the user’s balance is adjusted periodically. E.g any Liquidty stake token.
