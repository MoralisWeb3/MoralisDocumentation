---
description: >-
  Moralis Web3 API is a powerful way of getting all kinds of blockchain data.
  You can see all of the different functions documented in the sections below.
---

# Intro

As a blockchain developer, you have to be able to fetch different information such as block info, transaction info, NFT metadata, token prices, user balances, owner list of a particular NFT or token and so much more!

You find all of these powerful functions in the `Moralis.Web3API` namespace in the Moralis SDK.

{% embed url="https://www.youtube.com/watch?v=lX9A6yQXZ_8&ab_channel=MoralisWeb3" %}
Video demonstrating how the latest Web3 API can be accessed trought the SDK.
{% endembed %}

Moralis Web3 API can be easily accessed through the SDK functions as the video above demonstrates.

It's also possible to access all of this functionality through REST API from your own backend.

### Configuring Web3API

After you have initialized your application with `Moralis.start()`, it will automatically load the Web3API module. You don't need to make any additional steps to start using Web3API.

{% hint style="info" %}
[Initialize Moralis guide](https://docs.moralis.io/moralis-server/getting-started/connect-the-sdk#initialize-the-sdk)&#x20;
{% endhint %}

### Supported Chains&#x20;

All helper functions have a `chain` option to specify which blockchain to get data from. The following are the currently supported values for the chain option. Any of the "Lookup Values" will match the corresponding chain. If not specified the chain will default to Ethereum Mainnet.

| Chain                              | Lookup Values                                                           |
| ---------------------------------- | ----------------------------------------------------------------------- |
| Ethereum Mainnet                   | `eth`, `mainnet`, `0x1`                                                 |
| Ropsten (Ethereum Testnet)         | `testnet`, `ropsten`, `0x3`                                             |
| Rinkeby (Ethereum Testnet)         | `rinkeby`, `0x4`                                                        |
| Goerli (Ethereum Testnet)          | `goerli`, `0x5`                                                         |
| Kovan (Ethereum Testnet)           | `kovan`, `0x2a`                                                         |
| Binance Smart Chain Mainnet        | `bsc`, `binance`, `binance smart chain`, `0x38`                         |
| Binance Smart Chain Testnet        | `bsc testnet`, `binance testnet`, `binance smart chain testnet`, `0x61` |
| Polygon (Matic) Mainnet            | `matic`, `polygon`, `0x89`                                              |
| Mumbai (Matic Testnet)             | `mumbai`, `matic testnet`, `polygon testnet`, `0x13881`                 |
| Avalanche Mainnet                  | `avalanche`, `0xa86a `                                                  |
| Local Dev Chain (Ganache, Hardhat) | `ganache`, `hardhat`, `localdevchain`, `local devchain` ,`dev`, `0x539` |

### Web3 API return format

Almost all `Web3 API` function methods have a return format:

```javascript
{
  "total": 1,
  "page": 1,
  "page_size": 1,
  "result": Array(1)
}
```

###

###
