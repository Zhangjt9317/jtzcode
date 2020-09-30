---
title: Ethereum Blockchain Development
date: 2020-09-16 23:07:16
tags:
---

# Introduction 

Introduction to multiple components necessary for Ethereum blockchain app development, including smart contracts, tool (Remix), language (Solidity), and others.


Learning Resources:
  - [awesome blockchain](https://github.com/yjjnls/awesome-blockchain): a very good summary of dev and concepts

## Tools and Installation

1. IDE
   1. Local
      1. VsCode  -- for local IDE
      2. Bracket -- for text editor
   2. Online
      1. Remix   -- online IDE
2. Truffle Suite -- sweet tools for smart contracts
   1. Ganache 
   2. Drizzle
   3. Truffle
3. IPFS Desktop
4. Wallets and core (optional)
   1. Go-eth
   2. Ethereum Wallet
   3. Bitcoin core 

Both wallets and Bitcoin core eat up a lot of internal storage on your local machine, I tend to avoid them. Testing smart contracts online is pretty good based on my personal experinece. I use Remix extensively in this case, and it is preferrable to develop applications on local machine since it can be an web or desktop app.

## Smart Contracts and Solidity Introduction


## Smart Contract Lifecycle and Layout

**Smart contract lifecycle:**
1. Develop Contract 
   1. write the contract and compile
2. Deployed 
   1. once deployed, the smart contract is un-editable, after deploying on any network (e.g. mainnet, testnet), you can get an address specific to where contract was deployed
3. Invoked
   1. Use the address cerated to invoke contract to call functions on it
4. Self-Destruction
   1. self destruct after so no one can call functions on it anymore. 

![Smart contract lifecycle](smart_contract.png)

![contract_compiling](contract_compiling.png)


### Invoking a Smart Contract

* Online Wallet (like an Ethereum Wallet)
* Desktop Wallet
* Hardware Wallet (Ledger, Trezor)
* Dapp
  * A way to intreact with the Blockchain and the smart contracts using Web3 Interface
  * Web3 is a programmatic way to interact with the Blockchain


![solidity code structure](code_structure.png)


### Remix Online Editor 

* Used for Writing / Compiling / Deploying / Invoking a contract
* Test contracts in a simulator
* Deploy contracts to the live networks
* Connect to a live node on your computer
* 3 Environment options 
  * Javascript VM (an in-browser ETH network, used for testing)
    * very fast, no mining
  * Injected Web3 (used in conjunction with Metamask to deploy to public networks)
    * Metamask injects and exposes Web3 and ETH accounts into Chrome
  * Web3 Provider (used to connect to a locally running node)
    * For connecting to a local running node through Geth or Truffle
    * provide the URL of the locally running node

![solidity_memory](solidity_memory.png)


### Memory Management in Solidity

Memory in Solidity is divided into 3 types:

* memory 
* storage
* callData

![solidity_memory](solidity_memory.png)

```
contract DataLocation{
    //Always in storage
    uint count;
    uint[] points;

    function localVariables(){
        //This will give a warning, and ask you to put the ord storage if you don't say storage, it will create in storage

        uint[] localArray;
        uint[] memory memoryArray;

        //Creates a reference to a storage array
        uint[] pointer = points;
    }
}
```

### Solidity Elementary Data Types

* uint - Use for unsigned integers of various sizes
* int - Use for signed integers of various sizes
* bool - The possible values are constants true and false
* address - Represents address of externally owned account or contract account. Holds a 20 byte hex string of an Ethereum address. It is a Value Type.

* uint8 variables can only hold upto a bytes = 0 - 255
* the size of an integer can be set in 8 bit increments
* int256 = int
* the size of the address type variable is 20 bytes
