# Token

## getTokenMetadata

Returns metadata (name, symbol, decimals, logo) for a given token contract address (asynchronous).

#### Options:

* `chain`(optional): The blockchain to get data from. Valid values are listed on the [intro page in the Transactions and Balances section](https://docs.moralis.io/transactions-and-balances/intro). Default value `Eth`.
* `addresses` (required): The address or an array of addresses to get metadata for

```javascript
//Get metadata for one token
const options = { chain: "bsc", addresses: "0xe...556" };
const tokenMetadata = await Moralis.Web3API.token.getTokenMetadata(options);


//Get metadata for an array of tokens
const options = { chain: "bsc", addresses: ["0xe...556", "0xe...742"] };
const tokenMetadata = await Moralis.Web3API.token.getTokenMetadata(options);
```

#### Example result:

```javascript
[
  {
    "address": "0x2d30ca6f024dbc1307ac8a1a44ca27de6f797ec22ef20627a1307243b0ab7d09",
    "name": "Kylin Network",
    "symbol": "KYL",
    "decimals": "18",
    "logo": "https://cdn.moralis.io/eth/0x67b6d479c7bb412c54e03dca8e1bc6740ce6b99c.png",
    "logo_hash": "ee7aa2cdf100649a3521a082116258e862e6971261a39b5cd4e4354fcccbc54d",
    "thumbnail": "https://cdn.moralis.io/eth/0x67b6d479c7bb412c54e03dca8e1bc6740ce6b99c_thumb.png",
    "block_number": "string",
    "validated": "string"
  }
]
```

## getTokenMetadataBySymbol

Returns metadata (name, address, decimals, logo) for given symbols (asynchronous).

#### Options:

* `chain`(optional): The blockchain to get data from. Valid values are listed on the [intro page in the Transactions and Balances section](https://docs.moralis.io/transactions-and-balances/intro). Default value `Eth`.
* `symbols` (required): The token symbol or an array of symbols to get metadata for

```javascript
//Get metadata for one token
const options = { chain: "bsc", symbols: "LINK" };
const tokenMetadata = await Moralis.Web3API.token.getTokenMetadataBySymbol(options);


//Get metadata for an array of tokens
const options = { chain: "bsc", symbols: ["LINK", "AAVE"] };
const tokenMetadata = await Moralis.Web3API.token.getTokenMetadataBySymbol(options);
```

#### Example result:

```javascript
[
  {
    "address": "0x2d30ca6f024dbc1307ac8a1a44ca27de6f797ec22ef20627a1307243b0ab7d09",
    "name": "Kylin Network",
    "symbol": "KYL",
    "decimals": "18",
    "logo": "https://cdn.moralis.io/eth/0x67b6d479c7bb412c54e03dca8e1bc6740ce6b99c.png",
    "logo_hash": "ee7aa2cdf100649a3521a082116258e862e6971261a39b5cd4e4354fcccbc54d",
    "thumbnail": "https://cdn.moralis.io/eth/0x67b6d479c7bb412c54e03dca8e1bc6740ce6b99c_thumb.png",
    "block_number": "string",
    "validated": "string"
  }
]
```

## getTokenAllowance

Returns the amount which the spender is allowed to withdraw from the spender (asynchronous).

#### Options:

* `chain`(optional): The blockchain to get data from. Valid values are listed on the [intro page in the Transactions and Balances section](https://docs.moralis.io/transactions-and-balances/intro). Default value `Eth`.
* `owner_address` (required): The address of the token owner
* `spender_address` (required): The address of the token spender
* `address`(required): The address of the token contract

```javascript
//Get token allowace on ETH
const options = {
  owner_address: "0xe...556",
  spender_address: "0xe...556",
  address: "0xe...556"
};
const allowance = await Moralis.Web3API.token.getTokenAllowance(options);
```

#### Example result:

```javascript
{
  "allowance": "string"
}
```

## getTokenPrice

Returns the price nominated in the native token and usd for a given token contract address (asynchronous).

#### Options:

* `chain`(optional): The blockchain to get data from. Valid values are listed on the [intro page in the Transactions and Balances section](https://docs.moralis.io/transactions-and-balances/intro). Default value `Eth`.
* `exchange`(optional): The factory name or address of the token exchange. Possible exchanges, for different chains are:\
  ETH mainnet: `uniswap-v3`, `sushiswap`, `uniswap-v2`\
  BSC mainnet: `pancakeswap-v2, pancakeswap-v1`\
  Polygon mainnet: `quickswap`\
  _If no `exchange` is specified, all exchanges are checked (in the order as listed above) until a valid pool has been found. Note that this request can take more time. So specifying the `exchange` will result in faster responses most of the time._
* `address`(required): The address of the token contract
* `to_block`(optional): Returns the price for a given blocknumber (historical price-data)

```javascript
//Get token price on PancakeSwap v2 BSC
const options = {
  address: "0x7...2",
  chain: "bsc",
  exchange: "PancakeSwapv2"
};
const price = await Moralis.Web3API.token.getTokenPrice(options);
```

#### Example result:

```javascript
{
  "nativePrice": {
    "value": "8409770570506626",
    "decimals": 18,
    "name": "Ether",
    "symbol": "ETH"
  },
  "usdPrice": 19.722370676,
  "exchangeAddress": "0x1f98431c8ad98523631ae4a59f267346ea31f984",
  "exchangeName": "Uniswap v3"
}
```

## getAllTokenIds

Returns an object with number of NFTs and an array with NFT metadata (name, symbol) for a given token contract address (asynchronous).

#### Options:

* `chain`(optional): The blockchain to get data from. Valid values are listed on the [intro page in the Transactions and Balances section](https://docs.moralis.io/transactions-and-balances/intro). Default value `Eth`.
* `address`(required): The address of the token contract.

```javascript
const options = { address: "0xd...07", chain: "bsc" };
const NFTs = await Moralis.Web3API.token.getAllTokenIds(options);
```

#### Example result:

```javascript
[
  {
    "token_address": "0xdf7952b35f24acf7fc0487d01c8d5690a60dba07",
    "token_id": "414742",
    "contract_type": "ERC721",
    "token_uri": "https://ipfs.moralis.io:2053/ipfs/QmYsTqbmGA3H5cgouCkh8tswJAQE1AsEko9uBZX9jZ3oTC/twinkle.json",
    "metadata": "{\n\t\"name\": \"Twinkle\",\n\t\"description\": \"Three guesses what's put that twinkle in those eyes! (Hint: it's CAKE)\",\n\t\"image\": \"ipfs://QmYD9AtzyQPjSa9jfZcZq88gSaRssdhGmKqQifUDjGFfXm/twinkle.png\",\n\t\"attributes\": {\n\t\t\"bunnyId\": \"7\"\n\t}\n}",
    "synced_at": null,
    "amount": null,
    "name": "Pancake Bunnies",
    "symbol": "PB"
  }
]
```

## getNFTMetadata

Returns the contract level metadata (name, symbol, base token uri) for the given contract (asynchronous).

#### Options:

* `chain`(optional): The blockchain to get data from. Valid values are listed on the [intro page in the Transactions and Balances section](https://docs.moralis.io/transactions-and-balances/intro). Default value `Eth`.
* `address`(required): The address of the token contract.

```javascript
const options = { address: "0xd...07", chain: "bsc" };
const metaData = await Moralis.Web3API.token.getNFTMetadata(options);
```

{% hint style="info" %}
Requests for contract addresses not yet indexed will automatically start the indexing process for that NFT collection
{% endhint %}

#### Example result:

```javascript
{
  token_address: "0xdf7952b35f24acf7fc0487d01c8d5690a60dba07",
  name: "Pancake Bunnies",
  symbol: "PB",
  abi: null,
  contract_type: "ERC721",
  supports_token_uri: null,
  synced_at: "2021-09-16",
  
}
```

## getNFTOwners

Returns an object with number of NFT ownsers and an array with NFT metadata (name, symbol) for a given token contract address (asynchronous).

#### Options:

* `chain`(optional): The blockchain to get data from. Valid values are listed on the [intro page in the Transactions and Balances section](https://docs.moralis.io/transactions-and-balances/intro). Default value `Eth`.
* `format` (optional): The format of the token id. Available values : `decimal`, `hex`. Default value : `decimal.`
* `offset `(optional): offset.
* `limit`(optional): limit.
* `order `(optional): The field(s) to order on and if it should be ordered in ascending or descending order.
* `address`(required): Address of the contract

```javascript
const options = { address: "0xd...07", chain: "bsc" };
const nftOwners = await Moralis.Web3API.token.getNFTOwners(options);
```

The field(s) to order on and if it should be ordered in ascending or descending order. Specified by: `fieldName1.order`,` fieldName2.order`.

```javascript
//Example 1: "name". Example for name: "name.ASC" or "name.DESC"
const options = { address: "0xd...07", order: "name.ASC" };
const nftOwners = await Moralis.Web3API.token.getNFTOwners(options);

//Example 2: "Name and Symbol"
const options = { address: "0xd...07", order: "name.ASC,symbol.DESC" };
const nftOwners = await Moralis.Web3API.token.getNFTOwners(options);
```

{% hint style="info" %}
Make sure to include a sort parm on a column like token\_id for consistent pagination results
{% endhint %}

{% hint style="info" %}
Requests for contract addresses not yet indexed will automatically start the indexing process for that NFT collection
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

## searchNFTs

Very powerful and fast tool for getting the NFT data based on a metadata search (asynchronous).

#### Options:

* `chain`(optional): The blockchain to get data from. Valid values are listed on the [intro page in the Transactions and Balances section](https://docs.moralis.io/transactions-and-balances/intro). Default value `Eth`.
* `format` (optional): The format of the token id. Available values : `decimal`, `hex`. Default value : `decimal.`
* `offset `(optional): offset.
* `limit`(optional): limit.
* `q `(required): The search string parameter
* `filter`(required): What fields the search should match on. To look into the entire metadata set the value to `global`. To have a better response time you can look into a specific field like name. Available values : `name`; `description`; `attributes`; `global`; `name,description`; `name,attributes`; `description,attributes`; `name,description,attributes`

```javascript
const options = { q: "Pancake", chain: "bsc", filter: "name" };
const NFTs = await Moralis.Web3API.token.searchNFTs(options);
```

#### Example result:

```javascript
[
  {
    "token_id": "124436",
    "token_address": "0x3afa102b264b5f79ce80fed29e0724f922ba57c7",
    "token_uri": "https://ipfs.moralis.io:2053/ipfs/QmVAD8v4s2SXF8FgjePqMdQ2GV5hE2isZnzxcrA36XcSDA/metadata.json",
    "metadata": "{\"name\":\"Pancake\",\"description\":\"The dessert series 1\",\"image\":\"ipfs://QmNQFXCZ6LGzvpMW9Q5PWbCrEnLknQrPwr2r8pbQAgzQ9A/4863BD6B-6C92-4B96-BF80-8020B2F7C3A5.jpeg\"}",
    "contract_type": "ERC721",
    "token_hash": "d03fe436e972bf9215d7bb8c64c4c556",
    "synced_at": null,
    "created_at": "2021-09-19T10:36:16.610Z"
  }
]
```

#### Serching by `global`:

```javascript
const options = { q: "bored ape", chain: "bsc", filter: "global" };
const NFTs = await Moralis.Web3API.token.searchNFTs(options);
```

#### Serching by `description,attributes`:

```javascript
const options = { q: "loves bananas", chain: "bsc", filter: "description,attributes" };
const NFTs = await Moralis.Web3API.token.searchNFTs(options);
```

## getContractNFTTransfers

Returns an object with number of NFT transfers and an array with NFT transfers for a given token contract address (asynchronous).

#### Options:

* `chain`(optional): The blockchain to get data from. Valid values are listed on the [intro page in the Transactions and Balances section](https://docs.moralis.io/transactions-and-balances/intro). Default value `Eth`.
* `format` (optional): The format of the token id. Available values : `decimal`, `hex`. Default value is `decimal.`
* `offset `(optional): offset.
* `limit`(optional): limit.
* `order `(optional): The field(s) to order on and if it should be ordered in ascending or descending order.
* `address`(required): Address of the contract

```javascript
const options = { address: "0xd...07", chain: "bsc" };
const nftTransfers = await Moralis.Web3API.token.getContractNFTTransfers(options);
```

The field(s) to order on and if it should be ordered in ascending or descending order. Specified by: `fieldName1.order`,` fieldName2.order`.

```javascript
//Exmaple 1 "block_number": "block_number.ASC", "block_number.DESC", 
const options = { address: "0xd...07", order: "name.ASC" };
const nftTransfers = await Moralis.Web3API.token.getContractNFTTransfers(options);

//Example 2 "block_number and contract_type": "block_number.ASC,contract_type.DESC"
const options = { 
  address: "0xd...07", 
  order: "block_number.ASC, contract_type.DESC"
}
const nftTransfers = await Moralis.Web3API.token.getContractNFTTransfers(options);
```

#### Example result:

```javascript
[
  {
    "token_address": "0xdf7952b35f24acf7fc0487d01c8d5690a60dba07",
    "token_id": "191123",
    "from_address": "0x0000000000000000000000000000000000000000",
    "to_address": "0xccbc6f1df01bad0f477fe085760571572d115eed",
    "amount": "1",
    "contract_type": "ERC721",
    "block_number": "6827111",
    "block_timestamp": "2021-04-23T22:25:35.000Z",
    "block_hash": "0x79b8618df3d5b62ee4f230386284ea78037bc61f36418488db71564d2aca583c",
    "transaction_hash": "0x9dedbb0fa63eba77634564ef6682826ccfdd1cb58be6e7b2f44cdf0aada07733",
    "transaction_type": "Single",
    "transaction_index": 129,
    "log_index": 415
  }
]
```

## getTokenIdMetadata

Returns data, including fully resolved metadata for the given token id of the given contract address (asynchronous).

#### Options:

* `chain`(optional): The blockchain to get data from. Valid values are listed on the [intro page in the Transactions and Balances section](https://docs.moralis.io/transactions-and-balances/intro). Default value `Eth`.
* `format` (optional): The format of the token id. Available values : `decimal`, `hex`. Default value is `decimal.`
* `address`(required): Address of the contract
* `token_id` (required): The id of the token

```javascript
const options = { address: "0xd...07", token_id: "1", chain: "bsc" };
const tokenIdMetadata = await Moralis.Web3API.token.getTokenIdMetadata(options);
```

#### Example result:

```javascript
{
  "token_address": "0xdf7952b35f24acf7fc0487d01c8d5690a60dba07",
  "token_id": "222",
  "contract_type": "ERC721",
  "token_uri": "ipfs://QmYu9WwPNKNSZQiTCDfRk7aCR472GURavR9M1qosDmqpev/sparkle.json",
  "metadata": "{\n    \"name\": \"Sparkle\",\n    \"description\": \"It’s sparkling syrup, pancakes, and even lottery tickets! This bunny really loves it.\",\n    \"image\": \"ipfs://QmXdHqg3nywpNJWDevJQPtkz93vpfoHcZWQovFz2nmtPf5/sparkle.png\",\n    \"attributes\": {\n        \"bunnyId\": \"4\"\n    }\n}",
  "synced_at": "2021-08-19T10:49:00.324Z",
  "amount": "1",
  "name": "Pancake Bunnies",
  "symbol": "PB"
}
```

## getTokenIdOwners

Returns an object with number of NFT transfers and an array with all owners of NFT items within a given contract collection (asynchronous).

#### Options:

* `chain`(optional): The blockchain to get data from. Valid values are listed on the [intro page in the Transactions and Balances section](https://docs.moralis.io/transactions-and-balances/intro). Default value `Eth`.
* `format` (optional): The format of the token id. Available values : `decimal`, `hex`. Default value is `decimal`
* `offset `(optional): offset.
* `limit`(optional): limit.
* `order `(optional): The field(s) to order on and if it should be ordered in ascending or descending order.
* `address`(required): Address of the contract.
* `token_id`(requierd): The id of the token.

```javascript
const options = { address: "0xd...07", token_id: "1", chain: "bsc" };
const tokenIdOwners= await Moralis.Web3API.token.getTokenIdOwners(options);
```

The field(s) to order on and if it should be ordered in ascending or descending order. Specified by: `fieldName1.order`, `fieldName2.order`.

```javascript
//Example 1: "name", "name.ASC", "name.DESC"
const options = { address: "0xd...07", order: "name.DESC", token_id: "1" };
const nftTransfers = await Moralis.Web3API.token.getTokenIdOwners(options);

//Example 2: "Block and Contract Type", "block_number.ASC, contract_type.DESC" 
const options = { 
  address: "0xd...07", 
  order: "block_number.ASC, contract_type.DESC", 
  token_id: "1"
}
const nftTransfers = await Moralis.Web3API.token.getTokenIdOwners(options);
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

## getWalletTokenIdTransfers

Returns an object with number of NFT transfers and an array with all transfers of NFT by token id (asynchronous).

#### Options:

* `chain`(optional): The blockchain to get data from. Valid values are listed on the [intro page in the Transactions and Balances section](https://docs.moralis.io/transactions-and-balances/intro). Default value `Eth`.
* `format` (optional): The format of the token id. Available values : `decimal`, `hex`. Default value is `decimal`
* `offset `(optional): offset.
* `limit`(optional): limit.
* `order `(optional): The field(s) to order on and if it should be ordered in ascending or descending order.
* `address`(required): Address of the contract.
* `token_id`(requierd): The id of the token.

```javascript
const options = { address: "0xd...07", token_id: "1", chain: "bsc" };
const transfers = await Moralis.Web3API.token.getWalletTokenIdTransfers(options);
```

The field(s) to order on and if it should be ordered in ascending or descending order. Specified by: `fieldName1.order`, `fieldName2.order`.

```javascript
//Example 1: "name", "block_number.ASC", "block_number.DESC"
const options = { address: "0xd...07", order: "block_number.DESC", token_id: "1" };
const transfers = await Moralis.Web3API.token.getContractNFTTransfers(options);

//Example 2: "Block and Contract Type", "block_number.ASC, contract_type.DESC" 
const options = { 
  address: "0xd...07", 
  order: "block_number.ASC, contract_type.DESC",
  token_id: "1"
}
const transfers = await Moralis.Web3API.token.getWalletTokenIdTransfers(options);
```

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

## 🔥 getNFTTrades&#x20;

Returns an object with NFT trades for a given contract and marketplace

#### Options:

* `chain`(optional): The blockchain to get data from. Valid values are listed on the [intro page in the Transactions and Balances section](https://docs.moralis.io/transactions-and-balances/intro). Default value `Eth`.
* `from_date` (optional): The date from where to get the trades(any format that is accepted by momentjs). Provide the param 'from\_block' or 'from\_date' If 'from\_date' and 'from\_block' are provided, 'from\_block' will be used.
* `to_date` (optional): Get the trades to this date (any format that is accepted by momentjs). Provide the param 'to\_block' or 'to\_date' If 'to\_date' and 'to\_block' are provided, 'to\_block' will be used.
* `from_block` (optional): The minimum block number from where to get the tradesProvide the param 'from\_block' or 'from\_date' If 'from\_date' and 'from\_block' are provided, 'from\_block' will be used.
* `to_block` (optional): The maximum block number from where to get the trades. Provide the param 'to\_block' or 'to\_date' If 'to\_date' and 'to\_block' are provided, 'to\_block' will be used.
* `offset`(optional): Offset.
* `limit`(optional): Limit.
* `marketplace` (optional): Marketplace from where to get the trades (only opensea is supported at the moment).
* `address` (required): Address of the contract(i.e. `0x1a2b3x...`).

```javascript
const options = { address: "0xd...07", limit: "10", chain: "bsc" };
const NFTTrades = await Moralis.Web3API.token.getNFTTrades(options);
```

#### Example result:

```javascript
[
  {
    "transaction_hash": "0x4de0bcef1450492bd5c2e7693cf644c40005868d0dcc8a7a50a80ef2efa88d1e",
    "transaction_index": "164",
    "token_ids": [
      "16404"
    ],
    "seller_address": "0xbae90f486d751f133702655627ce599249cd26b8",
    "buyer_address": "0x8795e90de359c1e0bf2579646486f7f12f270d2f",
    "token_address": "0xdf7952b35f24acf7fc0487d01c8d5690a60dba07",
    "marketplace_address": "0x7be8076f4ea4a4ad08075c2508e481d6c946d12b",
    "price": "280000000000000000",
    "price_token_address": null,
    "block_timestamp": "2021-05-09T23:00:25.000Z",
    "block_number": "7281522",
    "block_hash": "0xe870c197b0c614e055f4de5b264bc7c69eafc93a6d0ce300309de444b2ff7e3a"
  },
  
]
```

## 🔥 getNFTLowestPrice&#x20;

Returns an object with the lowest price found for a NFT token contract for the last x days (only trades paid in ETH)

#### Options:

* `chain`(optional): The blockchain to get data from. Valid values are listed on the [intro page in the Transactions and Balances section](https://docs.moralis.io/transactions-and-balances/intro). Default value `Eth`.
* `days` (optional): The number of days to look back to find the lowest price If not provided 7 days will be the default
* `marketplace` (optional): Marketplace from where to get the trades (only opensea is supported at the moment).
* `address` (required): Address of the contract(i.e. `0x1a2b3x...`).

```javascript
const options = { address: "0xd...07", days: "3" };
const NFTLowestPrice = await Moralis.Web3API.token.getNFTLowestPrice(options);
```

#### Example result:

```javascript
[
  {
    "token_ids": [
      "15",
      "54"
    ],
    "from_address": "0x057Ec652A4F150f7FF94f089A38008f49a0DF88e",
    "to_address": "0x057Ec652A4F150f7FF94f089A38008f49a0DF88e",
    "value": "1000000000000000",
    "gas": "6721975",
    "gas_price": "20000000000",
    "receipt_cumulative_gas_used": "1340925",
    "receipt_gas_used": "1340925",
    "block_number": "88256",
    "block_timestamp": "2021-06-04T16:00:15",
    "transaction_hash": "0x057Ec652A4F150f7FF94f089A38008f49a0DF88e",
    "transaction_index": "string"
  }
]
```
