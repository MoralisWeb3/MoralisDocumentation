# Account

## getTransactions

Returns the transactions from the current user or address. Returns an object with the number of transactions  and the array of native transactions (asynchronous).

#### Options:

* `chain`(optional): The blockchain to get data from. Valid values are listed on the [intro page in the Transactions and Balances section](https://docs.moralis.io/transactions-and-balances/intro). Default value `Eth`.
* `address` (optional): A user address (i.e. `0x1a2b3x...`). If specified, the user attached to the query is ignored and the address will be used instead.
* `from_date` (optional): The date from where to get the transactions (any format that is accepted by momentjs). Provide the param 'from_block' or 'from_date' If 'from_date' and 'from_block' are provided, 'from_block' will be used.
* `to_date` (optional):  Get the transactions to this date (any format that is accepted by momentjs). Provide the param 'to_block' or 'to_date' If 'to_date' and 'to_block' are provided, 'to_block' will be used.
* `from_block` (optional): The minimum block number from where to get the transactions Provide the param 'from_block' or 'from_date' If 'from_date' and 'from_block' are provided, 'from_block' will be used.
* `to_block` (optional): The maximum block number from where to get the transactions. Provide the param 'to_block' or 'to_date' If 'to_date' and 'to_block' are provided, 'to_block' will be used.
* `offset`(optional): Offset.
* `limit`(optional): Limit

```javascript
// get mainnet transactions for the current user
const transactions = await Moralis.Web3API.account.getTransactions();

// get BSC transactions for a given address
// with most recent transactions appearing first
const options = { chain: "bsc", address: "0x...", order: "desc", from_block: "0" };
const transactions = await Moralis.Web3API.account.getTransactions(options);
```

#### Example result:

```javascript
[
  {
    "hash": "0x057Ec652A4F150f7FF94f089A38008f49a0DF88e",
    "nonce": "326595425",
    "transaction_index": "25",
    "from_address": "0xd4a3BebD824189481FC45363602b83C9c7e9cbDf",
    "to_address": "0xa71db868318f0a0bae9411347cd4a6fa23d8d4ef",
    "value": "650000000000000000",
    "gas": "6721975",
    "gas_price": "20000000000",
    "input": "string",
    "receipt_cumulative_gas_used": "1340925",
    "receipt_gas_used": "1340925",
    "receipt_contract_address": "0x1d6a4cf64b52f6c73f201839aded7379ce58059c",
    "receipt_root": "string",
    "receipt_status": "1",
    "block_timestamp": "2021-04-02T10:07:54.000Z",
    "block_number": "12526958",
    "block_hash": "0x0372c302e3c52e8f2e15d155e2c545e6d802e479236564af052759253b20fd86"
  }
]
```

## getNativeBalance

Returns native balance for a specific address (asynchronous).

#### Options:

* `chain`(optional): The blockchain to get data from. Valid values are listed on the [intro page in the Transactions and Balances section](https://docs.moralis.io/transactions-and-balances/intro). Default value `Eth`.
* `address` (optional): A user address (i.e. `0x1a2b3x...`). If specified, the user attached to the query is ignored and the address will be used instead.
* `to_block `(optional): The block number on which the balances should be checked.
* `address` (required): The address for which the native balance will be checked.

```javascript
// get mainnet native balance for the current user
const balance = await Moralis.Web3API.account.getNativeBalance();

// get BSC native balance for a given address
const options = { chain: "bsc", address: "0x...", to_block: "1234" };
const balance = await Moralis.Web3API.account.getNativeBalance(options);
```

#### Example result:

```javascript
{
  "balance": "1234567890"
}
```

## getTokenBalances

Retrieve all token balances of a current user or specified address.  Returns an object with the number of tokens and the array of token objects (asynchronous).

#### Options:

* `chain`(optional): The blockchain to get data from. Valid values are listed on the [intro page in the Transactions and Balances section](https://docs.moralis.io/transactions-and-balances/intro). Default value `Eth`.
*  `address` (optional): A user address (i.e. `0x1a2b3x...`). If specified, the user attached to the query is ignored and the address will be used instead.
* `to_block` (optional): The block number on which the balances should be checked

```javascript
const balances = await Moralis.Web3API.account.getTokenBalances();
```

Without any parameters specified, it defaults to 'Eth' as chain and the current user, but you can also specify the `chain`  and `address`in an options object:

```javascript
const options = { chain: 'bsc', address: "0x...", to_block: "10253391" }
const balances = await Moralis.Web3API.account.getTokenBalances(options);
```

#### Example result:

```javascript
[
  {
    "token_address": "0x2d30ca6f024dbc1307ac8a1a44ca27de6f797ec22ef20627a1307243b0ab7d09",
    "name": "Kylin Network",
    "symbol": "KYL",
    "logo": "https://cdn.moralis.io/eth/0x67b6d479c7bb412c54e03dca8e1bc6740ce6b99c.png",
    "thumbnail": "https://cdn.moralis.io/eth/0x67b6d479c7bb412c54e03dca8e1bc6740ce6b99c_thumb.png",
    "decimals": "18",
    "balance": "123456789"
  }
]
```

## getTokenTransfers

Get ERC20 token transfers from the current user or address. Returns an object with the number of token transfers and the array of token transfers (asynchronous).

#### Options:

* `chain`(optional): The blockchain to get data from. Valid values are listed on the [intro page in the Transactions and Balances section](https://docs.moralis.io/transactions-and-balances/intro). Default value `Eth`.
* `address` (optional): A user address (i.e. `0x1a2b3x...`). If specified, the user attached to the query is ignored and the address will be used instead.
* `from_date` (optional): The date from where to get the transactions (any format that is accepted by momentjs). Provide the param 'from_block' or 'from_date' If 'from_date' and 'from_block' are provided, 'from_block' will be used.
* `to_date` (optional):  Get the transactions to this date (any format that is accepted by momentjs). Provide the param 'to_block' or 'to_date' If 'to_date' and 'to_block' are provided, 'to_block' will be used.
* `from_block` (optional): The minimum block number from where to get the transactions Provide the param 'from_block' or 'from_date' If 'from_date' and 'from_block' are provided, 'from_block' will be used.
* `to_block` (optional): The maximum block number from where to get the transactions. Provide the param 'to_block' or 'to_date' If 'to_date' and 'to_block' are provided, 'to_block' will be used.
* `offset`(optional): Offset.
* `limit`(optional): Limit

```javascript
// get mainnet transfers for the current user
const userTrans = await Moralis.Web3API.account.getTokenTransfers();

// get BSC transfers for a given address
// with most recent transfers appearing first
const options = { chain: "bsc", address: "0x...", from_block: "0" };
const transfers = await Moralis.Web3API.account.getTokenTransfers(options);
```

#### Example result:

```javascript
[
  {
    "transaction_hash": "0x2d30ca6f024dbc1307ac8a1a44ca27de6f797ec22ef20627a1307243b0ab7d09",
    "address": "0x057Ec652A4F150f7FF94f089A38008f49a0DF88e",
    "block_timestamp": "2021-04-02T10:07:54.000Z",
    "block_number": "12526958",
    "block_hash": "0x0372c302e3c52e8f2e15d155e2c545e6d802e479236564af052759253b20fd86",
    "to_address": "0x62AED87d21Ad0F3cdE4D147Fdcc9245401Af0044",
    "from_address": "0xd4a3BebD824189481FC45363602b83C9c7e9cbDf",
    "value": "650000000000000000"
  }
]
```

## getNFTs

Get all NFTs from the current user or address. Supports both ERC721 and ERC1155. Returns an object with the number of NFT objects and the array of NFT objects (asynchronous).

#### Options:

* `chain`(optional): The blockchain to get data from. Valid values are listed on the [intro page in the Transactions and Balances section](https://docs.moralis.io/transactions-and-balances/intro). Default value `Eth`.
*  `address` (optional): A user address (i.e. `0x1a2b3x...`). If specified, the user attached to the query is ignored and the address will be used instead.

```javascript
// get NFTs for current user on Mainnet
const userEthNFTs = await Moralis.Web3API.account.getNFTs();

// get testnet NFTs for user
const testnetNFTs = await Moralis.Web3API.account.getNFTs({ chain: 'ropsten' });

// get polygon NFTs for address
const options = { chain: 'matic', address: '0x...' };
const polygonNFTs = await Moralis.Web3API.account.getNFTs(options);
```

#### Example result:

```javascript
[
  {
    "token_address": "0x057Ec652A4F150f7FF94f089A38008f49a0DF88e",
    "token_id": "15",
    "contract_type": "ERC721",
    "owner_of": "0x057Ec652A4F150f7FF94f089A38008f49a0DF88e",
    "block_number": "88256",
    "block_number_minted": "88256",
    "token_uri": "string",
    "metadata": "string",
    "synced_at": "string",
    "amount": "1",
    "name": "CryptoKitties",
    "symbol": "RARI"
  }
]
```

## getNFTTransfers

Get the NFT transfers. Returns an object with the number of NFT transfers and the array of NFT transfers (asynchronous).

#### Options:

* `chain`(optional): The blockchain to get data from. Valid values are listed on the [intro page in the Transactions and Balances section](https://docs.moralis.io/transactions-and-balances/intro). Default value `Eth`.
* `format` (optional): The format of the token id. Available values : `decimal`, `hex`. Default value : `decimal.`
* `offset`(optional): Offset.
* `direction`(optional): The transfer direction. Available values : `both`, `to`, `from` . Default value : `both`.
* `limit`(optional): Limit.
* `order `(optional): The field(s) to order on and if it should be ordered in ascending or descending order.
* `address` (optional): A user address (i.e. `0x1a2b3x...`). If specified, the user attached to the query is ignored and the address will be used instead.

```javascript
// get mainnet NFT transfers for the current user
const transfersNFT = await Moralis.Web3API.account.getNFTTransfers();

// get BSC NFT transfers for a given address
// with most recent transactions appearing first
const options = { chain: "bsc", address: "0x...", limit: "10" };
const transfersNFT = await Moralis.Web3API.account.getNFTTransfers(options);
```

{% hint style="info" %}
Use the token_address param to get results for a specific contract only.

Note results will include all indexed NFTs.

Any request which includes the token_address param will start the indexing process for that NFT collection the very first time it is requested.
{% endhint %}

#### Example result:

```javascript
[
  {
    "token_address": "0x057Ec652A4F150f7FF94f089A38008f49a0DF88e",
    "token_id": "15",
    "from_address": "0x057Ec652A4F150f7FF94f089A38008f49a0DF88e",
    "to_address": "0x057Ec652A4F150f7FF94f089A38008f49a0DF88e",
    "amount": "1",
    "contract_type": "ERC721",
    "block_number": "88256",
    "block_timestamp": "2021-06-04T16:00:15",
    "block_hash": "string",
    "transaction_hash": "0x057Ec652A4F150f7FF94f089A38008f49a0DF88e",
    "transaction_type": "string",
    "transaction_index": "string",
    "log_index": 0
  }
]
```

## getNFTsForContract

Returns an object with the NFT count for the specified contract and an NFT array belonging to the given address for the specified contract (asynchronous).

#### Options:

* `chain`(optional): The blockchain to get data from. Valid values are listed on the [intro page in the Transactions and Balances section](https://docs.moralis.io/transactions-and-balances/intro). Default value `Eth`.
* `format` (optional): The format of the token id. Available values : `decimal`, `hex`. Default value : `decimal.`
* `offset`(optional): Offset.
* `limit`(optional): Limit.
* `order`(optional): The field(s) to order on and if it should be ordered in ascending or descending order.
* `address` (optional): The owner of a given token (i.e. `0x1a2b3x...`). If specified, the user attached to the query is ignored and the address will be used instead.
* `token_address`(required): Address of the contract

```javascript
const options = { chain: 'matic', address: '0x...', token_address: '0x...' };
const polygonNFTs = await Moralis.Web3API.account.getNFTsForContract(options);
```

{% hint style="info" %}
Use the token_address param to get results for a specific contract only.

Note results will include all indexed NFTs.

Any request which includes the token_address param will start the indexing process for that NFT collection the very first time it is requested.
{% endhint %}

#### Example result:

```javascript
[
  {
    "token_address": "0x057Ec652A4F150f7FF94f089A38008f49a0DF88e",
    "token_id": "15",
    "contract_type": "ERC721",
    "owner_of": "0x057Ec652A4F150f7FF94f089A38008f49a0DF88e",
    "block_number": "88256",
    "block_number_minted": "88256",
    "token_uri": "string",
    "metadata": "string",
    "synced_at": "string",
    "amount": "1",
    "name": "CryptoKitties",
    "symbol": "RARI"
  }
]
```
