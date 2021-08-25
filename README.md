
## What we will be building

We'll be building, deploying, and connecting to a couple of basic smart contracts:

- A contract for creating and updating a message on the Ethereum blockchain.
- A contract for minting tokens, which allows the owner of the contract to send tokens to others and to read the token balances, and lets owners of the new tokens also send them to others.

We will also build out a React front end that will allow a user to:

- Read the greeting from the contract deployed to the blockchain
- Update the greeting
- Send the newly minted tokens from their address to another address
- Once someone has received tokens, allow them to also send their tokens to someone else
- Read the token balance from the contract deployed to the blockchain

### Ethereum development environment

Hardhat is an Ethereum development environment and framework designed for full stack development, and it's the framework that I will be using for this small project.

In my opinion, it's better than the traditional tools in the ecosystem, which are Ganache and Truffle.

### Ethereum Web Client Library

In our React app, we will need a way to interact with the smart contracts that have been deployed. We will need a way to read for data as well as send new transactions.

I will be using Ethers.js, instead of the traditional web3.js

### How to get started with create-react-app

To get started, we'll create a new React application:

```
npx create-react-app react-dapp
```

Next, change into the new directory and install ethers.js and hardhat

```
npm install ethers hardhat @nomiclabs/hardhat-waffle ethereum-waffle chai @nomiclabs/hardhat-ethers
```

initialize a new Ethereum Development Environment with Hardhat:

```
npx hardhat
```

### How to Read and Write to the Ethereum Blockchain

When writing or initializing a transaction, you have to pay for the transaction to be written to the blockchain. To make this work, you need to pay gas which is the fee or price required to successfully conduct a transaction and execute a contract on the Ethereum blockchain.

As long as you are only reading from the blockchain and not changing or updating anything, you don't need to carry out a transaction and there will be no gas or cost to do so. The function you call is then carried out only by the node you are connected to, so you don't need to pay any gas and the read is free.

From our React app, we will interact with the smart contract using a combination of the ethers.js library, the contract address, and the ABI that will be created from the contract by Hardhat.

What is an ABI? ABI stands for application binary interface. You can think of it as the interface between your client-side application and the Ethereum blockchain where the smart contract you are going to be interacting with is deployed.

ABIs are typically compiled from Solidity smart contracts by a development framework like Hardhat. You can also often find the ABIs for a smart contract on Etherscan

### How to compile the ABI

Run the following command:

```
npx hardhat compile
```

Now, you should see a new folder named artifacts in the src directory. The artifacts/contracts/Greeter.json file contains the ABI as one of the properties. When we need to use the ABI, we can import it from our JavaScript file:

```
import Greeter from './artifacts/contracts/Greeter.sol/Greeter.json'
```

We can then reference the ABI like this:

```
console.log("Greeter ABI: ", Greeter.abi)
```

TODO: differences between web3.js and ethers.js

### How to Deploy and Use a Local Network / Blockchain

To deploy to the local network, you first need to start the local test node:

```
npx hardhat node
```

This creates 20 test accounts and addresses for us that we can use to deploy and test our smart contracts. Each account is also loaded up with 10,000 fake Ether.

