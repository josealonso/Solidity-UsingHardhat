
## What we will be building

We'll be building, deploying, and connecting to a couple of basic smart contracts:

- A contract for creating and updating a message on the Ethereum blockchain.

We will also build out a React front end that will allow a user to:

- Read the greeting from the contract deployed to the blockchain
- Update the greeting

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

### Run unit tests

```
npx hardhat test
```

(Section taken from official hardhat guides)
### TypeScript Support

Hardhat will automatically enable its TypeScript support if your config file ends in .ts and is written in valid TypeScript. This requires a few changes to work properly.

#### Installing dependencies

Hardhat uses TypeScript and ts-node under the hood, so you need to install them. To do it, open your terminal, go to your Hardhat project, and run:

```
npm install --save-dev ts-node typescript
```

To be able to write your tests in TypeScript, you also need these packages:

```
npm install --save-dev chai @types/node @types/mocha @types/chai
```

#### TypeScript configuration

Now, we are going to rename the config file from hardhat.config.js to hardhat.config.ts, just run:

```
mv hardhat.config.js hardhat.config.ts
```

We need to apply three changes to your config for it to work with TypeScript:

1. Plugins must be loaded with *import* instead of *require*.
2. You need to explicitly import the Hardhat config functions, like *task*.
3. If you are defining tasks, they need to access the *Hardhat Runtime Environment* explicitly, as a parameter.

For example, the sample project's config turns from this

```
require("@nomiclabs/hardhat-waffle");

// This is a sample Hardhat task. To learn how to create your own go to
// https://hardhat.org/guides/create-task.html
task("accounts", "Prints the list of accounts", async (taskArgs, hre) => {
  const accounts = await hre.ethers.getSigners();

  for (const account of accounts) {
    console.log(await account.address);
  }
});

// You need to export an object to set up your config
// Go to https://hardhat.org/config/ to learn more

/**
 * @type import('hardhat/config').HardhatUserConfig
 */
module.exports = {
  solidity: "0.7.3"
};
```

into this

```
import { task } from "hardhat/config";
import "@nomiclabs/hardhat-waffle";

// This is a sample Hardhat task. To learn how to create your own go to
// https://hardhat.org/guides/create-task.html
task("accounts", "Prints the list of accounts", async (args, hre) => {
  const accounts = await hre.ethers.getSigners();

  for (const account of accounts) {
    console.log(await account.address);
  }
});

// You need to export an object to set up your config
// Go to https://hardhat.org/config/ to learn more

export default {
  solidity: "0.7.3"
};
```

You also need to create a *tsconfig.json* file. Here's a template you can base yours on:

```
{
  "compilerOptions": {
    "target": "es2018",
    "module": "commonjs",
    "strict": true,
    "esModuleInterop": true,
    "outDir": "dist"
  },
  "include": ["./scripts", "./test"],
  "files": ["./hardhat.config.ts"]
}
```

You can use different settings, but please make sure your Hardhat config file is included. The easiest way of doing this is by keeping its path in the *files* array.

And that's really all it takes. Now you can write your config, tests, tasks and scripts in TypeScript.

#### Writing tests and scripts in TypeScript

To write your smart contract tests and scripts you'll most likely need access to an Ethereum library to interact with your smart contracts. This will probably be one of hardhat-ethers
(opens new window) or hardhat-web3

(opens new window), all of which inject instances into the Hardhat Runtime Environment.

When using JavaScript, all the properties in the HRE are injected into the global scope, and are also available by getting the HRE explicitly. When using TypeScript nothing will be available in the global scope and you will need to import everything explicitly.

An example for tests:

```
import { ethers } from "hardhat";
import { Signer } from "ethers";

describe("Token", function () {
  let accounts: Signer[];

  beforeEach(async function () {
    accounts = await ethers.getSigners();
  });

  it("should do something right", async function () {
    // Do something with the accounts
  });
});
```

An example for scripts:
```
import { run, ethers } from "hardhat";

async function main() {
  await run("compile");

  const accounts = await ethers.getSigners();

  console.log(
    "Accounts:",
    accounts.map((a) => a.address)
  );
}

main()
  .then(() => process.exit(0))
  .catch((error) => {
    console.error(error);
    process.exit(1);
  });
```

#### Type-safe smart contract interactions

If you want to type-check smart contract interactions (calling methods, reading events), use *@typechain/hardhat*. It generates typing files (*.d.ts) based on ABI's, and it requires little to no configuration when used with Hardhat.

#### Type-safe configuration

One of the advantages of using TypeScript, is that you can have a type-safe configuration, and avoid typos and other common errors.

To do that, you have to write your config in this way:
```
import { HardhatUserConfig } from "hardhat/config";

const config: HardhatUserConfig = {
  // Your type-safe config goes here
};

export default config;
```

