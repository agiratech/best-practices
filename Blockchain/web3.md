# Web3.js
    This document is intended to give a clear cut view on best practices that needs to be followed using Web3.js 
    A javascript based library used for integrating with deployed smart contract. 
    - Reference links [Web3.js Tutorial](https://web3js.readthedocs.io/en/v1.10.0/).
    
    
## Table of Contents:

* [Introduction](#introduction)
* [Installation](#installation)
* [Modules](#modules)
* [Creating an instance and fetching data](#instance)
* [Contract Initialization](#contract-initialization)
* [Creating Read functions](#read-function)
* [Creating Write functions](#write-function)

## Introduction

```Web3. js``` is a collection of libraries that allow developers to interact with a remote or local Ethereum node using HTTP, IPC, or WebSocket. Using this library, you can develop websites or clients that interact with the blockchain.

```Note:``` Web3.js is mainly used for Frontend integration and for backend we can go with ether.js

Web3.js is well-suited for frontend applications because it has excellent browser support, and integrates well with popular frontend frameworks.


## Installation

```npm:``` npm install web3
```yarn:``` yarn add web3
```pure js:``` link the dist/web3.min.js


## Modules

Frequently used modules:

```[web3-eth]``` - is for the EVM based blockchain. It allows you to perform various Ethereum-related operations, such as sending transactions, interacting with smart contracts, and querying blockchain data.

```[web3-utils]``` - contains useful helper functions for Dapp developers. Some of the helper functions like conversion (from wei to ether)

This modules are used rarely

```[web3-shh]``` - is for the whisper protocol, to communicate p2p and broadcast.
```[web3-bzz]``` - is for the swarm protocol, the decentralized file storage.

## Creating an instance and fetching data

We need to create a ```web3 instance``` and set a provider.
Most Ethereum-supported browsers like MetaMask have an EIP-1193 compliant provider available at ```window.ethereum```.

Create a seperate file called ```Web3Services.js```

eg:
```javascript
export const detectCurrentProvider = (conn_provider = 'metamask') => {
  let provider;
  if (window.ethereum) {
    provider = window.ethereum;
  }
  if (provider &&
    provider.providers &&
    provider.providers.length &&
    conn_provider === 'metamask') {
    let m_provider = window.ethereum.providers.find((x) => x.isMetaMask);
    if (m_provider) provider = m_provider;
  }
  return provider;
};

// main: initiate the metamask wallet (used it in header part)
export const initiateNetwork = async (provider) => {
  try {
    const currentProvider = detectCurrentProvider(provider);
    if (currentProvider) {
      await currentProvider.request({ method: 'eth_requestAccounts' });
      const web3 = new Web3(currentProvider);
      const userAccount = await web3.eth.getAccounts();
      const chainId = await web3.eth.getChainId();
      const networkId = await web3.eth.net.getId();

      if (userAccount.length === 0) {
        return new Error('Please connect to Metamask');
      } else {
        const account = userAccount[0];
        let balance = await web3.eth.getBalance(account); // Get wallet balance
        balance = web3.utils.fromWei(balance, 'ether'); //Convert balance to wei

        return { balance, account, chainId, networkId };
      }
    } else return new Error('Non-Ethereum browser detected. You should consider trying Metamask')
  } catch (err) {
    console.log(err);
    return new Error(
      'There was an error fetching your accounts. Make sure your Ethereum client is configured correctly.'
    );
  }
}
```
We can import the initiate function in the frontend code to connect with metamask or any other wallet and fetch the data.

## Contract Initialization

For contract initialization we need to get two parameters
    ```Contract address```
    ```Contract ABI```
     
```Contract ABI```
- The Contract ABI is a JSON (JavaScript Object Notation) representation of the contract's functions, their input parameters, and output types. It defines how to encode function calls and decode function responses for communication with the contract.

- We need to add the contract address in ```.env``` file and save the ABI as json file and import it in the ```Web3Services.js``` file.

eg:
```javascript
import Web3 from 'web3';
import registerabi from './abi/registerabi.json';

let isRegisterInitialized = false;
let registerContract;

// common: initiate Register contract

export const initiateRegisterContract = async (conn_provider) => {
  let provider = detectCurrentProvider(conn_provider)

  if (provider) {
    const web3 = new Web3(provider);
    registerContract = new web3.eth.Contract(
      registerabi,
      process.env.REACT_APP_REGISTER_CONTRACT
    );
      // console.log("Address", process.env.REACT_APP_REGISTER_CONTRACT)
    isRegisterInitialized = true;
  }
}
```
This will detect the detect the current provider and initiate the contract

## Creating Read functions

In this example it will call the read function of the smart contract and get the data.

eg:

```javascript
export const verifyUserSetup = async (conn_provider, <Parameters received from web2>) => {

  if (!isRegisterInitialized) {
    await initiateRegisterContract(conn_provider)
  }
  return await registerContract.methods.userVerify(<Need to pass the parameters received from web2 to web3 for calling the read function>)
    .call()
    .then((useStatus) => {
      console.log(useStatus, "useStatus")
      return useStatus
    })
}
```

- It will check whether the contract is initialized or not. If the contract was not initialized then it will initiate the contract and call the ```userVerify``` method inside the registration contract and fetch the data.

## Creating Write functions

In this example it will call the write function of the smart contract and get the data.

The write function ```requires gas fee```, so we need to connect to a wallet and process the transactions.

```javascript
export const addUserSetup = async (conn_provider, w_address, firstName, lastName, dob, phoneNo, walletAddress) => {
    
    let provider = detectCurrentProvider(conn_provider);
    
    if (!isRegisterInitialized) {
      await initiateRegisterContract(conn_provider)
    }
    try {
      
      if (provider) {
          return await registerContract.methods.addUser({
          firstName: firstName,
          lastName: lastName,
          dob: dob,
          phoneNumber: phoneNo,        
          walletAddress: walletAddress,
        }).send({
          from: w_address
        })
      }
    } catch (error) {
      console.log("Error",error)
    }
  }
```

It will connect to the ```from: wallet address``` for processing the transaction. 

It will also check whether the contract is initialized or not. If the contract was not initialized then it will initiate the contract and call the ```addUser``` method inside the registration contract and add the data into the Smart contract.