---
description: >-
  You can get the content of any block in past, or address events based on block
  number.
---

# Native

## getBlock

Retrieve the contents of a block by block hash.  Returns a block object (asynchronous). 

#### Options:

* `chain`(optional): The blockchain to get data from. Valid values are listed on the [intro page in the Transactions and Balances section](https://docs.moralis.io/transactions-and-balances/intro). Default value `Eth`.
*  `block_number_or_hash` (required): The block hash or block number.

```javascript
const options = { chain: "bsc", block_number_or_hash: "2" };

// get block content on BSC
const transactions = await Moralis.Web3API.native.getBlock(options);
```

#### Example result:

```javascript
{
  "timestamp": "2021-05-07T11:08:35.000Z",
  "number": "12386788",
  "hash": "0x9b559aef7ea858608c2e554246fe4a24287e7aeeb976848df2b9a2531f4b9171",
  "parent_hash": "0x011d1fc45839de975cc55d758943f9f1d204f80a90eb631f3bf064b80d53e045",
  "nonce": "0xedeb2d8fd2b2bdec",
  "sha3_uncles": "0x1dcc4de8dec75d7aab85b567b6ccd41ad312451b948a7413f0a142fd40d49347",
  "logs_bloom": "0xdde5fc46c5d8bcbd58207bc9f267bf43298e23791a326ff02661e99790da9996b3e0dd912c0b8202d389d282c56e4d11eb2dec4898a32b6b165f1f4cae6aa0079498eab50293f3b8defbf6af11bb75f0408a563ddfc26a3323d1ff5f9849e95d5f034d88a757ddea032c75c00708c9ff34d2207f997cc7d93fd1fa160a6bfaf62a54e31f9fe67ab95752106ba9d185bfdc9b6dc3e17427f844ee74e5c09b17b83ad6e8fc7360f5c7c3e4e1939e77a6374bee57d1fa6b2322b11ad56ad0398302de9b26d6fbfe414aa416bff141fad9d4af6aea19322e47595e342cd377403f417dfd396ab5f151095a5535f51cbc34a40ce9648927b7d1d72ab9daf253e31daf",
  "transactions_root": "0xe4c7bf3aff7ad07f9e80d57f7189f0252592fee6321c2a9bd9b09b6ce0690d27",
  "state_root": "0x49e3bfe7b618e27fde8fa08884803a8458b502c6534af69873a3cc926a7c724b",
  "receipts_root": "0x7cf43d7e837284f036cf92c56973f5e27bdd253ca46168fa195a6b07fa719f23",
  "miner": "0xea674fdde714fd979de3edf0f56aa9716b898ec8",
  "difficulty": "7253857437305950",
  "total_difficulty": "24325637817906576196890",
  "size": "61271",
  "extra_data": "0x65746865726d696e652d6575726f70652d7765737433",
  "gas_limit": "14977947",
  "gas_used": "14964688",
  "transaction_count": "252",
  "transactions": [
    {
      "hash": "0x1ed85b3757a6d31d01a4d6677fc52fd3911d649a0af21fe5ca3f886b153773ed",
      "nonce": "1848059",
      "transaction_index": "108",
      "from_address": "0x267be1c1d684f78cb4f6a176c4911b741e4ffdc0",
      "to_address": "0x003dde3494f30d861d063232c6a8c04394b686ff",
      "value": "115580000000000000",
      "gas": "30000",
      "gas_price": "52500000000",
      "input": "0x",
      "receipt_cumulative_gas_used": "4923073",
      "receipt_gas_used": "21000",
      "receipt_contract_address": null,
      "receipt_root": null,
      "receipt_status": "1",
      "block_timestamp": "2021-05-07T11:08:35.000Z",
      "block_number": "12386788",
      "block_hash": "0x9b559aef7ea858608c2e554246fe4a24287e7aeeb976848df2b9a2531f4b9171",
      "logs": [
        {
          "log_index": "273",
          "transaction_hash": "0xdd9006489e46670e0e85d1fb88823099e7f596b08aeaac023e9da0851f26fdd5",
          "transaction_index": "204",
          "address": "0x3105d328c66d8d55092358cf595d54608178e9b5",
          "data": "0x00000000000000000000000000000000000000000000000de05239bccd4d537400000000000000000000000000024dbc80a9f80e3d5fc0a0ee30e2693781a443",
          "topic0": "0x2caecd17d02f56fa897705dcc740da2d237c373f70686f4e0d9bd3bf0400ea7a",
          "topic1": "0x000000000000000000000000031002d15b0d0cd7c9129d6f644446368deae391",
          "topic2": "0x000000000000000000000000d25943be09f968ba740e0782a34e710100defae9",
          "topic3": null,
          "block_timestamp": "2021-05-07T11:08:35.000Z",
          "block_number": "12386788",
          "block_hash": "0x9b559aef7ea858608c2e554246fe4a24287e7aeeb976848df2b9a2531f4b9171"
        }
      ]
    }
  ]
}
```

## getDateToBlock

Retrieve the closest block of the provided date  (asynchronous). 

#### Options:

* `chain`(optional): The blockchain to get data from. Valid values are listed on the [intro page in the Transactions and Balances section](https://docs.moralis.io/transactions-and-balances/intro). Default value `Eth`.
*  `date` (required): Unix date in miliseconds or a datestring (any format that is accepted by momentjs)

```javascript
const options = {
          chain: "bsc",
          date: "2021-09-29T13:09:15+00:00"
};
const date = await Moralis.Web3API.native.getDateToBlock(options);
```

#### Example result:

```javascript
{
  "date": "2020-01-01T00:00:00+00:00",
  "block": 9193266,
  "timestamp": 1577836811
}
```

## getLogsByAddress

Retrieve the logs from an address  (asynchronous). 

#### Options:

* `chain`(optional): The blockchain to get data from. Valid values are listed on the [intro page in the Transactions and Balances section](https://docs.moralis.io/transactions-and-balances/intro). Default value `Eth`.
* `from_date` (optional): The date from where to get the transactions (any format that is accepted by momentjs). Provide the param 'from_block' or 'from_date' If 'from_date' and 'from_block' are provided, 'from_block' will be used.
* `to_date` (optional):  Get the transactions to this date (any format that is accepted by momentjs). Provide the param 'to_block' or 'to_date' If 'to_date' and 'to_block' are provided, 'to_block' will be used.
* `from_block` (optional): The minimum block number from where to get the transactions Provide the param 'from_block' or 'from_date' If 'from_date' and 'from_block' are provided, 'from_block' will be used.
* `to_block` (optional): The maximum block number from where to get the transactions. Provide the param 'to_block' or 'to_date' If 'to_date' and 'to_block' are provided, 'to_block' will be used.
* `address` (required): A smart contract address
* `topic0` (optional): Event topic
* `topic1` (optional): Event topic
* `topic2` (optional): Event topic
* `topic3` (optional): Event topic

```javascript
const options = {
   address: "0x057Ec652A4F150f7FF94f089A38008f49a0DF88e",
   chain: "bsc",
   topic0: "0x2caecd17d02f56fa897705dcc740da2d237c373f70686f4e0d9bd3bf0400ea7a",
   topic1: "0x000000000000000000000000031002d15b0d0cd7c9129d6f644446368deae391",
   topic2: "0x000000000000000000000000d25943be09f968ba740e0782a34e710100defae9"
};

 const logs = await Moralis.Web3API.native.getLogsByAddress(options);
```

#### Example result:

```javascript
{
  "transaction_hash": "0x2d30ca6f024dbc1307ac8a1a44ca27de6f797ec22ef20627a1307243b0ab7d09",
  "address": "0x057Ec652A4F150f7FF94f089A38008f49a0DF88e",
  "block_timestamp": "2021-04-02T10:07:54.000Z",
  "block_number": "12526958",
  "block_hash": "0x0372c302e3c52e8f2e15d155e2c545e6d802e479236564af052759253b20fd86",
  "data": "0x00000000000000000000000000000000000000000000000de05239bccd4d537400000000000000000000000000024dbc80a9f80e3d5fc0a0ee30e2693781a443",
  "topic0": "0x2caecd17d02f56fa897705dcc740da2d237c373f70686f4e0d9bd3bf0400ea7a",
  "topic1": "0x000000000000000000000000031002d15b0d0cd7c9129d6f644446368deae391",
  "topic2": "0x000000000000000000000000d25943be09f968ba740e0782a34e710100defae9",
  "topic3": null
}
```

## getContractEvents

Get the  events in descending order based on block number. Returns an object with the number of Contract events and the array of Contract events (asynchronous).

#### Options:

* `chain`(optional): The blockchain to get data from. Valid values are listed on the [intro page in the Transactions and Balances section](https://docs.moralis.io/transactions-and-balances/intro). Default value `Eth`.
* `offset`(optional): Offset.
* `limit`(optional): Limit.
* `from_block` (optional): To get contract events starting from this block 
* `to_block` (optional): To get contract events up to this block
* `topic `(required): The topic of the event
* `address` (required): A smart contract address
* `abi` (required): Event ABI (do not insert the ABI of the whole smart contract). ABI has a JSON format.

```javascript
const ABI = {
  "anonymous": false,
  "inputs": [
    {
      "indexed": false,
      "internalType": "uint112",
      "name": "reserve0",
      "type": "uint112"
    },
    {
      "indexed": false,
      "internalType": "uint112",
      "name": "reserve1",
      "type": "uint112"
    }
  ],
  "name": "Sync",
  "type": "event"
};

const options = {
  chain: "bsc",
  address: "0x...16",
  topic: "0x1c411e9a96e071241c2f21f7726b17ae89e3cab4c78be50e062b03a9fffbbad1",
  abi: ABI 
};
const events = await Moralis.Web3API.native.getContractEvents(options);
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
    "data": {

    }
  }
]
```

## getNFTTransfersByBlock

Retrieve NFT transfers by block number or block hash.  Returns an array of NFT transfers (asynchronous). 

#### Options:

* `chain`(optional): The blockchain to get data from. Valid values are listed on the [intro page in the Transactions and Balances section](https://docs.moralis.io/transactions-and-balances/intro). Default value `Eth`.
*  `block_number_or_hash` (required): The block hash or block number.

```javascript
const options = { chain: "bsc", block_number_or_hash: "11284830" };

// get NFT transfers by block number or block hash
const NFTTransfers = await Moralis.Web3API.native.getNFTTransfersByBlock(options);
```

#### Example result:

```javascript
[
  {
    amount: "1",
    block_hash: "0x4c00bcee8900e5fc4f6a5d11dff6edca3b97470da290986940da1d2f0ef221fb",
    block_number: "11284830",
    block_timestamp: "2021-09-27T17:07:54.000Z",
    contract_type: "ERC721",
    from_address: "0x6102f780a5c51dee8ce46c5db5b62d88119d559d",
    log_index: 130,
    to_address: "0xd95f7419ce0debf3a991ef9944808010928db319",
    token_address: "0xcf58705e2ff8642c4276c2dd2747ec8af5bafc1d",
    token_id: "281",
    transaction_hash: "0x19721cf6855f412f82de87f8c6050e6de3cc77a81388002bc24efea53bd36a0f",
    transaction_index: 35,
    transaction_type: "Single"
  },
]
```

## getTransaction

Get the transaction by transaction hash.  Returns a transaction object (asynchronous). 

#### Options:

* `chain`(optional): The blockchain to get data from. Valid values are listed on the [intro page in the Transactions and Balances section](https://docs.moralis.io/transactions-and-balances/intro). Default value `Eth`.
*  `transaction_hash` (required): The transaction hash.

```javascript
const options = {
  chain: "bsc",
  transaction_hash: "0x1ed85b3757a6d31d01a4d6677fc52fd3911d649a0af21fe5ca3f886b153773ed"
};
const transaction = await Moralis.Web3API.native.getTransaction(options);
```

#### Example result:

```javascript
{
  "hash": "0x1ed85b3757a6d31d01a4d6677fc52fd3911d649a0af21fe5ca3f886b153773ed",
  "nonce": "1848059",
  "transaction_index": "108",
  "from_address": "0x267be1c1d684f78cb4f6a176c4911b741e4ffdc0",
  "to_address": "0x003dde3494f30d861d063232c6a8c04394b686ff",
  "value": "115580000000000000",
  "gas": "30000",
  "gas_price": "52500000000",
  "input": "0x",
  "receipt_cumulative_gas_used": "4923073",
  "receipt_gas_used": "21000",
  "receipt_contract_address": null,
  "receipt_root": null,
  "receipt_status": "1",
  "block_timestamp": "2021-05-07T11:08:35.000Z",
  "block_number": "12386788",
  "block_hash": "0x9b559aef7ea858608c2e554246fe4a24287e7aeeb976848df2b9a2531f4b9171",
  "logs": [
    {
      "log_index": "273",
      "transaction_hash": "0xdd9006489e46670e0e85d1fb88823099e7f596b08aeaac023e9da0851f26fdd5",
      "transaction_index": "204",
      "address": "0x3105d328c66d8d55092358cf595d54608178e9b5",
      "data": "0x00000000000000000000000000000000000000000000000de05239bccd4d537400000000000000000000000000024dbc80a9f80e3d5fc0a0ee30e2693781a443",
      "topic0": "0x2caecd17d02f56fa897705dcc740da2d237c373f70686f4e0d9bd3bf0400ea7a",
      "topic1": "0x000000000000000000000000031002d15b0d0cd7c9129d6f644446368deae391",
      "topic2": "0x000000000000000000000000d25943be09f968ba740e0782a34e710100defae9",
      "topic3": null,
      "block_timestamp": "2021-05-07T11:08:35.000Z",
      "block_number": "12386788",
      "block_hash": "0x9b559aef7ea858608c2e554246fe4a24287e7aeeb976848df2b9a2531f4b9171"
    }
  ]
}
```

## runContractFunction

Runs a given function of a contract abi and returns readonly data (asynchronous).

#### Options:

* `chain`(optional): The blockchain to get data from. Valid values are listed on the [intro page in the Transactions and Balances section](https://docs.moralis.io/transactions-and-balances/intro). Default value `Eth`.
* `function_name `(required): The function name
* `address` (required): A smart contract address
* `abi` (required): contract or function ABI(should be provided as an array)

```javascript
const ABI = [{
  "constant": true,
  "inputs": [
    {
      "internalType": "address",
      "name": "owner",
      "type": "address"
    },
    {
      "internalType": "address",
      "name": "spender",
      "type": "address"
    }
  ],
  "name": "allowance",
  "outputs": [
    {
      "internalType": "uint256",
      "name": "",
      "type": "uint256"
    }
  ],
  "payable": false,
  "stateMutability": "view",
  "type": "function"
}];

const options = {
  chain: "bsc",
  address: "0x...16",
  function_name: "allowance",
  abi: ABI 
};
const allowance = await Moralis.Web3API.native.runContractFunction(options);
```

#### Example result:

```javascript
"string"
```
