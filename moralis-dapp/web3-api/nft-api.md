---
description: >-
  Fetch metadata, ownership and transfer data, price data, search NFTs based on
  metadata and so much more.
---

# 🖼 NFT API

## The Most Powerful Cross-chain NFT API

Full documentation: [https://github.com/nft-api/nft-api](https://github.com/nft-api/nft-api)

## What is NFT API?

The NFT API removes all the difficulties from working with NFTs and integrating NFTs into your dapps.

Working with NFTs is normally difficult as NFT token standards are poorly followed and a lot of edge-cases arise when you start parsing on-chain NFT data.

That's why the NFT API was built to provide a unified interface for all NFT tokens across different blockchains so that you can focus on your dapp without running your own NFT parsing pipeline.

The most important use-cases for this API is getting cross-chain data related to:

1. **NFT Metadata**
2. **NFT Ownership data**
3. **NFT Transfer data**
4. **NFT Prices**

## NFT API Specification

**Chain supported:**

1. Ethereum (ETH)
2. Binance Smart Chain (BSC)
3. Polygon (MATIC)
4. Avalanche (AVAX)
5. Fantom (FTM)
6. Testnets are fully supported.

**Endpoints:**

### searchNFTs

Very powerful and fast tool for getting the NFT data based on a metadata search (asynchronous).

#### Options:

- `chain`(optional): The blockchain to get data from. Valid values are listed on [Supported Chains](supported-chains.md). Default value `Eth`.
- `format` (optional): The format of the token id. Available values : `decimal`, `hex`. Default value : `decimal.`
- `cursor` (optional): The next page of data to retrieve. Next page cursor value returned from each request.
- `limit`(optional): limit (max 100).
- `q` (required): The search string parameter
- `filter`(required): What fields the search should match on. To look into the entire metadata set the value to `global`. To have a better response time you can look into a specific field like name. Available values : `name`; `description`; `attributes`; `global`; `name,description`; `name,attributes`; `description,attributes`; `name,description,attributes`

{% tabs %}
{% tab title="JS" %}

```javascript
const options = { q: "Pancake", chain: "bsc", filter: "name" };
const NFTs = await Moralis.Web3API.token.searchNFTs(options);
```

{% endtab %}

{% tab title="React" %}

```javascript
import React from "react";
import { useMoralisWeb3Api } from "react-moralis";

const Web3Api = useMoralisWeb3Api();

const fetchSearchNFTs = async () => {
  const options = { q: "Pancake", chain: "bsc", filter: "name" };
  const NFTs = await Web3Api.token.searchNFTs(options);
  console.log(NFTs);
};
```

{% endtab %}

{% tab title="curl" %}

```bash
curl -X 'GET' \
  'https://deep-index.moralis.io/api/v2/nft/search?chain=bsc&format=decimal&q=Pancake&filter=name' \
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
    public async void fetchSearchNFTs()
    {
        NftMetadataCollection nft = await Moralis.Web3Api.Token.SearchNFTs(q: "Pancake", ChainList.bsc, filter: "name");
        Debug.Log(nft.ToJson());
    }
}
```

{% endtab %}
{% endtabs %}

#### Example result:

```javascript
[
  {
    token_id: "124436",
    token_address: "0x3afa102b264b5f79ce80fed29e0724f922ba57c7",
    token_uri:
      "https://ipfs.moralis.io:2053/ipfs/QmVAD8v4s2SXF8FgjePqMdQ2GV5hE2isZnzxcrA36XcSDA/metadata.json",
    metadata:
      '{"name":"Pancake","description":"The dessert series 1","image":"ipfs://QmNQFXCZ6LGzvpMW9Q5PWbCrEnLknQrPwr2r8pbQAgzQ9A/4863BD6B-6C92-4B96-BF80-8020B2F7C3A5.jpeg"}',
    contract_type: "ERC721",
    token_hash: "d03fe436e972bf9215d7bb8c64c4c556",
    synced_at: null,
    created_at: "2021-09-19T10:36:16.610Z",
  },
];
```

#### Searching by `global`:

{% tabs %}
{% tab title="JS" %}

```javascript
const options = { q: "bored ape", chain: "bsc", filter: "global" };
const NFTs = await Moralis.Web3API.token.searchNFTs(options);
```

{% endtab %}

{% tab title="React" %}

```javascript
import React from "react";
import { useMoralisWeb3Api } from "react-moralis";

const Web3Api = useMoralisWeb3Api();

const fetchSearchNFTs = async () => {
  const options = { q: "bored ape", chain: "bsc", filter: "global" };
  const NFTs = await Web3Api.token.searchNFTs(options);
  console.log(NFTs);
};
```

{% endtab %}

{% tab title="curl" %}

```bash
curl -X 'GET' \
  'https://deep-index.moralis.io/api/v2/nft/search?chain=bsc&format=decimal&q=Pancake&filter=global' \
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
    public async void fetchSearchNFTs()
    {
        NftMetadataCollection nft = await Moralis.Web3Api.Token.SearchNFTs(q: "Pancake", ChainList.bsc, filter: "global");
        Debug.Log(nft.ToJson());
    }
}
```

{% endtab %}
{% endtabs %}

#### Searching by `description,attributes`:

{% tabs %}
{% tab title="JS" %}

```javascript
const options = {
  q: "loves bananas",
  chain: "bsc",
  filter: "description,attributes",
};
const NFTs = await Moralis.Web3API.token.searchNFTs(options);
```

{% endtab %}

{% tab title="React" %}

```javascript
import React from "react";
import { useMoralisWeb3Api } from "react-moralis";

const Web3Api = useMoralisWeb3Api();

const fetchSearchNFTs = async () => {
  const options = {
    q: "loves bananas",
    chain: "bsc",
    filter: "description,attributes",
  };
  const NFTs = await Web3Api.token.searchNFTs(options);
  console.log(NFTs);
};
```

{% endtab %}

{% tab title="curl" %}

```bash
curl -X 'GET' \
  'https://deep-index.moralis.io/api/v2/nft/search?chain=bsc&format=decimal&q=Pancake&filter=description%2Cattributes' \
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
    public async void fetchSearchNFTs()
    {
        NftMetadataCollection nft = await Moralis.Web3Api.Token.SearchNFTs(q: "Pancake", ChainList.bsc, filter: "description,attributes");
        Debug.Log(nft.ToJson());
    }
}
```

{% endtab %}
{% endtabs %}

#### Example result:

```javascript
"result": [
    {
      token_id: "854645",
      token_address: "0xdf7952b35f24acf7fc0487d01c8d5690a60dba07",
      token_uri: "https://ipfs.moralis.io:2053/ipfs/QmYUHFzEvPsoseNWcHtqE18Ao8HPBRktLPoDMKpdDYvHQV",
      metadata: "{\n  \"name\": \"Pancake Christmas 2021\",\n  \"description\": \"A great collab between Chef Cecy and the winner of the #PancakeChristmas event. Merry Christmas, and Happy New Year!\",\n  \"mp4_url\": \"ipfs://QmXYFMtYVBJBKjgGwf6MmrKhVtNdbVjLQCPLCu99hpAu47/christmas-2021.mp4\",\n  \"gif_url\": \"ipfs://QmXYFMtYVBJBKjgGwf6MmrKhVtNdbVjLQCPLCu99hpAu47/christmas-2021.gif\",\n  \"webm_url\": \"ipfs://QmXYFMtYVBJBKjgGwf6MmrKhVtNdbVjLQCPLCu99hpAu47/christmas-2021.webm\",\n  \"image\": \"ipfs://QmXYFMtYVBJBKjgGwf6MmrKhVtNdbVjLQCPLCu99hpAu47/christmas-2021.png\",\n  \"attributes\": {\n    \"bunnyId\": \"23\"\n  }\n}",
      is_valid: 1,
      syncing: 2,
      frozen: 0,
      resyncing: 0,
      synced_at: "2022-01-02T19:50:58.391Z",
      contract_type: "ERC721",
      token_hash: "fffedca3037a8dc03483193b99fa9736",
      batch_id: null,
      metadata_name: "\"Pancake Christmas 2021\"",
      metadata_description: "\"A great collab between Chef Cecy and the winner of the #PancakeChristmas event. Merry Christmas, and Happy New Year!\"",
      metadata_attributes: "{\"bunnyId\":\"23\"}",
      block_number_minted: "14022006",
      opensea_lookup: null,
      minter_address: "0xd338d2d63b55cbb059162394544a473156d513bd",
      transaction_minted: "0xd7d0651c5bdf168c83f7a7e6570345bf73b11aabee3da7a6a3126034a77e382d",
      frozen_log_index: null,
      imported: null,
      createdAt: "2022-01-02T19:49:52.234Z",
      updatedAt: "2022-01-02T19:49:52.234Z"
    }
```

### getNFTs

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
const testnetNFTs = await Moralis.Web3API.account.getNFTs({ chain: "goerli" });

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
    chain: "goerli",
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

### getNFTsForContract

Returns an object with the NFT count for the specified contract and an NFT array belonging to the given address for the specified contract (asynchronous).

#### Options:

- `chain`(optional): The blockchain to get data from. Valid values are listed on [Supported Chains](supported-chains.md). Default value `Eth`.
- `format` (optional): The format of the token id. Available values : `decimal`, `hex`. Default value : `decimal.`
- `cursor` (optional): The next page of data to retrieve. Next page cursor value returned from each request.
- `limit`(optional): limit (max 100).
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

### getNFTTransfers

Get the NFT transfers. Returns an object with the number of NFT transfers and the array of NFT transfers (asynchronous).

#### Options:

- `chain`(optional): The blockchain to get data from. Valid values are listed on [Supported Chains](supported-chains.md). Default value `Eth`.
- `format` (optional): The format of the token id. Available values : `decimal`, `hex`. Default value : `decimal.`
- `direction`(optional): The transfer direction. Available values : `both`, `to`, `from` . Default value : `both`.
- `cursor` (optional): The next page of data to retrieve. Next page cursor value returned from each request.
- `limit`(optional): limit (max 100).
- `address` (required): A user address (i.e. `0x1a2b3x...`). If specified, the user attached to the query is ignored and the authenticated address will be used instead.

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

### getNftTransfersFromToBlock

Gets NFT transfers from a block number to a block number

#### Options:

Needs at least one of `from_block`, `to_block` , `from_date` , `to_date`

- `chain`(optional): The blockchain to get data from. Valid values are listed on [Supported Chains](supported-chains.md). Default value `Eth`.
- `from_block`(optional): The minimum block number from where to get the transfers. Provide the param 'from_block' or 'from_date'. If 'from_date' and 'from_block' are provided, 'from_block' will be used.
- `to_block` (optional): The maximum block number from where to get the transfers. Provide the param 'to_block' or 'to_date', If 'to_date' and 'to_block' are provided, 'to_block' will be used.
- `from_date` (optional): The date from where to get the transfers (any format that is accepted by momentjs). Provide the param 'from_block' or 'from_date'. If 'from_date' and 'from_block' are provided, 'from_block' will be used.
- `to_date`(optional): Get transfers up until this date (any format that is accepted by momentjs). Provide the param 'to_block' or 'to_date'. If 'to_date' and 'to_block' are provided, 'to_block' will be used.
- `format`(optional): The format of the token id. Available values : `decimal`, `hex`. Default value : `decimal.`
- `limit`(optional): limit

{% tabs %}
{% tab title="JS" %}

```javascript
const options = {
  from_block: "14876000",
  to_block: "14877000",
  //from_date: "",
  //to_date: "",
  format: "decimal",
  limit: "10",
};
const data = await Moralis.Web3API.token.getNftTransfersFromToBlock(options);
```

{% endtab %}

{% tab title="React" %}

```javascript
import React from "react";
import { useMoralisWeb3Api } from "react-moralis";

const Web3Api = useMoralisWeb3Api();

const nftTransfers = async () => {
  const options = {
    from_block: "14876000",
    to_block: "14877000",
    format: "decimal",
    limit: "10",
  };
  const nftTransfers = await Web3Api.token.getNftTransfersFromToBlock(options);
  console.log(nftTransfers);
};
```

{% endtab %}

{% tab title="curl" %}

```bash
curl -X 'GET' \
  'https://deep-index.moralis.io/api/v2/nft/transfers?chain=eth&from_block=14876000&format=decimal' \
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
        NftTransferCollection nftTransfers = await Moralis.Web3Api.Token.getNftTransfersFromToBlock(from_block: "14876000", ChainList.eth);
        Debug.Log(nftTransfers.ToJson());
    }
}
```

{% endtab %}
{% endtabs %}

{% hint style="info" %}
Make sure to include a sort parm on a column like token_id for consistent pagination results
{% endhint %}

{% hint style="info" %}
Requests for contract addresses not yet indexed will automatically start the indexing process for that NFT collection
{% endhint %}

#### Example result:

```javascript
"result": [
    {
      "block_number": "14876001",
      "block_timestamp": "2022-05-31T01:41:33.000Z",
      "block_hash": "0x38e7a69058a46376774dfe7f625e0cca7828a129925ada09ab7761bda6b701de",
      "transaction_hash": "0x0ec413e59b7ca4d0eee2c18ae054315b30faed0c3ddb5de6af0689794f225abe",
      "transaction_index": 180,
      "log_index": 416,
      "value": "24900000000000000",
      "contract_type": "ERC721",
      "transaction_type": "Single",
      "token_address": "0x415f77738147a65a9d76bb0407af206a921cee0f",
      "token_id": "1377",
      "from_address": "0xcc3490aecec5eb123c2f6a51e406f04debb47f96",
      "to_address": "0xe966275f1e1932fb064f9ba90aa65e0c2ad0bfad",
      "amount": "1",
      "verified": 1,
      "operator": null
    },
    {
      "block_number": "14876001",
      "block_timestamp": "2022-05-31T01:41:33.000Z",
      "block_hash": "0x38e7a69058a46376774dfe7f625e0cca7828a129925ada09ab7761bda6b701de",
      "transaction_hash": "0x7de568004f40c193989306e2e3bb3c17670c2aef7da2fb5065b633d6cd48bd1b",
      "transaction_index": 178,
      "log_index": 413,
      "value": "80000000000000000",
      "contract_type": "ERC1155",
      "transaction_type": "Single",
      "token_address": "0xb66a603f4cfe17e3d27b87a8bfcad319856518b8",
      "token_id": "68508210689022796360004252200345882845300391813826407755322269495475862241291",
      "from_address": "0x977645ec9a66cb8baa744c73862b7525845390ec",
      "to_address": "0x0ee8951fe70b088b5ecf63af4491ed230bbd51a6",
      "amount": "1",
      "verified": 1,
      "operator": "0x5cf4ce4bcd9c49c153e644f11d3fed7a64ccc065"
    },
];
```

### getNFTTransfersByBlock

Retrieve NFT transfers by block number or block hash. Returns an array of NFT transfers (asynchronous).

#### Options:

- `chain`(optional): The blockchain to get data from. Valid values are listed on [Supported Chains](supported-chains.md). Default value `Eth`.
- `block_number_or_hash` (required): The block hash or block number.

{% tabs %}
{% tab title="JS" %}

```javascript
const options = { chain: "bsc", block_number_or_hash: "11284830" };

// get NFT transfers by block number or block hash
const NFTTransfers = await Moralis.Web3API.native.getNFTTransfersByBlock(
  options
);
```

{% endtab %}

{% tab title="React" %}

```javascript
import React from "react";
import { useMoralisWeb3Api } from "react-moralis";

const Web3Api = useMoralisWeb3Api();

const fetchNFTTransfersByBlock = async () => {
  // get NFT transfers by block number or block hash
  const options = { chain: "bsc", block_number_or_hash: "11284830" };

  const NFTTransfers = await Web3Api.native.getNFTTransfersByBlock(options);
  console.log(NFTTransfers);
};
```

{% endtab %}

{% tab title="curl" %}

```bash
curl -X 'GET' \
  'https://deep-index.moralis.io/api/v2/block/11284830/nft/transfers?chain=bsc&limit=500' \
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
    public async void fetchNFTTransfersByBlock()
    {
        NftTransferCollection nftTransfers = await Moralis.Web3Api.Native.GetNFTTransfersByBlock(blockNumberOrHash: "11284830", ChainList.bsc);
        Debug.Log(nftTransfers.ToJson());
    }
}
```

{% endtab %}
{% endtabs %}

#### Example result:

```javascript
[
  {
    amount: "1",
    block_hash:
      "0x4c00bcee8900e5fc4f6a5d11dff6edca3b97470da290986940da1d2f0ef221fb",
    block_number: "11284830",
    block_timestamp: "2021-09-27T17:07:54.000Z",
    contract_type: "ERC721",
    from_address: "0x6102f780a5c51dee8ce46c5db5b62d88119d559d",
    log_index: 130,
    to_address: "0xd95f7419ce0debf3a991ef9944808010928db319",
    token_address: "0xcf58705e2ff8642c4276c2dd2747ec8af5bafc1d",
    token_id: "281",
    transaction_hash:
      "0x19721cf6855f412f82de87f8c6050e6de3cc77a81388002bc24efea53bd36a0f",
    transaction_index: 35,
    transaction_type: "Single",
  },
];
```

### getAllTokenIds

Returns an object with a number of NFTs and an array with NFT metadata (name, symbol) for a given token contract address (asynchronous).

#### Options:

- `chain`(optional): The blockchain to get data from. Valid values are listed on [Supported Chains](supported-chains.md). Default value `Eth`.
- `format` (optional): The format of the token id. Available values : `decimal`, `hex`. Default value : `decimal.`
- `cursor` (optional): The next page of data to retrieve. Next page cursor value returned from each request.
- `limit`(optional): limit (max 100).
- `address`(required): The address of the token contract.

{% tabs %}
{% tab title="JS" %}

```javascript
const options = {
  address: "0x7dE3085b3190B3a787822Ee16F23be010f5F8686",
  chain: "eth",
};
const NFTs = await Moralis.Web3API.token.getAllTokenIds(options);
```

{% endtab %}

{% tab title="React" %}

```javascript
import React from "react";
import { useMoralisWeb3Api } from "react-moralis";

const Web3Api = useMoralisWeb3Api();

const fetchAllTokenIds = async () => {
  const options = {
    address: "0x7dE3085b3190B3a787822Ee16F23be010f5F8686",
    chain: "eth",
  };
  const NFTs = await Web3Api.token.getAllTokenIds(options);
  console.log(NFTs);
};
```

{% endtab %}

{% tab title="curl" %}

```bash
curl -X 'GET' \
  'https://deep-index.moralis.io/api/v2/nft/0x7dE3085b3190B3a787822Ee16F23be010f5F8686?chain=eth&format=decimal' \
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
    public async void fetchAllTokenIds()
    {
        NftCollection nfts = await Moralis.Web3Api.Token.GetAllTokenIds(address: "0x7dE3085b3190B3a787822Ee16F23be010f5F8686", ChainList.eth);
        Debug.Log(nfts.ToJson());
    }
}
```

{% endtab %}
{% endtabs %}

#### Example result:

```javascript
[
  {
    token_address: "0x7de3085b3190b3a787822ee16f23be010f5f8686",
    token_id: "728",
    amount: "1",
    contract_type: "ERC721",
    name: "Baby Ape Mutant Club",
    symbol: "BAMC",
    token_uri:
      "https://gateway.moralisipfs.com/ipfs/QmajSqgxY3cWBgBeRm38vasJAcTit1kp5EwqVHxszJYgUC/728.json",
    metadata:
      '{\n  "name": "Baby Mutant #728",\n  "description": "",\n  "image": "ipfs://QmPUDVLP9W1pWpCTpGvpPbMu4nVpCuu2A7M6tQovDpVDoD/728.png",\n  "dna": "172bef0b78106072e4eacb26db57ae70fb17b37b",\n  "edition": 728,\n  "date": 1645023568505,\n  "artist": "Skurvydogg",\n  "attributes": [\n    {\n      "trait_type": "BackGrounds",\n      "value": "Putrid_Purple"\n    },\n    {\n      "trait_type": "Furs",\n      "value": "Red_rum"\n    },\n    {\n      "trait_type": "Eyes",\n      "value": "Robot_M1"\n    },\n    {\n      "trait_type": "Hats",\n      "value": "Bunny_Ears_M2"\n    },\n    {\n      "trait_type": "Mouths",\n      "value": "Xenomorf_M1"\n    }\n  ]\n}',
    synced_at: "2022-03-05T02:29:18.441Z",
  },
];
```

### getContractNFTTransfers

Returns an object with number of NFT transfers and an array with NFT transfers for a given token contract address (asynchronous).

#### Options:

- `chain`(optional): The blockchain to get data from. Valid values are listed on [Supported Chains](supported-chains.md). Default value `Eth`.
- `format` (optional): The format of the token id. Available values : `decimal`, `hex`. Default value is `decimal.`
- `cursor` (optional): The next page of data to retrieve. Next page cursor value returned from each request.
- `limit`(optional): limit (max 100).
- `address`(required): Address of the contract

{% tabs %}
{% tab title="JS" %}

```javascript
const options = {
  address: "0x7de3085b3190b3a787822ee16f23be010f5f8686",
  chain: "eth",
};
const nftTransfers = await Moralis.Web3API.token.getContractNFTTransfers(
  options
);
```

{% endtab %}

{% tab title="React" %}

```javascript
import React from "react";
import { useMoralisWeb3Api } from "react-moralis";

const Web3Api = useMoralisWeb3Api();

const fetchContractNFTTransfers = async () => {
  const options = {
    address: "0x7de3085b3190b3a787822ee16f23be010f5f8686",
    chain: "eth",
  };
  const nftTransfers = await Web3Api.token.getContractNFTTransfers(options);
  console.log(NFTs);
};
```

{% endtab %}

{% tab title="curl" %}

```bash
curl -X 'GET' \
  'https://deep-index.moralis.io/api/v2/nft/0x7de3085b3190b3a787822ee16f23be010f5f8686/1/transfers?chain=eth&format=decimal' \
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
    public async void fetchContractNFTTransfers()
    {
        NftTransferCollection nftTransfers = await Moralis.Web3Api.Token.GetContractNFTTransfers(address: "0x7de3085b3190b3a787822ee16f23be010f5f8686", ChainList.eth);
        Debug.Log(nftTransfers.ToJson());
    }
}
```

{% endtab %}
{% endtabs %}

#### Example result:

```javascript
[
  {
    block_number: "14238158",
    block_timestamp: "2022-02-19T18:49:02.000Z",
    block_hash:
      "0x8124f4a126996d306a90fa00b871b6ed9669a1a9806106b34fa28f3de61e5f8b",
    transaction_hash:
      "0x2db36892fc17bf99a3f6dd8a639f3c704f772858f3961cbcd26b3a42a2cd561e",
    transaction_index: 162,
    log_index: 419,
    value: "0",
    contract_type: "ERC721",
    transaction_type: "Single",
    token_address: "0x7de3085b3190b3a787822ee16f23be010f5f8686",
    token_id: "1",
    from_address: "0x0000000000000000000000000000000000000000",
    to_address: "0x324fb4a58674758e00c3a49409b815de1398bfe8",
    amount: "1",
    verified: 1,
    operator: null,
  },
];
```

### getNFTLowestPrice

Returns an object with the lowest price found for a NFT token contract for the last x days (only trades paid in ETH)

#### Options:

- `chain`(optional): The blockchain to get data from. Valid values are listed on [Supported Chains](supported-chains.md). Default value `Eth`.
- `days` (optional): The number of days to look back to find the lowest price If not provided 7 days will be the default
- `marketplace` (optional): Marketplace from where to get the trades (only opensea is supported at the moment).
- `address` (required): Address of the contract(i.e. `0x1a2b3x...`).

{% tabs %}
{% tab title="JS" %}

```javascript
const options = {
  address: "0x7de3085b3190b3a787822ee16f23be010f5f8686",
  days: "3",
};
const NFTLowestPrice = await Moralis.Web3API.token.getNFTLowestPrice(options);
```

{% endtab %}

{% tab title="React" %}

```javascript
import React from "react";
import { useMoralisWeb3Api } from "react-moralis";

const Web3Api = useMoralisWeb3Api();

const fetchNFTLowestPrice = async () => {
  const options = {
    address: "0x7de3085b3190b3a787822ee16f23be010f5f8686",
    days: "3",
  };
  const NFTLowestPrice = await Web3Api.token.getNFTLowestPrice(options);
  console.log(NFTLowestPrice);
};
```

{% endtab %}

{% tab title="curl" %}

```bash
curl -X 'GET' \
  'https://deep-index.moralis.io/api/v2/nft/0x7de3085b3190b3a787822ee16f23be010f5f8686/lowestprice?chain=eth&days=3&marketplace=opensea' \
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
    public async void fetchNFTLowestPrice()
    {
        Trade NFTLowestPrice = await Moralis.Web3Api.Token.GetNFTLowestPrice(address: "0x7de3085b3190b3a787822ee16f23be010f5f8686", ChainList.eth, days: 2);
        Debug.Log(NFTLowestPrice.ToJson());
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
      "0xda5f3337eb74b56e99aebb5d9ebfc1e82b1c0aacc3b9b7cc606d4112ba12801d",
    transaction_index: "168",
    token_ids: ["254"],
    seller_address: "0x8a04cea099e2d2886aea08e33446b5a52b9cf900",
    buyer_address: "0xcf806faa913b3ac7bcb7b57b1199411a76d96646",
    token_address: "0x7de3085b3190b3a787822ee16f23be010f5f8686",
    marketplace_address: "0x7be8076f4ea4a4ad08075c2508e481d6c946d12b",
    price: "0",
    block_timestamp: "2022-02-26T17:19:23.000Z",
    block_number: "14283046",
    block_hash:
      "0x997713ef27375f735f7017824958c0f0093236fe7515f8759bead1ce21d554fb",
  },
];
```

### getNFTMetadata

Returns the contract level metadata (name, symbol, base token uri) for the given contract (asynchronous).

#### Options:

- `chain`(optional): The blockchain to get data from. Valid values are listed on [Supported Chains](supported-chains.md). Default value `Eth`.
- `address`(required): The address of the token contract.

{% tabs %}
{% tab title="JS" %}

```javascript
const options = {
  address: "0x7dE3085b3190B3a787822Ee16F23be010f5F8686",
  chain: "eth",
};
const metaData = await Moralis.Web3API.token.getNFTMetadata(options);
```

{% endtab %}

{% tab title="React" %}

```javascript
import React from "react";
import { useMoralisWeb3Api } from "react-moralis";

const Web3Api = useMoralisWeb3Api();

const fetchNFTMetadata = async () => {
  const options = {
    address: "0x7dE3085b3190B3a787822Ee16F23be010f5F8686",
    chain: "eth",
  };
  const metaData = await Web3Api.token.getNFTMetadata(options);
  console.log(metaData);
};
```

{% endtab %}

{% tab title="curl" %}

```bash
curl -X 'GET' \
  'https://deep-index.moralis.io/api/v2/nft/0x7dE3085b3190B3a787822Ee16F23be010f5F8686/metadata?chain=eth' \
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
    public async void fetchNFTMetadata()
    {
        NftContractMetadata metadata = await Moralis.Web3Api.Token.GetNFTMetadata(address: "0x7dE3085b3190B3a787822Ee16F23be010f5F8686", ChainList.eth);
        Debug.Log(metadata.ToJson());
    }
}
```

{% endtab %}
{% endtabs %}

{% hint style="info" %}
Requests for contract addresses not yet indexed will automatically start the indexing process for that NFT collection
{% endhint %}

#### Example result:

```javascript
{
  "token_address": "0x7de3085b3190b3a787822ee16f23be010f5f8686",
  "name": "Baby Ape Mutant Club",
  "symbol": "BAMC",
  "contract_type": "ERC721",
  "synced_at": "2022-02-19"
}
```

### getNFTOwners

Returns an object with a number of NFT owners and an array with NFT metadata (name, symbol) for a given token contract address (asynchronous).

#### Options:

- `chain`(optional): The blockchain to get data from. Valid values are listed on [Supported Chains](supported-chains.md). Default value `Eth`.
- `format` (optional): The format of the token id. Available values : `decimal`, `hex`. Default value : `decimal.`
- `cursor` (optional): The next page of data to retrieve. Next page cursor value returned from each request.
- `limit`(optional): limit (max 100).
- `address`(required): Address of the contract

{% tabs %}
{% tab title="JS" %}

```javascript
const options = {
  address: "0x7de3085b3190b3a787822ee16f23be010f5f8686",
  chain: "eth",
};
const nftOwners = await Moralis.Web3API.token.getNFTOwners(options);
```

{% endtab %}

{% tab title="React" %}

```javascript
import React from "react";
import { useMoralisWeb3Api } from "react-moralis";

const Web3Api = useMoralisWeb3Api();

const fetchNFTOwners = async () => {
  const options = {
    address: "0x7de3085b3190b3a787822ee16f23be010f5f8686",
    chain: "eth",
  };
  const nftOwners = await Web3Api.token.getNFTOwners(options);
  console.log(nftOwners);
};
```

{% endtab %}

{% tab title="curl" %}

```bash
curl -X 'GET' \
  'https://deep-index.moralis.io/api/v2/nft/0x7de3085b3190b3a787822ee16f23be010f5f8686/owners?chain=eth&format=decimal' \
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
    public async void fetchNFTOwners()
    {
        NftOwnerCollection nftowners = await Moralis.Web3Api.Token.GetNFTOwners(address: "0x7de3085b3190b3a787822ee16f23be010f5f8686", ChainList.eth);
        Debug.Log(nftowners.ToJson());
    }
}
```

{% endtab %}
{% endtabs %}

{% hint style="info" %}
Make sure to include a sort parm on a column like token_id for consistent pagination results
{% endhint %}

{% hint style="info" %}
Requests for contract addresses not yet indexed will automatically start the indexing process for that NFT collection
{% endhint %}

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

### getNFTTrades

Returns an object with NFT trades for a given contract and marketplace

#### Options:

- `chain`(optional): The blockchain to get data from. Valid values are listed on [Supported Chains](supported-chains.md). Default value `Eth`.
- `from_date` (optional): The date from where to get the trades(any format that is accepted by momentjs). Provide the param 'from_block' or 'from_date' If 'from_date' and 'from_block' are provided, 'from_block' will be used.
- `to_date` (optional): Get the trades to this date (any format that is accepted by momentjs). Provide the param 'to_block' or 'to_date' If 'to_date' and 'to_block' are provided, 'to_block' will be used.
- `from_block` (optional): The minimum block number from where to get the tradesProvide the param 'from_block' or 'from_date' If 'from_date' and 'from_block' are provided, 'from_block' will be used.
- `to_block` (optional): The maximum block number from where to get the trades. Provide the param 'to_block' or 'to_date' If 'to_date' and 'to_block' are provided, 'to_block' will be used.
- `cursor` (optional): The next page of data to retrieve. Next page cursor value returned from each request.
- `limit`(optional): limit (max 100).
- `marketplace` (optional): Marketplace from where to get the trades (only opensea is supported at the moment).
- `address` (required): Address of the contract(i.e. `0x1a2b3x...`).

{% tabs %}
{% tab title="JS" %}

```javascript
const options = {
  address: "0x7de3085b3190b3a787822ee16f23be010f5f8686",
  limit: "10",
  chain: "eth",
};
const NFTTrades = await Moralis.Web3API.token.getNFTTrades(options);
```

{% endtab %}

{% tab title="React" %}

```javascript
import React from "react";
import { useMoralisWeb3Api } from "react-moralis";

const Web3Api = useMoralisWeb3Api();

const fetchNFTTrades = async () => {
  const options = {
    address: "0x7de3085b3190b3a787822ee16f23be010f5f8686",
    limit: "10",
    chain: "eth",
  };
  const NFTTrades = await Web3Api.token.getNFTTrades(options);
  console.log(NFTTrades);
};
```

{% endtab %}

{% tab title="curl" %}

```bash
curl -X 'GET' \
  'https://deep-index.moralis.io/api/v2/nft/0x7de3085b3190b3a787822ee16f23be010f5f8686/trades?chain=eth&marketplace=opensea' \
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
    public async void fetchNFTTrades()
    {
        TradeCollection trades = await Moralis.Web3Api.Token.GetNFTTrades(address: "0x7de3085b3190b3a787822ee16f23be010f5f8686", ChainList.eth, limit: 10);
        Debug.Log(trades.ToJson());
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
      "0x4de0bcef1450492bd5c2e7693cf644c40005868d0dcc8a7a50a80ef2efa88d1e",
    transaction_index: "164",
    token_ids: ["16404"],
    seller_address: "0xbae90f486d751f133702655627ce599249cd26b8",
    buyer_address: "0x8795e90de359c1e0bf2579646486f7f12f270d2f",
    token_address: "0xdf7952b35f24acf7fc0487d01c8d5690a60dba07",
    marketplace_address: "0x7be8076f4ea4a4ad08075c2508e481d6c946d12b",
    price: "280000000000000000",
    price_token_address: null,
    block_timestamp: "2021-05-09T23:00:25.000Z",
    block_number: "7281522",
    block_hash:
      "0xe870c197b0c614e055f4de5b264bc7c69eafc93a6d0ce300309de444b2ff7e3a",
  },
];
```

### getTokenIdMetadata

Returns data, including fully resolved metadata for the given token id of the given contract address (asynchronous).

#### Options:

- `chain`(optional): The blockchain to get data from. Valid values are listed on [Supported Chains](supported-chains.md). Default value `Eth`.
- `format` (optional): The format of the token id. Available values : `decimal`, `hex`. Default value is `decimal.`
- `address`(required): Address of the contract
- `token_id` (required): The id of the token

{% tabs %}
{% tab title="JS" %}

```javascript
const options = {
  address: "0x7de3085b3190b3a787822ee16f23be010f5f8686",
  token_id: "1",
  chain: "eth",
};
const tokenIdMetadata = await Moralis.Web3API.token.getTokenIdMetadata(options);
```

{% endtab %}

{% tab title="React" %}

```javascript
import React from "react";
import { useMoralisWeb3Api } from "react-moralis";

const Web3Api = useMoralisWeb3Api();

const fetchTokenIdMetadata = async () => {
  const options = {
    address: "0x7de3085b3190b3a787822ee16f23be010f5f8686",
    token_id: "1",
    chain: "eth",
  };
  const tokenIdMetadata = await Web3Api.token.getTokenIdMetadata(options);
  console.log(tokenIdMetadata);
};
```

{% endtab %}

{% tab title="curl" %}

```bash
curl -X 'GET' \
  'https://deep-index.moralis.io/api/v2/nft/0x7de3085b3190b3a787822ee16f23be010f5f8686/1?chain=eth&format=decimal' \
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
    public async void fetchTokenIdMetadata()
    {
        Nft tokenIdMetadata = await Moralis.Web3Api.Token.GetTokenIdMetadata(address: "0x7de3085b3190b3a787822ee16f23be010f5f8686", tokenId: "1", ChainList.eth);
        Debug.Log(tokenIdMetadata.ToJson());
    }
}
```

{% endtab %}
{% endtabs %}

#### Example result:

```javascript
{
  token_address: "0x7de3085b3190b3a787822ee16f23be010f5f8686",
  token_id: "1",
  block_number_minted: "14238158",
  owner_of: "0x324fb4a58674758e00c3a49409b815de1398bfe8",
  block_number: "14238158",
  amount: "1",
  contract_type: "ERC721",
  name: "Baby Ape Mutant Club",
  symbol: "BAMC",
  token_uri: "https://ipfs.moralis.io:2053/ipfs/QmYuHVS98nZxzYmuk9E6HA1TNr2W6PpV4fvE4MDfCFgfCP",
  metadata: "{\n  \"name\": \"Baby Ape Mutant Club\",\n  \"description\": \"Wait for reveal\",\n  \"image\": \"ipfs://QmPS5CMBd9Zwidies964iHb9hcdDmXZkWBGZyaJ9c1Gmif\"\n}\n",
  synced_at: "2022-02-19T18:50:43.217Z",
  is_valid: 1,
  syncing: 2,
  frozen: 0
}
```

### getTokenIdOwners

Returns an object with number of NFT transfers and an array with all owners of NFT items within a given contract collection (asynchronous).

#### Options:

- `chain`(optional): The blockchain to get data from. Valid values are listed on [Supported Chains](supported-chains.md). Default value `Eth`.
- `format` (optional): The format of the token id. Available values : `decimal`, `hex`. Default value is `decimal`
- `cursor` (optional): The next page of data to retrieve. Next page cursor value returned from each request.
- `limit`(optional): limit (max 100).
- `address`(required): Address of the contract.
- `token_id`(required): The id of the token.

{% tabs %}
{% tab title="JS" %}

```javascript
const options = {
  address: "0x7de3085b3190b3a787822ee16f23be010f5f8686",
  token_id: "1",
  chain: "eth",
};
const tokenIdOwners = await Moralis.Web3API.token.getTokenIdOwners(options);
```

{% endtab %}

{% tab title="React" %}

```javascript
import React from "react";
import { useMoralisWeb3Api } from "react-moralis";

const Web3Api = useMoralisWeb3Api();

const fetchTokenIdOwners = async () => {
  const options = {
    address: "0x7de3085b3190b3a787822ee16f23be010f5f8686",
    token_id: "1",
    chain: "eth",
  };
  const tokenIdOwners = await Web3Api.token.getTokenIdOwners(options);
  console.log(tokenIdOwners);
};
```

{% endtab %}

{% tab title="curl" %}

```bash
curl -X 'GET' \
  'https://deep-index.moralis.io/api/v2/nft/0x7de3085b3190b3a787822ee16f23be010f5f8686/1/owners?chain=eth&format=decimal' \
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
    public async void fetchTokenIdOwners()
    {
        NftOwnerCollection tokenIdOwners = await Moralis.Web3Api.Token.GetTokenIdOwners(address: "0x7de3085b3190b3a787822ee16f23be010f5f8686", tokenId: "1", ChainList.eth);
        Debug.Log(tokenIdOwners.ToJson());
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

### getWalletTokenIdTransfers

Returns an object with number of NFT transfers and an array with all transfers of NFT by token id (asynchronous).

#### Options:

- `chain`(optional): The blockchain to get data from. Valid values are listed on [Supported Chains](supported-chains.md). Default value `Eth`.
- `format` (optional): The format of the token id. Available values : `decimal`, `hex`. Default value is `decimal`
- `cursor` (optional): The next page of data to retrieve. Next page cursor value returned from each request.
- `limit`(optional): limit (max 100).
- `address`(required): Address of the contract.
- `token_id`(required): The id of the token.

{% tabs %}
{% tab title="JS" %}

```javascript
const options = {
  address: "0x7de3085b3190b3a787822ee16f23be010f5f8686",
  token_id: "1",
  chain: "eth",
};
const transfers = await Moralis.Web3API.token.getWalletTokenIdTransfers(
  options
);
```

{% endtab %}

{% tab title="React" %}

```javascript
import React from "react";
import { useMoralisWeb3Api } from "react-moralis";

const Web3Api = useMoralisWeb3Api();

const fetchWalletTokenIdTransfers = async () => {
  const options = {
    address: "0x7de3085b3190b3a787822ee16f23be010f5f8686",
    token_id: "1",
    chain: "eth",
  };
  const transfers = await Web3Api.token.getWalletTokenIdTransfers(options);
  console.log(transfers);
};
```

{% endtab %}

{% tab title="curl" %}

```bash
curl -X 'GET' \
  'https://deep-index.moralis.io/api/v2/nft/0x7de3085b3190b3a787822ee16f23be010f5f8686/1/transfers?chain=eth&format=decimal' \
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
    public async void fetchWalletTokenIdTransfers()
    {
        NftTransferCollection transfers = await Moralis.GetClient().Web3Api.Token.GetWalletTokenIdTransfers(address: "0x7de3085b3190b3a787822ee16f23be010f5f8686", tokenId: "1", ChainList.eth);
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
    block_number: "14238158",
    block_timestamp: "2022-02-19T18:49:02.000Z",
    block_hash:
      "0x8124f4a126996d306a90fa00b871b6ed9669a1a9806106b34fa28f3de61e5f8b",
    transaction_hash:
      "0x2db36892fc17bf99a3f6dd8a639f3c704f772858f3961cbcd26b3a42a2cd561e",
    transaction_index: 162,
    log_index: 419,
    value: "0",
    contract_type: "ERC721",
    transaction_type: "Single",
    token_address: "0x7de3085b3190b3a787822ee16f23be010f5f8686",
    token_id: "1",
    from_address: "0x0000000000000000000000000000000000000000",
    to_address: "0x324fb4a58674758e00c3a49409b815de1398bfe8",
    amount: "1",
    verified: 1,
    operator: null,
  },
];
```
