# Moralis Units

## Intro 

All crypto transactions are made in the smallest value - Wei. But to improve the user experience, on the frontend the transfer amounts are usually inputed in an `ETH `style. Users want to input value `0.0001 TokenA` instead of `100000000000000 Wei TokenA`. Standard web3 methods are inconvenient for easy converting. The most convenient conversion method is to use the `Moralis.Units` helper functions.

{% hint style="info" %}
Wei is the smallest denomination of ether, and you should always make calculations in Wei and convert only for display reasons.
{% endhint %}

## Converting ERC20 Token to Wei

To convert ERC20 token to Wei, you need to specify the amount of tokens and number of decimals. 

```javascript
//Example: We want to convert 0.5 BUSD. It has 18 decimals
const busdInWei = Moralis.Units.Token("0.5", "18")
// expected output: 500000000000000000 Wei
```

The number of decimals for a token can normally be found on Etherscan, on the website of the project or from Moralis API.

## Converting Native Asset (ETH/BNB/MATIC etc) to Wei

To convert native asset to Wei, you need to specify the amount of native crypto.

```javascript
//Example: We want to convert 0.5 ETH to Wei
const ethInWei = Moralis.Units.ETH("0.5")
// expected output: 500000000000000000 Wei
```

## Converting token value from Wei

All token values are shown in Wei. If you want to display token values in the "Eth" style, you can use: 

```javascript
//Convert token value to ETH style with 6 decimals
const tokenValue = Moralis.Units.FromWei("2000000000000000000", 6)

//Convert token value to ETH style with 18 decimals
//If you do not specify decimals, 18 decimals will be automatically used
const tokenValue = Moralis.Units.FromWei("2000000000000000000")
```

Web3 API responses for token balances have **decimals **and **balance **in Wei fields:

```javascript
  { 
    "token_address": "0x...b0",
    "name": "name",
    "symbol": "NAME",
    "logo": null,
    "thumbnail": null,
    "decimals": "18", 
    "balance": "2000000000000000000"
  },
```

{% hint style="info" %}
Wei is the smallest denomination of ether, and you should always make calculations in Wei and convert only for display reasons.
{% endhint %}
