# 🔥DeFi (new)

## 🔥 getPairAddress (new)

Fetches and returns pair data of the provided token0+token1 combination (asynchronous).

The token0 and token1 options are interchangable (ie. there is no different outcome in `token0=WETH` and `token1=USDT` or `token0=USDT` and `token1=WETH`).

#### Options:

- `chain`(optional): The blockchain to get data from. Valid values are listed on the [supported chains page](https://docs.moralis.io/moralis-server/web3-sdk/supported-chains). Default value `Eth`.
- `to_date` (optional): Get the pair address to this date (any format that is accepted by momentjs) Provide the param 'to_block' or 'to_date' If 'to_date' and 'to_block' are provided, 'to_block' will be used.
- `to_block` (optional): To get the pair address at this block number
- `exchange` (required): The factory name or address of the token exchange. Available values : uniswapv2, uniswapv3, sushiswapv2, pancakeswapv2, pancakeswapv1, quickswap
- `token0_address` (required): Token0 address
- `token1_address` (required): Token1 address

{% tabs %}
{% tab title="JS" %}

```javascript
const options = {
  token0_address: "0xbb4cdb9cbd36b01bd1cbaebf2de08d9173bc095c",
  token1_address: "0xe9e7cea3dedca5984780bafc599bd69add087d56",
  exchange: "pancakeswapv2",
  chain: "bsc",
};
const pairAddress = await Moralis.Web3API.defi.getPairAddress(options);
```

{% endtab %}
{% tab title="React" %}

```javascript
import React from "react";
import { useMoralisWeb3Api } from "react-moralis";

const Web3Api = useMoralisWeb3Api();

const fetchPairAddress = async () => {
  const options = {
    token0_address: "0xbb4cdb9cbd36b01bd1cbaebf2de08d9173bc095c",
    token1_address: "0xe9e7cea3dedca5984780bafc599bd69add087d56",
    exchange: "pancakeswapv2",
    chain: "bsc",
  };
  const pairAddress = await Web3Api.defi.getPairAddress(options);
  console.log(pairAddress);
};
```

{% endtab %}
{% tab title="curl" %}

```sh
curl -X 'GET' \
  'https://deep-index.moralis.io/api/v2/0xbb4cdb9cbd36b01bd1cbaebf2de08d9173bc095c/0xe9e7cea3dedca5984780bafc599bd69add087d56/pairAddress?chain=bsc&exchange=pancakeswapv2' \
  -H 'accept: application/json' \
  -H 'X-API-Key: MY-API-KEY'
```

{% endtab %}
{% endtabs %}

#### Example result:

```javascript
{
  "token0": {
    "address": "0xbb4cdb9cbd36b01bd1cbaebf2de08d9173bc095c",
    "name": "Wrapped BNB",
    "symbol": "WBNB",
    "logo": null,
    "logo_hash": null,
    "thumbnail": null,
    "decimals": "18",
    "block_number": "8242108",
    "validated": 1
  },
  "token1": {
    "address": "0xe9e7cea3dedca5984780bafc599bd69add087d56",
    "name": "BUSD Token",
    "symbol": "BUSD",
    "logo": null,
    "logo_hash": null,
    "thumbnail": null,
    "decimals": "18",
    "block_number": "8242108",
    "validated": 1
  },
  "pairAddress": "0x58f876857a02d6762e0101bb5c46a8c1ed44dc16"
}
```

## 🔥 getPairReserves (new)

Get the liquidity reserves for a given pair address (asynchronous).&#x20;

#### Options:

- `chain`(optional): The blockchain to get data from. Valid values are listed on the [supported chains page](https://docs.moralis.io/moralis-server/web3-sdk/supported-chains). Default value `Eth`.
- `to_date` (optional): Get the reserves to this date (any format that is accepted by momentjs) Provide the param 'to_block' or 'to_date' If 'to_date' and 'to_block' are provided, 'to_block' will be used.
- `to_block` (optional): To get the reserves at this block number
- `pair_address` (required): Liquidity pair address

{% tabs %}
{% tab title="JS" %}

```javascript
const options = {
  pair_address: "0x58f876857a02d6762e0101bb5c46a8c1ed44dc16",
  chain: "bsc",
};
const reserves = await Moralis.Web3API.defi.getPairReserves(options);
```

{% endtab %}
{% tab title="React" %}

```javascript
import React from "react";
import { useMoralisWeb3Api } from "react-moralis";

const Web3Api = useMoralisWeb3Api();

const fetchTransactions = async () => {
  const options = {
    pair_address: "0x58f876857a02d6762e0101bb5c46a8c1ed44dc16",
    chain: "bsc",
  };
  const reserves = await Web3Api.defi.getPairReserves(options);
  console.log(reserves);
};
```

{% endtab %}
{% tab title="curl" %}

```sh
curl -X 'GET' \
  'https://deep-index.moralis.io/api/v2/0x58f876857a02d6762e0101bb5c46a8c1ed44dc16/reserves?chain=bsc' \
  -H 'accept: application/json' \
  -H 'X-API-Key: MY-API-KEY'
```

{% endtab %}
{% endtabs %}

#### Example result:

```javascript
{
  "reserve0": "412342701316335044600434",
  "reserve1": "156921226397095561306301741"
}
```
