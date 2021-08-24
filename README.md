
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

