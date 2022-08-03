---
description: >-
  Moralis makes building cross-chain dapps a breeze - here's how.
---

# Cross Chain Support

## Intro

Moralis supports multiple chains (see full list [here](https://docs.moralis.io/moralis-dapp/web3-api/supported-chains)), which means any dapp built with Moralis is cross-chain by default. This empowers you to make cross-chain dapps quickly with minimal code. No more rewriting everything to support each chain you want to deploy to.

## How To Make a Cross-Chain Dapp

Making a cross-chain dapp with Moralis is easy. Start by [creating your own server](https://docs.moralis.io/moralis-dapp/getting-started/create-a-moralis-dapp), select the chains you want your dapp to deploy on, and... that's it! A quick check of the docs, a few lines of code, and your dapp will have access to all cross-chain user data.

## Examples 

Here are some examples of using Moralis to fetch address data from multiple chains. For these quick examples, we're going to use vanilla JavaScript.

### Fetching Native Balance 

```javascript
const chains = ["eth", "bsc", "polygon", "avalanche"]; //List of chains we want data from
let options = {
    address: "0xea674fdde714fd979de3edf0f56aa9716b898ec8", 
    chain: ""
}
//Loop through chains and get native balance for each one
for (let i = 0; i < chains.length; i++) {
    options.chain = chains[i];
    const balance = await Moralis.Web3API.account.getNativeBalance(options);
    console.log(`${options.chain}: ${Moralis.Units.FromWei(balance.balance, 18)}`); //Print it out nicely, converting it from Wei
}
```

#### Example Result

```
eth: 708.896026219224464098
bsc: 3.15282393
polygon: 0
avalanche: 0.314258184458559963
```

### Fetching ERC20 Token Balances

```javascript
const chains = ["eth", "bsc", "polygon", "avalanche"]; //List of chains we want data from
let options = {
    address: "0xea674fdde714fd979de3edf0f56aa9716b898ec8",
    chain: ""
}
//Loop through chains and get a list of erc20 tokens for each one
for (let i = 0; i < chains.length; i++) {
    options.chain = chains[i];
    console.log(await Moralis.Web3API.account.getTokenBalances(options));
}
```

#### Example Result

An array of objects of ERC20 tokens for every chain, example of one from ETH:

```javascript
{
    balance: "10000000000000000",
    decimals: 18,
    logo: "https://cdn.moralis.io/eth/0x514910771af9ca656af840dff83e8264ecf986ca.png",
    name: "Chain Link",
    symbol: "LINK",
    thumbnail: "https://cdn.moralis.io/eth/0x514910771af9ca656af840dff83e8264ecf986ca_thumb.png",
    token_address: "0x514910771af9ca656af840dff83e8264ecf986ca"
}
```

### Fetching NFTs 

```javascript
const chains = ["eth", "bsc", "polygon", "avalanche"]; //List of chains we want data from
let options = {
    address: "0x47024aa3900e331d9948cd71c5559722297e0a18",
    chain: ""
}
//Loop through chains and get a list of NFTs for each one
for (let i = 0; i < chains.length; i++) {
    options.chain = chains[i];
    console.log(await Moralis.Web3API.account.getNFTs(options));
}
```

#### Example Result

An array of objects of NFTs for every chain, example of one from ETH: 

```javascript 
{
    amount: "1",
    block_number: "15042725",
    block_number_minted: "15042725",
    contract_type: "ERC721",
    last_metadata_sync: "2022-07-21T10:21:13.079Z",
    last_token_uri_sync: "2022-06-29T02:09:37.703Z",
    metadata: "string",
    name: "Poor Dudes",
    owner_of: "0x47024aa3900e331d9948cd71c5559722297e0a18",
    symbol: "DUDES",
    token_address: "0x6ce9ee25a3942baab219c2ee6fadcb8abefaee52",
    token_hash: "adeac37fc8e621fde418ead003d36d38",
    token_id: "8338",
    token_uri: "string",
    updated_at: "1656468574.953",
}
```
