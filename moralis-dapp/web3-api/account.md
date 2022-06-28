# 💰 Web3API.account

## getTransactions

Returns the transactions from the current user or address. Returns an object with the number of transactions and the array of native transactions (asynchronous).

#### Options:

- `chain`(optional): The blockchain to get data from. Valid values are listed on [Supported Chains](supported-chains.md). Default value `Eth`.
- `address` (optional): A user address (i.e. `0x1a2b3x...`). If specified, the user attached to the query is ignored and the address will be used instead.
- `from_date` (optional): The date from where to get the transactions (any format that is accepted by momentjs). Provide the param 'from_block' or 'from_date' If 'from_date' and 'from_block' are provided, 'from_block' will be used.
- `to_date` (optional): Get the transactions to this date (any format that is accepted by momentjs). Provide the param 'to_block' or 'to_date' If 'to_date' and 'to_block' are provided, 'to_block' will be used.
- `from_block` (optional): The minimum block number from where to get the transactions Provide the param 'from_block' or 'from_date' If 'from_date' and 'from_block' are provided, 'from_block' will be used.
- `to_block` (optional): The maximum block number from where to get the transactions. Provide the param 'to_block' or 'to_date' If 'to_date' and 'to_block' are provided, 'to_block' will be used.
- `offset`(optional): Offset.
- `limit`(optional): Limit

{% tabs %}
{% tab title="JS" %}

```javascript
// get mainnet transactions for the current user
const transactions = await Moralis.Web3API.account.getTransactions();

// get BSC transactions for a given address
// with most recent transactions appearing first
const options = {
  chain: "bsc",
  address: "0x3d6c0e79a1239df0039ec16Cc80f7A343b6C530e",
  from_block: "0",
};
const transactions = await Moralis.Web3API.account.getTransactions(options);
```

{% endtab %}

{% tab title="React" %}

```javascript
import React from "react";
import { useMoralisWeb3Api } from "react-moralis";

const Web3Api = useMoralisWeb3Api();

const fetchTransactions = async () => {
  // get mainnet transactions for the current user
  const transactions = await Web3Api.account.getTransactions();
  console.log(transactions);

  // get BSC transactions for a given address
  // with most recent transactions appearing first
  const options = {
    chain: "bsc",
    address: "0x3d6c0e79a1239df0039ec16Cc80f7A343b6C530e",
    from_block: "0",
  };
  const bscTransactions = await Web3Api.account.getTransactions(options);
  console.log(bscTransactions);
};
```

{% endtab %}

{% tab title="curl" %}

```bash
curl -X 'GET' \
  'https://deep-index.moralis.io/api/v2/0x3d6c0e79a1239df0039ec16Cc80f7A343b6C530e?chain=bsc&from_block=0' \
  -H 'accept: application/json' \
  -H 'X-API-Key: MY-API-KEY'
```

{% endtab %}

{% tab title="Unity" %}

```csharp
using MoralisUnity;
using MoralisUnity.Web3Api.Models;
using UnityEngine;

public class Example
{
    public async void GetTransactions()
    {
        // get BSC transactions for a given address
        TransactionCollection BSCtransactions = await Moralis.Web3Api.Account.GetTransactions("0x3d6c0e79a1239df0039ec16Cc80f7A343b6C530e".ToLower(), ChainList.bsc);
        Debug.Log(BSCtransactions.ToJson());
    }
}
```

{% endtab %}
{% endtabs %}

#### Example result:

```javascript
[
  {
    hash: "0x057Ec652A4F150f7FF94f089A38008f49a0DF88e",
    nonce: "326595425",
    transaction_index: "25",
    from_address: "0xd4a3BebD824189481FC45363602b83C9c7e9cbDf",
    to_address: "0xa71db868318f0a0bae9411347cd4a6fa23d8d4ef",
    value: "650000000000000000",
    gas: "6721975",
    gas_price: "20000000000",
    input: "string",
    receipt_cumulative_gas_used: "1340925",
    receipt_gas_used: "1340925",
    receipt_contract_address: "0x1d6a4cf64b52f6c73f201839aded7379ce58059c",
    receipt_root: "string",
    receipt_status: "1",
    block_timestamp: "2021-04-02T10:07:54.000Z",
    block_number: "12526958",
    block_hash:
      "0x0372c302e3c52e8f2e15d155e2c545e6d802e479236564af052759253b20fd86",
  },
];
```

## getNativeBalance

Returns native balance for a specific address (asynchronous).

#### Options:

- `chain`(optional): The blockchain to get data from. Valid values are listed on [Supported Chains](supported-chains.md). Default value `Eth`.
- `to_block` (optional): The block number on which the balances should be checked.
- `address` (optional): The address for which the native balance will be checked. If specified, the user attached to the query is ignored and the address will be used instead. If user is not authenticated with a Wallet, then address has to be specified.

{% tabs %}
{% tab title="JS" %}

```javascript
// get mainnet native balance for the current user
const balance = await Moralis.Web3API.account.getNativeBalance();

// get BSC native balance for a given address
const options = {
  chain: "bsc",
  address: "0x3d6c0e79a1239df0039ec16Cc80f7A343b6C530e",
  to_block: "1234",
};
const balance = await Moralis.Web3API.account.getNativeBalance(options);
```

{% endtab %}

{% tab title="React" %}

```javascript
import React from "react";
import { useMoralisWeb3Api } from "react-moralis";

const Web3Api = useMoralisWeb3Api();
const fetchNativeBalance = async () => {
  // get mainnet native balance for the current user
  const balance = await Web3Api.account.getNativeBalance();
  console.log(balance);
  // get BSC native balance for a given address
  const options = {
    chain: "bsc",
    address: "0x3d6c0e79a1239df0039ec16Cc80f7A343b6C530e",
    to_block: "1234",
  };
  const bscBalance = await Web3Api.account.getNativeBalance(options);
  console.log(bscBalance);
};
```

{% endtab %}

{% tab title="curl" %}

```bash
curl -X 'GET' \
  'https://deep-index.moralis.io/api/v2/0x3d6c0e79a1239df0039ec16Cc80f7A343b6C530e/balance?chain=bsc' \
  -H 'accept: application/json' \
  -H 'X-API-Key: MY-API-KEY'
```

{% endtab %}

{% tab title="Unity" %}

```csharp
using MoralisUnity;
using MoralisUnity.Web3Api.Models;
using UnityEngine;

public class Example
{
    public async void GetNativeBalance()
    {
        // get BSC native balance for a given address
        NativeBalance BSCbalance = await Moralis.Web3Api.Account.GetNativeBalance("0x4c6Ec2448C243B39Cd1e9E6db0F9bF7436c0c93f".ToLower(), ChainList.bsc);
        Debug.Log(BSCbalance.ToJson());
    }
}
```

{% endtab %}
{% endtabs %}

#### Example result:

```javascript
{
  "balance": "1234567890"
}
```

## getTokenBalances

Retrieve all token balances of a current user or specified address. Returns an object with the number of tokens and the array of token objects (asynchronous).

#### Options:

- `chain`(optional): The blockchain to get data from. Valid values are listed on [Supported Chains](supported-chains.md). Default value `Eth`.
- `address` (optional): A user address (i.e. `0x1a2b3x...`). If specified, the user attached to the query is ignored and the address will be used instead.
- `to_block` (optional): The block number on which the balances should be checked

{% tabs %}
{% tab title="JS" %}

```javascript
const balances = await Moralis.Web3API.account.getTokenBalances();
```

{% endtab %}

{% tab title="React" %}

```javascript
import React from "react";
import { useMoralisWeb3Api } from "react-moralis";

const Web3Api = useMoralisWeb3Api();

const fetchTokenBalances = async () => {
  const balances = await Web3Api.account.getTokenBalances();
  console.log(balances);
};
```

{% endtab %}

{% tab title="curl" %}

```bash
curl -X 'GET' \
  'https://deep-index.moralis.io/api/v2/0x3d6c0e79a1239df0039ec16Cc80f7A343b6C530e/erc20' \
  -H 'accept: application/json' \
  -H 'X-API-Key: MY-API-KEY'
```

{% endtab %}

{% tab title="Unity" %}

```csharp
using MoralisUnity;
using MoralisUnity.Web3Api.Models;
using System.Collections.Generic;
using UnityEngine;

public class Example
{
    public async void fetchTokenBalance()
    {
        List<Erc20TokenBalance> balance = await Moralis.Web3Api.Account.GetTokenBalances("0x3d6c0e79a1239df0039ec16Cc80f7A343b6C530e".ToLower(), ChainList.bsc);
        foreach (Erc20TokenBalance erc20bal in balance)
        {
            Debug.Log(erc20bal.ToJson());
        }
    }
}
```

{% endtab %}
{% endtabs %}

Without any parameters specified, it defaults to 'Eth' as chain and the current user, but you can also specify the `chain` and `address`in an options object:

{% tabs %}
{% tab title="JS" %}

```javascript
const options = {
  chain: "bsc",
  address: "0x3d6c0e79a1239df0039ec16Cc80f7A343b6C530e",
  to_block: "10253391",
};
const balances = await Moralis.Web3API.account.getTokenBalances(options);
```

{% endtab %}

{% tab title="React" %}

```javascript
import React from "react";
import { useMoralisWeb3Api } from "react-moralis";

const Web3Api = useMoralisWeb3Api();

const fetchTokenBalances = async () => {
  const options = {
    chain: "bsc",
    address: "0x3d6c0e79a1239df0039ec16Cc80f7A343b6C530e",
    to_block: "10253391",
  };
  const balances = await Web3Api.account.getTokenBalances(options);
  console.log(balances);
};
```

{% endtab %}

{% tab title="curl" %}

```bash
curl -X 'GET' \
  'https://deep-index.moralis.io/api/v2/0x3d6c0e79a1239df0039ec16Cc80f7A343b6C530e/erc20?chain=bsc&to_block=10253391' \
  -H 'accept: application/json' \
  -H 'X-API-Key: MY-API-KEY'
```

{% endtab %}
{% endtabs %}

#### Example result:

```javascript
[
  {
    token_address:
      "0x2d30ca6f024dbc1307ac8a1a44ca27de6f797ec22ef20627a1307243b0ab7d09",
    name: "Kylin Network",
    symbol: "KYL",
    logo: "https://cdn.moralis.io/eth/0x67b6d479c7bb412c54e03dca8e1bc6740ce6b99c.png",
    thumbnail:
      "https://cdn.moralis.io/eth/0x67b6d479c7bb412c54e03dca8e1bc6740ce6b99c_thumb.png",
    decimals: "18",
    balance: "123456789",
  },
];
```

## getTokenTransfers

Get ERC20 token transfers from the current user or address. Returns an object with the number of token transfers and the array of token transfers (asynchronous).

#### Options:

- `chain`(optional): The blockchain to get data from. Valid values are listed on [Supported Chains](supported-chains.md). Default value `Eth`.
- `address` (optional): A user address (i.e. `0x1a2b3x...`). If specified, the user attached to the query is ignored and the address will be used instead.
- `from_date` (optional): The date from where to get the transactions (any format that is accepted by momentjs). Provide the param 'from_block' or 'from_date' If 'from_date' and 'from_block' are provided, 'from_block' will be used.
- `to_date` (optional): Get the transactions to this date (any format that is accepted by momentjs). Provide the param 'to_block' or 'to_date' If 'to_date' and 'to_block' are provided, 'to_block' will be used.
- `from_block` (optional): The minimum block number from where to get the transactions Provide the param 'from_block' or 'from_date' If 'from_date' and 'from_block' are provided, 'from_block' will be used.
- `to_block` (optional): The maximum block number from where to get the transactions. Provide the param 'to_block' or 'to_date' If 'to_date' and 'to_block' are provided, 'to_block' will be used.
- `offset`(optional): Offset.
- `limit`(optional): Limit

{% tabs %}
{% tab title="JS" %}

```javascript
// get mainnet transfers for the current user
const userTrans = await Moralis.Web3API.account.getTokenTransfers();

// get BSC transfers for a given address
// with most recent transfers appearing first
const options = {
  chain: "bsc",
  address: "0x3d6c0e79a1239df0039ec16Cc80f7A343b6C530e",
  from_block: "0",
};
const transfers = await Moralis.Web3API.account.getTokenTransfers(options);
```

{% endtab %}

{% tab title="React" %}

```javascript
import React from "react";
import { useMoralisWeb3Api } from "react-moralis";

const Web3Api = useMoralisWeb3Api();

const fetchTokenTransfers = async () => {
  // get mainnet transfers for the current user
  const userTrans = await Web3Api.account.getTokenTransfers();
  console.log(userTrans);

  // get BSC transfers for a given address
  // with most recent transfers appearing first
  const options = {
    chain: "bsc",
    address: "0x3d6c0e79a1239df0039ec16Cc80f7A343b6C530e",
    from_block: "0",
  };
  const transfers = await Web3Api.account.getTokenTransfers(options);
  console.log(transfers);
};
```

{% endtab %}

{% tab title="curl" %}

```bash
curl -X 'GET' \
  'https://deep-index.moralis.io/api/v2/0x3d6c0e79a1239df0039ec16Cc80f7A343b6C530e/erc20/transfers?chain=bsc&from_block=0' \
  -H 'accept: application/json' \
  -H 'X-API-Key: MY-API-KEY'
```

{% endtab %}

{% tab title="Unity" %}

```csharp
using MoralisUnity;
using MoralisUnity.Web3Api.Models;
using UnityEngine;

public class Example
{
    public async void fetchTokenTransfers()
    {
        Erc20TransactionCollection transfers = await Moralis.Web3Api.Account.GetTokenTransfers("0x3d6c0e79a1239df0039ec16Cc80f7A343b6C530e".ToLower(), ChainList.bsc);
        Debug.Log(transfers.ToJson());
    }
}
```

{% endtab %}
{% endtabs %}

#### Example result:

```javascript
[
  {
    transaction_hash:
      "0x2d30ca6f024dbc1307ac8a1a44ca27de6f797ec22ef20627a1307243b0ab7d09",
    address: "0x057Ec652A4F150f7FF94f089A38008f49a0DF88e",
    block_timestamp: "2021-04-02T10:07:54.000Z",
    block_number: "12526958",
    block_hash:
      "0x0372c302e3c52e8f2e15d155e2c545e6d802e479236564af052759253b20fd86",
    to_address: "0x62AED87d21Ad0F3cdE4D147Fdcc9245401Af0044",
    from_address: "0xd4a3BebD824189481FC45363602b83C9c7e9cbDf",
    value: "650000000000000000",
  },
];
```

## getNFTs

Get all NFTs from the current user or address. Supports both ERC721 and ERC1155. Returns an object with the number of NFT objects and the array of NFT objects (asynchronous).

#### Options:

- `chain`(optional): The blockchain to get data from. Valid values are listed on [Supported Chains](supported-chains.md). Default value `Eth`.
- `address` (optional): A user address (i.e. `0x1a2b3x...`). If specified, the user attached to the query is ignored and the address will be used instead.

{% tabs %}
{% tab title="JS" %}

```javascript
// get NFTs for current user on Mainnet
const userEthNFTs = await Moralis.Web3API.account.getNFTs();

// get testnet NFTs for user
const testnetNFTs = await Moralis.Web3API.account.getNFTs({ chain: "ropsten" });

// get polygon NFTs for address
const options = {
  chain: "polygon",
  address: "0x75e3e9c92162e62000425c98769965a76c2e387a",
};
const polygonNFTs = await Moralis.Web3API.account.getNFTs(options);
```

{% endtab %}

{% tab title="React" %}

```javascript
import React from "react";
import { useMoralisWeb3Api } from "react-moralis";

const Web3Api = useMoralisWeb3Api();

const fetchNFTs = async () => {
  // get NFTs for current user on Mainnet
  const userEthNFTs = await Web3Api.account.getNFTs();
  console.log(userEthNFTs);
  // get testnet NFTs for user
  const testnetNFTs = await Web3Api.Web3API.account.getNFTs({
    chain: "ropsten",
  });
  console.log(testnetNFTs);

  // get polygon NFTs for address
  const options = {
    chain: "polygon",
    address: "0x75e3e9c92162e62000425c98769965a76c2e387a",
  };
  const polygonNFTs = await Web3Api.account.getNFTs(options);
};
console.log(polygonNFTs);
```

{% endtab %}

{% tab title="curl" %}

```bash
curl -X 'GET' \
  'https://deep-index.moralis.io/api/v2/0x75e3e9c92162e62000425c98769965a76c2e387a/nft?chain=polygon&format=decimal' \
  -H 'accept: application/json' \
  -H 'X-API-Key: MY-API-KEY'
```

{% endtab %}

{% tab title="Unity" %}

```csharp
using MoralisUnity;
using MoralisUnity.Web3Api.Models;
using UnityEngine;

public class Example
{
    public async void fetchNFTs()
    {
        NftOwnerCollection polygonNFTs = await Moralis.Web3Api.Account.GetNFTs("0x75e3e9c92162e62000425c98769965a76c2e387a".ToLower(), ChainList.polygon);
        Debug.Log(polygonNFTs.ToJson());
    }
}
```

{% endtab %}
{% endtabs %}

#### Example result:

```javascript
[
  {
    token_address: "0x057Ec652A4F150f7FF94f089A38008f49a0DF88e",
    token_id: "15",
    contract_type: "ERC721",
    owner_of: "0x057Ec652A4F150f7FF94f089A38008f49a0DF88e",
    block_number: "88256",
    block_number_minted: "88256",
    token_uri: "string",
    metadata: "string",
    synced_at: "string",
    amount: "1",
    name: "CryptoKitties",
    symbol: "RARI",
  },
];
```

## getNFTTransfers

Get the NFT transfers. Returns an object with the number of NFT transfers and the array of NFT transfers (asynchronous).

#### Options:

- `chain`(optional): The blockchain to get data from. Valid values are listed on [Supported Chains](supported-chains.md). Default value `Eth`.
- `format` (optional): The format of the token id. Available values : `decimal`, `hex`. Default value : `decimal.`
- `offset`(optional): Offset.
- `direction`(optional): The transfer direction. Available values : `both`, `to`, `from` . Default value : `both`.
- `limit`(optional): Limit.
- `address` (optional): A user address (i.e. `0x1a2b3x...`). If specified, the user attached to the query is ignored and the address will be used instead.

{% tabs %}
{% tab title="JS" %}

```javascript
// get mainnet NFT transfers for the current user
const transfersNFT = await Moralis.Web3API.account.getNFTTransfers();

// get BSC NFT transfers for a given address
// with most recent transactions appearing first
const options = {
  chain: "polygon",
  address: "0x75e3e9c92162e62000425c98769965a76c2e387a",
  limit: "5",
};
const transfersNFT = await Moralis.Web3API.account.getNFTTransfers(options);
```

{% endtab %}

{% tab title="React" %}

```javascript
import React from "react";
import { useMoralisWeb3Api } from "react-moralis";

const Web3Api = useMoralisWeb3Api();

const fetchNFTTransfers = async () => {
  // get mainnet NFT transfers for the current user
  const transfersNFT = await Web3Api.account.getNFTTransfers();
  console.log(transfersNFT);
  // get BSC NFT transfers for a given address
  // with most recent transactions appearing first
  const options = {
    chain: "polygon",
    address: "0x75e3e9c92162e62000425c98769965a76c2e387a",
    limit: "5",
  };
  const bscTransfersNFT = await Web3Api.account.getNFTTransfers(options);
  console.log(bscTransfersNFT);
};
```

{% endtab %}

{% tab title="curl" %}

```bash
curl -X 'GET' \
  'https://deep-index.moralis.io/api/v2/0x75e3e9c92162e62000425c98769965a76c2e387a/nft/transfers?chain=polygon&format=decimal&direction=both&limit=5' \
  -H 'accept: application/json' \
  -H 'X-API-Key: MY-API-KEY'
```

{% endtab %}

{% tab title="Unity" %}

```csharp
using MoralisUnity;
using MoralisUnity.Web3Api.Models;
using UnityEngine;

public class Example
{
    public async void fetchNFTTransfers()
    {
        NftTransferCollection BSCnfttransfers = await Moralis.Web3Api.Account.GetNFTTransfers("0x3d6c0e79a1239df0039ec16Cc80f7A343b6C530e".ToLower(), ChainList.bsc);
        Debug.Log(BSCnfttransfers.ToJson());
    }
}
```

{% endtab %}
{% endtabs %}

{% hint style="info" %}
Use the token_address param to get results for a specific contract only.

Note results will include all indexed NFTs.

Any request which includes the token_address param will start the indexing process for that NFT collection the very first time it is requested.
{% endhint %}

#### Example result:

```javascript
[
  {
    token_address: "0x057Ec652A4F150f7FF94f089A38008f49a0DF88e",
    token_id: "15",
    from_address: "0x057Ec652A4F150f7FF94f089A38008f49a0DF88e",
    to_address: "0x057Ec652A4F150f7FF94f089A38008f49a0DF88e",
    amount: "1",
    contract_type: "ERC721",
    block_number: "88256",
    block_timestamp: "2021-06-04T16:00:15",
    block_hash: "string",
    transaction_hash: "0x057Ec652A4F150f7FF94f089A38008f49a0DF88e",
    transaction_type: "string",
    transaction_index: "string",
    log_index: 0,
  },
];
```

## getNFTsForContract

Returns an object with the NFT count for the specified contract and an NFT array belonging to the given address for the specified contract (asynchronous).

#### Options:

- `chain`(optional): The blockchain to get data from. Valid values are listed on [Supported Chains](supported-chains.md). Default value `Eth`.
- `format` (optional): The format of the token id. Available values : `decimal`, `hex`. Default value : `decimal.`
- `offset`(optional): Offset.
- `limit`(optional): Limit.
- `address` (optional): The owner of a given token (i.e. `0x1a2b3x...`). If specified, the user attached to the query is ignored and the address will be used instead.
- `token_address`(required): Address of the contract

{% tabs %}
{% tab title="JS" %}

```javascript
const options = {
  chain: "polygon",
  address: "0x75e3e9c92162e62000425c98769965a76c2e387a",
  token_address: "0x2953399124F0cBB46d2CbACD8A89cF0599974963",
};
const polygonNFTs = await Moralis.Web3API.account.getNFTsForContract(options);
```

{% endtab %}

{% tab title="React" %}

```javascript
import React from "react";
import { useMoralisWeb3Api } from "react-moralis";

const Web3Api = useMoralisWeb3Api();

const fetchNFTsForContract = async () => {
  const options = {
    chain: "polygon",
    address: "0x75e3e9c92162e62000425c98769965a76c2e387a",
    token_address: "0x2953399124F0cBB46d2CbACD8A89cF0599974963",
  };
  const polygonNFTs = await Web3Api.account.getNFTsForContract(options);
  console.log(polygonNFTs);
};
```

{% endtab %}

{% tab title="curl" %}

```bash
curl -X 'GET' \
  'https://deep-index.moralis.io/api/v2/0x75e3e9c92162e62000425c98769965a76c2e387a/nft/0x2953399124F0cBB46d2CbACD8A89cF0599974963?chain=polygon&format=decimal' \
  -H 'accept: application/json' \
  -H 'X-API-Key: My-API-KEY'
```

{% endtab %}

{% tab title="Unity" %}

```csharp
using MoralisUnity;
using MoralisUnity.Web3Api.Models;
using UnityEngine;

public class Example
{
    public async void fetchNFTsForContract()
    {
        NftOwnerCollection polygonNFTs = await Moralis.Web3Api.Account.GetNFTsForContract("0x3d6c0e79a1239df0039ec16Cc80f7A343b6C530e".ToLower(), "0x2953399124F0cBB46d2CbACD8A89cF0599974963", ChainList.polygon);
        Debug.Log(polygonNFTs.ToJson());
    }
}
```

{% endtab %}
{% endtabs %}

{% hint style="info" %}
Use the token_address param to get results for a specific contract only.

Note results will include all indexed NFTs.

Any request which includes the token_address param will start the indexing process for that NFT collection the very first time it is requested.
{% endhint %}

#### Example result:

```javascript
{
  total: 5,
  page: 0,
  page_size: 1,
  cursor: "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ3aGVyZSI6eyJ0b2tlbl9hZGRyZXNzIjoiMHgyOTUzMzk5MTI0ZjBjYmI0NmQyY2JhY2Q4YTg5Y2YwNTk5OTc0OTYzIiwib3duZXJfb2YiOiIweDc1ZTNlOWM5MjE2MmU2MjAwMDQyNWM5ODc2OTk2NWE3NmMyZTM4N2EifSwibGltaXQiOjEsIm9mZnNldCI6MSwib3JkZXIiOltbInRyYW5zZmVyX2luZGV4IiwiREVTQyJdXSwicGFnZSI6MSwiaWF0IjoxNjQ2NDkxMzYzfQ.2emXTUoQYAV5dcC-05fkX5bHuuCHgL8aSQ2P9nqJPs0",
  result: [
    {
      "token_address": "0x2953399124f0cbb46d2cbacd8a89cf0599974963",
      "token_id": "54882136101329053367331551663964422650505490251725779255107807133728757514309",
      "block_number_minted": "22364568",
      "owner_of": "0x75e3e9c92162e62000425c98769965a76c2e387a",
      "block_number": "22728905",
      "amount": "1",
      "contract_type": "ERC1155",
      "name": "OpenSea Collections",
      "symbol": "OPENSTORE",
      "token_uri": "https://api.opensea.io/api/v2/metadata/matic/0x2953399124F0cBB46d2CbACD8A89cF0599974963/0x7956302fe62df98c5c7f35354a2d03eb8b160f0e000000000000020000000045",
      "metadata": null,
      "synced_at": "2021-12-10T17:04:09.775Z",
      "is_valid": 0,
      "syncing": 2,
      "frozen": 0
    }
  ],
  status: "SYNCED"
}
```
