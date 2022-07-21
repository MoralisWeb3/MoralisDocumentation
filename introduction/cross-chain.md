---
description: >-
  Moralis offers and enables you to make cross chain dapps easier than ever, here is how
---

# Cross Chain Support

## Intro

Moralis supports multiple chains (see [full list](https://github.com/MoralisWeb3/MoralisDocumentation)), which enables you to make your dapp cross chain, by adding very little code and not having to rewrite everything when wanting to support another chain.

## How To

To make a cross chain dapp using Moralis, just [create your own server](https://docs.moralis.io/moralis-dapp/getting-started/create-a-moralis-dapp), selecting the chains you want your dapp to be available on when creating the Moralis dapp. And that is all! All that is left is exploring the docs and a few lines of code and you have all data for your users cross chain.


## Examples 

Here are some examples using Moralis to fetch data about an address from multiple chains. For the purpose of these quick examples, we are going to use vanilla JavaScript.

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
    amount: "1"
    block_number: "15185192"
    block_number_minted: "15185192"
    contract_type: "ERC721"
    last_metadata_sync: "2022-07-21T09:49:21.033Z"
    last_token_uri_sync: "2022-07-21T09:49:10.267Z"
    metadata: "{\"dna\":\"2a9b0c466f3818a835ae31ea64ccef2b81a33231\",\"name\":\"WoP #1768\",\"description\":\"World of Pride\",\"image\":\"https://wop.fra1.digitaloceanspaces.com/images/1768.png\",\"edition\":1768,\"date\":1655975706548,\"Team\":\"WoP\",\"attributes\":[{\"trait_type\":\"background\",\"value\":\"stripes right\"},{\"trait_type\":\"special\",\"value\":\"none\"},{\"trait_type\":\"body\",\"value\":\"light\"},{\"trait_type\":\"features\",\"value\":\"none\"},{\"trait_type\":\"wear down\",\"value\":\"skirt with feathers\"},{\"trait_type\":\"hair\",\"value\":\"the bob\"},{\"trait_type\":\"wear up\",\"value\":\"short tank top\"},{\"trait_type\":\"accessories\",\"value\":\"pois earrings\"},{\"trait_type\":\"beard\",\"value\":\"none\"},{\"trait_type\":\"mouth\",\"value\":\"smile red\"},{\"trait_type\":\"glasses\",\"value\":\"cat eye\"}],\"compiler\":\"WoP Compiler\"}"
    name: "World of Pride"
    owner_of: "0x47024aa3900e331d9948cd71c5559722297e0a18"
    symbol: "WoP"
    token_address: "0x73868abd52b00f55124a5a823d7ece28ab9de6e2"
    token_hash: "9abfdbf3067a8cbd2ec0ee5ed98af9d8"
    token_id: "1768"
    token_uri: "https://ipfs.moralis.io:2053/ipfs/QmeXtZ1P1QihbsYWvBprLp8mwcaEfes3NeuXJ7BMjJu5S7/1768.json"
    updated_at: "1658396947.209"
}
```