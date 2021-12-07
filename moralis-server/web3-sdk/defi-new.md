# 🔥DeFi (new)

## 🔥 getPairAddress (new)

Fetches and returns pair data of the provided token0+token1 combination (asynchronous).

The token0 and token1 options are interchangable (ie. there is no different outcome in `token0=WETH` and `token1=USDT` or `token0=USDT` and `token1=WETH`).

#### Options:

* `chain`(optional): The blockchain to get data from. Valid values are listed on the [intro page in the Transactions and Balances section](https://docs.moralis.io/transactions-and-balances/intro). Default value `Eth`.
* `to_date` (optional):  Get the pair address to this date (any format that is accepted by momentjs) Provide the param 'to\_block' or 'to\_date' If 'to\_date' and 'to\_block' are provided, 'to\_block' will be used.
* `to_block` (optional): To get the pair address at this block number
* `exchange` (required): The factory name or address of the token exchange. Available values : uniswapv2, uniswapv3, sushiswapv2, pancakeswapv2, pancakeswapv1, quickswap
* `token0_address` (required): Token0 address
* `token1_address` (required): Token1 address

```javascript
const options = {
  token0_address: "0xbb4cdb9cbd36b01bd1cbaebf2de08d9173bc095c",
  token1_address: "0xe9e7cea3dedca5984780bafc599bd69add087d56",
  exchange: "pancakeswapv2",
  chain: "bsc"
 };
const pairAddress = await Moralis.Web3API.defi.getPairAddress(options);
```

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

* `chain`(optional): The blockchain to get data from. Valid values are listed on the [intro page in the Transactions and Balances section](https://docs.moralis.io/transactions-and-balances/intro). Default value `Eth`.
* `to_date` (optional):  Get the reserves to this date (any format that is accepted by momentjs) Provide the param 'to\_block' or 'to\_date' If 'to\_date' and 'to\_block' are provided, 'to\_block' will be used.
* `to_block` (optional): To get the reserves at this block number
* `pair_address` (required): Liquidity pair address

```javascript
const options = {
  address: "0xa527a61703d82139f8a06bc30097cc9caa2df5a6",
  chain: "bsc"
 };
const reserves = await Moralis.Web3API.defi.getPairReserves(options);
```

#### Example result:

```javascript
{
  "reserve0": "168322120697847448277111",
  "reserve1": "7119912071113173800533"

```
