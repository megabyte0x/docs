---
sidebar_position: 2
---
# Installation
Using `npm` package manager

```bash
npm i @biconomy/account 
```
OR

Using `yarn` package manager

```bash
yarn add @biconomy/account
```

In the following sections, you will learn more about the Bundler and Paymaster packages individually. In order to take full advantage of the account we need to install these two packages as well:

Using `npm` package manager

```bash
npm install @biconomy/account @biconomy/bundler @biconomy/common @biconomy/core-types @biconomy/paymaster
```

OR

Using `yarn` package manager

```bash
yarn add @biconomy/account @biconomy/bundler @biconomy/common @biconomy/core-types @biconomy/paymaster
```

Let’s take a look at each of these packages.
The account package will help you create Smart Contract Accounts - (SCAs) and interface with them to create transactions.
The ```bundler``` package helps you interact with our bundler or another bundler of your choice.
The ```paymaster``` package works similarly to the bundler package in that you can use our paymaster or any other one you choose.
The ```core-types``` package will give us Enums for the proper ChainId we may want to use
The ```common``` package is needed by our accounts package as another dependency.

## Smart Account instance configuration

| Key           | Description |
| ------------- | ------------- |
| signer        | This signer will be used for signing userOps for any transactions you build. You can supply your EOA wallet as a signer|
| chainId       | This represents the network your smart wallet transactions will be conducted on. Take a look at the [following](../../supportedchains/supportedchains.md) for supported chains |
| rpcUrl        | This represents the EVM node RPC URL you'll interact with, adjustable according to your needs. We recommend using some private node url for efficient ```userOp``` building|
| paymaster     | You can pass the same paymaster instance that you have built in the previous step. Alternatively, you can skip this if you are not interested in sponsoring transactions using Paymaster |
|               | Note: if you don't pass the Paymaster instance, your Smart Contract Account will need funds to pay for transaction fees.|
| bundler       | You can pass the same bundler instance that you have built in the previous step. Alternatively, you can skip this if you are only interested in building ```userOp```|


:::info
We are utilizing Ethers to pass the signer during initialization. However, it's worth mentioning that you have the flexibility to obtain the signer from various other SDKs. Some popular options include Web3.js, Wagmi React hooks, as well as authentication providers like Particle, Web3Auth, Magic, and many others.
:::

## Example Usage

```typescript
// This is how you create BiconomySmartAccount instance in your dapp's

import { BiconomySmartAccount, BiconomySmartAccountConfig } from "@biconomy/account"

// Note that paymaster and bundler are optional. You can choose to create new instances of this later and make account API use 
const biconomySmartAccountConfig: BiconomySmartAccountConfig = {
    signer: wallet.getSigner(),
    chainId: ChainId.POLYGON_MAINNET, 
    rpcUrl: '',
    // paymaster: paymaster, // check the README.md section of Paymaster package
    // bundler: bundler, // check the README.md section of Bundler package
}

const biconomyAccount = new BiconomySmartAccount(biconomySmartAccountConfig)
const biconomySmartAccount =  await biconomyAccount.init()

// native token transfer
// you can create any sort of transaction following same structure 
const transaction = {
  to: '0x85B51B068bF0fefFEFD817882a14f6F5BDF7fF2E',
  data: '0x',
  value: ethers.utils.parseEther('0.1'),
}

// building partialUserOp
const partialUserOp = await biconomySmartAccount.buildUserOp([transaction])

// using the paymaster package one can populate paymasterAndData to partial userOp. by default it is '0x'


```

```typescript
const userOpResponse = await smartAccount.sendUserOp(partialUserOp)
const transactionDetails = await userOpResponse.wait()
console.log("transaction details below")
console.log(transactionDetails)
```
Finally, we send the ```userOp``` and save the value to a variable named ```userOpResponse``` and get the ```transactionDetails``` after calling ```userOpResponse.wait()```

```typescript
const transactionDetails = await userOpResponse.wait()
console.log("transaction details below")
console.log(transactionDetails)
```
