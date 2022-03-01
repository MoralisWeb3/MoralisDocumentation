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

When sending an ERC721 token you have to specify the `contractAddress `of the NFT and the specific `tokenId `you are interested in.

As it's only possible to transfer one ERC721 at a time, no `amount `is needed.

```javascript
// sending a token with token id = 1
const options = {type: "erc721",  
                 receiver: "0x..",
                 contractAddress: "0x..",
                 tokenId: 1}
let transaction = await Moralis.transfer(options)
```

## Transferring ERC1155 Tokens (Semi-Fungible)

When sending an ERC1155 token you have to specify the `contractAddress `of the NFT and the specific`tokenId`you are interested in. Additionally, you have to specify the`amount`of the token you want to transfer.

```javascript
// sending 15 tokens with token id = 1
const options = {type: "erc1155",  
                 receiver: "0x..",
                 contractAddress: "0x..",
                 tokenId: 1,
                 amount: 15}
let transaction = await Moralis.transfer(options)
```

### Resolving the results

`Moralis.transfer()` returns a [transaction response](https://docs.ethers.io/v5/api/providers/types/#providers-TransactionResponse). This object contains all data about the transaction. If you need data about the **result** of the transation, then you need to wait for the transaction to be **confirmed**.

You can do this via `transaction.wait()` to wait for 1 confirmation (or `transaction.wait(5)` to wait for 5 confirmations for example):

```javascript
const transaction = await Moralis.transfer(options)
const result = await transaction.wait()
```
