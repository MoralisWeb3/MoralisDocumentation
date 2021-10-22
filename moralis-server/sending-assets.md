---
description: >-
  Transferring assets using Moralis SDK is very easy. Sending native assets
  (ETH/BSC/MATIC etc), ERC20 tokens or NFTs is just one line of code.
---

# 💸 Sending ETH, Tokens and NFTs

## Why Moralis SDK for transferring

Sending assets using bare-bones low-level libraries like Web3.js is surprisingly difficult. Developers are required to know the ABI of specific smart contracts in order to initiate token and NFT transactions.

Moralis SDK removes all this complexity. Transferring assets with Moralis SDK is just one line of code! 🧙

{% embed url="https://www.youtube.com/watch?v=suZYvqrc_Hg&ab_channel=MoralisWeb3" %}
Tutorial explaining how to transfer ETH, tokens and NFTs with 1 line of code with Moralis SDK.
{% endembed %}

## Transferring Native Asset (ETH/BNB/MATIC etc)

In order to transfer native assets of the blockchain (ETH on Ethereum, BNB on Binance Smart Chain, etc) you have to construct an`options`object where you set`type:native`to tell Moralis that you are sending the native asset (and not an ERC20 token or NFT).

Next, you have to tell Moralis the amount you want to transfer, to do that you need to use `Moralis.Units.ETH` (even when sending on BSC or Polygon) and specify the amount as a string denominated in ETH. `Moralis.Units.ETH`is a helper function that will convert your sting to wei which is required in order to construct the transaction.

Finally, you have to specify the`receiver`of the funds. You do so by specifying the address as a string as demonstrated below.

```javascript
// sending 0.5 ETH
const options = {type: "native", amount: Moralis.Units.ETH("0.5"), receiver: "0x.."}
let result = await Moralis.transfer(options)
```

## Transferring ERC20 Tokens

When sending an ERC20 you need to know the contract address of the token and the number of decimals.&#x20;

```javascript
// sending 0.5 tokens with 18 decimals
const options = {type: "erc20", 
                 amount: Moralis.Units.Token("0.5", "18"), 
                 receiver: "0x..",
                 contractAddress: "0x.."}
let result = await Moralis.transfer(options)
```

The contract address and the number of decimals for a token can normally be found on Etherscan or on the website of the project.

By using `Moralis.Units.Token`helper function you can multiply the amount you want to send by the number of decimals. This is required to construct an ERC20 transaction.

If you need to programmatically get the address and decimals of a token you can use Moralis SDK [Moralis.Web3API.token.getTokenMetadataBySymbol()](https://docs.moralis.io/moralis-server/web3-sdk/token#gettokenmetadatabysymbol).

## Transferring ERC721 Tokens (Non-Fungible)

When sending an ERC721 token you have to specify the `contactAddress `of the NFT and the specific `tokenId `you are interested in.

As it's only possible to transfer one ERC721 at a time, no `amount `is needed.

```javascript
// sending a token with token id = 1
const options = {type: "erc721",  
                 receiver: "0x..",
                 contractAddress: "0x..",
                 tokenId: 1}
let result = await Moralis.transfer(options)
```

## Transferring ERC1155 Tokens (Semi-Fungible)

When sending an ERC1155 token you have to specify the `contactAddress `of the NFT and the specific`tokenId`you are interested in. Additionally, you have to specify the`amount`of the token you want to transfer.

```javascript
// sending 15 tokens with token id = 1
const options = {type: "erc1155",  
                 receiver: "0x..",
                 contractAddress: "0x..",
                 tokenId: 1,
                 amount: 15}
let result = await Moralis.transfer(options)
```

## 🔥Callbacks Promises Events (new)

Moralis.transfer() supports promise events. It allows you to get info on different stages of your interaction with the blockchain.&#x20;

For example, you can instantly get a `transactionHash` without awaitng the entire transaction has been processed. And then after the transaction is processed, you can receive a `receipt`

To receive all data as event callbacks, you need to switch `chawaitReceipt` to `false` in your  transaction options.

```javascript
const txOptions = {
  type: "erc20",
  amount: Moralis.Units.Token("10", "18"),
  receiver: "0xB5...ee035",
  contractAddress: "0x7b...605e",
  awaitReceipt: false // should be switched to false
}

const tx = await Moralis.transfer(txOptions );

tx.on("transactionHash", (hash) => { ... })
  .on("receipt", (receipt) => { ... })
  .on("confirmation", (confirmationNumber, receipt) => { ... })
  .on("error", (error) => { ... });

```
