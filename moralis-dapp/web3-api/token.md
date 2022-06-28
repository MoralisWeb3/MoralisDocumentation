# 🎴 Web3API.token

## getTokenMetadata

Returns metadata (name, symbol, decimals, logo) for a given token contract address (asynchronous).

#### Options:

* `chain`(optional): The blockchain to get data from. Valid values are listed on [Supported Chains](supported-chains.md). Default value `Eth`.
* `addresses` (required): The address or an array of addresses to get metadata for

{% tabs %}
{% tab title="JS" %}
```javascript
//Get metadata for one token. Ex: USDT token on ETH
const options = {
  chain: "eth",
  addresses: "0xdAC17F958D2ee523a2206206994597C13D831ec7",
};
const tokenMetadata = await Moralis.Web3API.token.getTokenMetadata(options);

//Get metadata for an array of tokens. Ex: USDT and USDC tokens on BSC
const options = {
  chain: "bsc",
  addresses: [
    "0x55d398326f99059ff775485246999027b3197955",
    "0x0a385f86059e0b2a048171d78afd1f38558121f3",
  ],
};
const tokenMetadata = await Moralis.Web3API.token.getTokenMetadata(options);
```
{% endtab %}

{% tab title="React" %}
```javascript
import React from "react";
import { useMoralisWeb3Api } from "react-moralis";

const Web3Api = useMoralisWeb3Api();

const fetchTokenMetadata = async () => {
  //Get metadata for one token. Ex: USDT token on ETH
  const options = {
    chain: "eth",
    addresses: "0xdAC17F958D2ee523a2206206994597C13D831ec7",
  };
  const tokenMetadata = await Web3Api.token.getTokenMetadata(options);
  console.log(tokenMetadata);

  //Get metadata for an array of tokens. Ex: USDT and USDC tokens on BSC
  const options = {
    chain: "bsc",
    addresses: [
      "0x55d398326f99059ff775485246999027b3197955",
      "0x0a385f86059e0b2a048171d78afd1f38558121f3",
    ],
  };
  const tokenArrayMetadata = await Web3Api.token.getTokenMetadata(options);
  console.log(tokenArrayMetadata);
};
```
{% endtab %}

{% tab title="curl" %}
```bash
curl -X 'GET' \
  'https://deep-index.moralis.io/api/v2/erc20/metadata?chain=bsc&addresses=0x55d398326f99059ff775485246999027b3197955&addresses=0x0a385f86059e0b2a048171d78afd1f38558121f3' \
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
    public async void fetchTokenMetadata()
    {
        List<string> addresses = new List<string>();
        addresses.Add("0xdAC17F958D2ee523a2206206994597C13D831ec7");
        addresses.Add("0x0a385f86059e0b2a048171d78afd1f38558121f3");
        List<Erc20Metadata> resp = await Moralis.Web3Api.Token.GetTokenMetadata(addresses, ChainList.eth);
        foreach (Erc20Metadata erc20metadata in resp)
        {
            Debug.Log(erc20metadata.ToJson());
        }
    }
}
```
{% endtab %}
{% endtabs %}

#### Example result:

```javascript
[
  {
    address: "0x0a385f86059e0b2a048171d78afd1f38558121f3",
    name: "USD Coin on BSC",
    symbol: "USDC",
    logo: null,
    logo_hash: null,
    thumbnail: null,
    decimals: "6",
    block_number: "8242108",
    validated: 1,
    created_at: "2022-01-20T10:41:03.034Z",
  },
  {
    address: "0x55d398326f99059ff775485246999027b3197955",
    name: "Tether USD",
    symbol: "USDT",
    logo: null,
    logo_hash: null,
    thumbnail: null,
    decimals: "18",
    block_number: "8242108",
    validated: 1,
    created_at: "2022-01-20T10:41:03.034Z",
  },
];
```

## getTokenMetadataBySymbol

Returns metadata (name, address, decimals, logo) for given symbols (asynchronous).

#### Options:

* `chain`(optional): The blockchain to get data from. Valid values are listed on [Supported Chains](supported-chains.md). Default value `Eth`.
* `symbols` (required): The token symbol or an array of symbols to get metadata for

{% tabs %}
{% tab title="JS" %}
```javascript
//Get metadata for one token
const options = { chain: "bsc", symbols: "LINK" };
const tokenMetadata = await Moralis.Web3API.token.getTokenMetadataBySymbol(
  options
);

//Get metadata for an array of tokens
const options = { chain: "bsc", symbols: ["LINK", "AAVE"] };
const tokenMetadata = await Moralis.Web3API.token.getTokenMetadataBySymbol(
  options
);
```
{% endtab %}

{% tab title="React" %}
```javascript
import React from "react";
import { useMoralisWeb3Api } from "react-moralis";

const Web3Api = useMoralisWeb3Api();

const fetchTokenMetadataBySymbol = async () => {
  //Get metadata for one token
  const options = { chain: "bsc", symbols: "LINK" };
  const tokenMetadata = await Web3Api.token.getTokenMetadataBySymbol(options);
  console.log(tokenMetadata);

  //Get metadata for an array of tokens
  const options = { chain: "bsc", symbols: ["LINK", "AAVE"] };
  const tokenArrayMetadata = await Web3Api.token.getTokenMetadataBySymbol(
    options
  );
  console.log(tokenArrayMetadata);
};
```
{% endtab %}

{% tab title="curl" %}
```bash
curl -X 'GET' \
  'https://deep-index.moralis.io/api/v2/erc20/metadata/symbols?chain=bsc&symbols=LINK&symbols=AAVE' \
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

    public async void fetchTokenMetadataBySymbol()
    {
        List<string> symbols = new List<string>();
        symbols.Add("AAVE");
        symbols.Add("LINK");
        List<Erc20Metadata> resp = await Moralis.Web3Api.Token.GetTokenMetadataBySymbol(symbols, ChainList.bsc);
        foreach (Erc20Metadata erc20metadata in resp)
        {
            Debug.Log(erc20metadata.ToJson());
        }
    }
}
```
{% endtab %}
{% endtabs %}

#### Example result:

```javascript
[
  {
    address:
      "0x2d30ca6f024dbc1307ac8a1a44ca27de6f797ec22ef20627a1307243b0ab7d09",
    name: "Kylin Network",
    symbol: "KYL",
    decimals: "18",
    logo: "https://cdn.moralis.io/eth/0x67b6d479c7bb412c54e03dca8e1bc6740ce6b99c.png",
    logo_hash:
      "ee7aa2cdf100649a3521a082116258e862e6971261a39b5cd4e4354fcccbc54d",
    thumbnail:
      "https://cdn.moralis.io/eth/0x67b6d479c7bb412c54e03dca8e1bc6740ce6b99c_thumb.png",
    block_number: "string",
    validated: "string",
  },
];
```

## getTokenAllowance

Returns the amount which the spender is allowed to withdraw from the spender (asynchronous).

#### Options:

* `chain`(optional): The blockchain to get data from. Valid values are listed on [Supported Chains](supported-chains.md). Default value `Eth`.
* `owner_address` (required): The address of the token owner
* `spender_address` (required): The address of the token spender
* `address`(required): The address of the token contract

{% tabs %}
{% tab title="JS" %}
```javascript
//Get token allowace on ETH
const options = {
  //token holder
  owner_address: "0xd1628228ffaede220cd583da5f9262355682210a",
  //uniswap v3 router 2 contract address
  spender_address: "0x68b3465833fb72A70ecDF485E0e4C7bD8665Fc45",
  //ENS token contract address
  address: "0xC18360217D8F7Ab5e7c516566761Ea12Ce7F9D72",
};
const allowance = await Moralis.Web3API.token.getTokenAllowance(options);
```
{% endtab %}

{% tab title="React" %}
```javascript
import React from "react";
import { useMoralisWeb3Api } from "react-moralis";

const Web3Api = useMoralisWeb3Api();

const fetchTokenAllowance = async () => {
  //Get token allowace on ETH
  const options = {
    //token holder
    owner_address: "0xd1628228ffaede220cd583da5f9262355682210a",
    //uniswap v3 router 2 contract address
    spender_address: "0x68b3465833fb72A70ecDF485E0e4C7bD8665Fc45",
    //ENS token contract address
    address: "0xC18360217D8F7Ab5e7c516566761Ea12Ce7F9D72",
  };
  const allowance = await Web3Api.token.getTokenAllowance(options);
  console.log(allowance);
};
```
{% endtab %}

{% tab title="curl" %}
```bash
curl -X 'GET' \
  'https://deep-index.moralis.io/api/v2/erc20/0xC18360217D8F7Ab5e7c516566761Ea12Ce7F9D72/allowance?chain=eth&owner_address=0xd1628228ffaede220cd583da5f9262355682210a&spender_address=0x68b3465833fb72A70ecDF485E0e4C7bD8665Fc45' \
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
    public async void fetchTokenMetadataBySymbol()
    {
        Erc20Allowance allowance = await Moralis.Web3Api.Token.GetTokenAllowance(address: "0xC18360217D8F7Ab5e7c516566761Ea12Ce7F9D72", ownerAddress: "0xd1628228ffaede220cd583da5f9262355682210a", spenderAddress: "0x68b3465833fb72A70ecDF485E0e4C7bD8665Fc45", ChainList.eth);
        Debug.Log(allowance.ToJson());
    }
}
```
{% endtab %}
{% endtabs %}

#### Example result:

```javascript
{
  "allowance": "115792089237316195423570985008687907853269984665640564039457584007913129639935"
}
```

## getTokenPrice

Returns the price nominated in the native token and usd for a given token contract address (asynchronous).

#### Options:

* `chain`(optional): The blockchain to get data from. Valid values are listed on [Supported Chains](supported-chains.md). Default value `Eth`.
* `exchange`(optional): The factory name or address of the token exchange. Possible exchanges, for different chains are:\
  ETH mainnet: `uniswap-v3`, `sushiswap`, `uniswap-v2`\
  BSC mainnet: `pancakeswap-v2, pancakeswap-v1`\
  Polygon mainnet: `quickswap`\
  _If no `exchange` is specified, all exchanges are checked (in the order as listed above) until a valid pool has been found. Note that this request can take more time. So specifying the `exchange` will result in faster responses most of the time._
* `address`(required): The address of the token contract
* `to_block`(optional): Returns the price for a given blocknumber (historical price-data)

{% tabs %}
{% tab title="JS" %}
```javascript
//Get token price on PancakeSwap v2 BSC
const options = {
  address: "0x42F6f551ae042cBe50C739158b4f0CAC0Edb9096",
  chain: "bsc",
  exchange: "PancakeSwapv2",
};
const price = await Moralis.Web3API.token.getTokenPrice(options);
```
{% endtab %}

{% tab title="React" %}
```javascript
import React from "react";
import { useMoralisWeb3Api } from "react-moralis";

const Web3Api = useMoralisWeb3Api();

const fetchTokenPrice = async () => {
  //Get token price on PancakeSwap v2 BSC
  const options = {
    address: "0x42F6f551ae042cBe50C739158b4f0CAC0Edb9096",
    chain: "bsc",
    exchange: "PancakeSwapv2",
  };
  const price = await Web3Api.token.getTokenPrice(options);
  console.log(price);
};
```
{% endtab %}

{% tab title="curl" %}
```bash
curl -X 'GET' \
  'https://deep-index.moralis.io/api/v2/erc20/0x42F6f551ae042cBe50C739158b4f0CAC0Edb9096/price?chain=bsc&exchange=PancakeSwapv2' \
  -H 'accept: application/json' \
  -H 'X-API-Key: MY-API-KEY'
```
{% endtab %}
{% endtabs %}

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

Returns an object with a number of NFTs and an array with NFT metadata (name, symbol) for a given token contract address (asynchronous).

#### Options:

* `chain`(optional): The blockchain to get data from. Valid values are listed on [Supported Chains](supported-chains.md). Default value `Eth`.
* `format` (optional): The format of the token id. Available values : `decimal`, `hex`. Default value : `decimal.`
* `offset` (optional): offset.
* `limit`(optional): limit.
* `address`(required): The address of the token contract.

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

## getNFTMetadata

Returns the contract level metadata (name, symbol, base token uri) for the given contract (asynchronous).

#### Options:

* `chain`(optional): The blockchain to get data from. Valid values are listed on [Supported Chains](supported-chains.md). Default value `Eth`.
* `address`(required): The address of the token contract.

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

## getNFTOwners

Returns an object with a number of NFT owners and an array with NFT metadata (name, symbol) for a given token contract address (asynchronous).

#### Options:

* `chain`(optional): The blockchain to get data from. Valid values are listed on [Supported Chains](supported-chains.md). Default value `Eth`.
* `format` (optional): The format of the token id. Available values : `decimal`, `hex`. Default value : `decimal.`
* `offset` (optional): offset.
* `limit`(optional): limit.
* `address`(required): Address of the contract

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
Make sure to include a sort parm on a column like token\_id for consistent pagination results
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

## getNftTransfersFromToBlock

Gets NFT transfers from a block number to a block number

#### Options:

Needs at least one of `from_block`, `to_block` , `from_date` , `to_date`

* `chain`(optional): The blockchain to get data from. Valid values are listed on [Supported Chains](supported-chains.md). Default value `Eth`.
* `from_block`(optional): The minimum block number from where to get the transfers. Provide the param 'from\_block' or 'from\_date'. If 'from\_date' and 'from\_block' are provided, 'from\_block' will be used.
* `to_block` (optional): The maximum block number from where to get the transfers. Provide the param 'to\_block' or 'to\_date', If 'to\_date' and 'to\_block' are provided, 'to\_block' will be used.
* `from_date` (optional): The date from where to get the transfers (any format that is accepted by momentjs). Provide the param 'from\_block' or 'from\_date'. If 'from\_date' and 'from\_block' are provided, 'from\_block' will be used.
* `to_date`(optional): Get transfers up until this date (any format that is accepted by momentjs). Provide the param 'to\_block' or 'to\_date'. If 'to\_date' and 'to\_block' are provided, 'to\_block' will be used.
* `format`(optional): The format of the token id. Available values : `decimal`, `hex`. Default value : `decimal.`
* `limit`(optional): limit

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
Make sure to include a sort parm on a column like token\_id for consistent pagination results
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

## searchNFTs

Very powerful and fast tool for getting the NFT data based on a metadata search (asynchronous).

#### Options:

* `chain`(optional): The blockchain to get data from. Valid values are listed on [Supported Chains](supported-chains.md). Default value `Eth`.
* `format` (optional): The format of the token id. Available values : `decimal`, `hex`. Default value : `decimal.`
* `offset` (optional): offset.
* `limit`(optional): limit.
* `q` (required): The search string parameter
* `filter`(required): What fields the search should match on. To look into the entire metadata set the value to `global`. To have a better response time you can look into a specific field like name. Available values : `name`; `description`; `attributes`; `global`; `name,description`; `name,attributes`; `description,attributes`; `name,description,attributes`

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

## getContractNFTTransfers

Returns an object with number of NFT transfers and an array with NFT transfers for a given token contract address (asynchronous).

#### Options:

* `chain`(optional): The blockchain to get data from. Valid values are listed on [Supported Chains](supported-chains.md). Default value `Eth`.
* `format` (optional): The format of the token id. Available values : `decimal`, `hex`. Default value is `decimal.`
* `offset` (optional): offset.
* `limit`(optional): limit.

\`\`

* `address`(required): Address of the contract

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

## getTokenIdMetadata

Returns data, including fully resolved metadata for the given token id of the given contract address (asynchronous).

#### Options:

* `chain`(optional): The blockchain to get data from. Valid values are listed on [Supported Chains](supported-chains.md). Default value `Eth`.
* `format` (optional): The format of the token id. Available values : `decimal`, `hex`. Default value is `decimal.`
* `address`(required): Address of the contract
* `token_id` (required): The id of the token

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
  "token_address": "0x7de3085b3190b3a787822ee16f23be010f5f8686",
  "token_id": "1",
  "block_number_minted": "14238158",
  "owner_of": "0x324fb4a58674758e00c3a49409b815de1398bfe8",
  "block_number": "14238158",
  "amount": "1",
  "contract_type": "ERC721",
  "name": "Baby Ape Mutant Club",
  "symbol": "BAMC",
  "token_uri": "https://ipfs.moralis.io:2053/ipfs/QmYuHVS98nZxzYmuk9E6HA1TNr2W6PpV4fvE4MDfCFgfCP",
  "metadata": "{\n  \"name\": \"Baby Ape Mutant Club\",\n  \"description\": \"Wait for reveal\",\n  \"image\": \"ipfs://QmPS5CMBd9Zwidies964iHb9hcdDmXZkWBGZyaJ9c1Gmif\"\n}\n",
  "synced_at": "2022-02-19T18:50:43.217Z",
  "is_valid": 1,
  "syncing": 2,
  "frozen": 0
}
```

## getTokenIdOwners

Returns an object with number of NFT transfers and an array with all owners of NFT items within a given contract collection (asynchronous).

#### Options:

* `chain`(optional): The blockchain to get data from. Valid values are listed on [Supported Chains](supported-chains.md). Default value `Eth`.
* `format` (optional): The format of the token id. Available values : `decimal`, `hex`. Default value is `decimal`
* `offset` (optional): offset.
* `limit`(optional): limit.
* `address`(required): Address of the contract.
* `token_id`(required): The id of the token.

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

## getWalletTokenIdTransfers

Returns an object with number of NFT transfers and an array with all transfers of NFT by token id (asynchronous).

#### Options:

* `chain`(optional): The blockchain to get data from. Valid values are listed on [Supported Chains](supported-chains.md). Default value `Eth`.
* `format` (optional): The format of the token id. Available values : `decimal`, `hex`. Default value is `decimal`
* `offset` (optional): offset.
* `limit`(optional): limit.
* `address`(required): Address of the contract.
* `token_id`(required): The id of the token.

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
        NftTransferCollection transfers = await Moralis.Web3Api.Token.GetWalletTokenIdTransfers(address: "0x7de3085b3190b3a787822ee16f23be010f5f8686", tokenId: "1", ChainList.eth);
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

## 🔥 getNFTTrades

Returns an object with NFT trades for a given contract and marketplace

#### Options:

* `chain`(optional): The blockchain to get data from. Valid values are listed on [Supported Chains](supported-chains.md). Default value `Eth`.
* `from_date` (optional): The date from where to get the trades(any format that is accepted by momentjs). Provide the param 'from\_block' or 'from\_date' If 'from\_date' and 'from\_block' are provided, 'from\_block' will be used.
* `to_date` (optional): Get the trades to this date (any format that is accepted by momentjs). Provide the param 'to\_block' or 'to\_date' If 'to\_date' and 'to\_block' are provided, 'to\_block' will be used.
* `from_block` (optional): The minimum block number from where to get the tradesProvide the param 'from\_block' or 'from\_date' If 'from\_date' and 'from\_block' are provided, 'from\_block' will be used.
* `to_block` (optional): The maximum block number from where to get the trades. Provide the param 'to\_block' or 'to\_date' If 'to\_date' and 'to\_block' are provided, 'to\_block' will be used.
* `offset`(optional): Offset.
* `limit`(optional): Limit.
* `marketplace` (optional): Marketplace from where to get the trades (only opensea is supported at the moment).
* `address` (required): Address of the contract(i.e. `0x1a2b3x...`).

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

## 🔥 getNFTLowestPrice

Returns an object with the lowest price found for a NFT token contract for the last x days (only trades paid in ETH)

#### Options:

* `chain`(optional): The blockchain to get data from. Valid values are listed on [Supported Chains](supported-chains.md). Default value `Eth`.
* `days` (optional): The number of days to look back to find the lowest price If not provided 7 days will be the default
* `marketplace` (optional): Marketplace from where to get the trades (only opensea is supported at the moment).
* `address` (required): Address of the contract(i.e. `0x1a2b3x...`).

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

## reSyncMetadata

Resync the metadata for a given token\_id

#### Options:

* `chain`(optional): The blockchain to get data from. Valid values are listed on [Supported Chains](supported-chains.md). Default value `Eth`.
* `address`(required): Address of the contract.
* `token_id`(required): The id of the token.
* `flag` (optional): The type of resync to operate. Available values : `uri`, `metadata`. Default value : `metadata.` The metadata flag will resync of the metadata of nft. The uri flag will resync the token\_uri of NFT.

{% tabs %}
{% tab title="JS" %}
```javascript
const options = {
  address: "0x7de3085b3190b3a787822ee16f23be010f5f8686",
  token_id: "1",
  flag: "metadata",
};
const NFTLowestPrice = await Moralis.Web3API.token.reSyncMetadata(options);
```
{% endtab %}

{% tab title="React" %}
```javascript
import React from "react";
import { useMoralisWeb3Api } from "react-moralis";

const Web3Api = useMoralisWeb3Api();

const reSyncMetadata = async () => {
  const options = {
    address: "0x7de3085b3190b3a787822ee16f23be010f5f8686",
    token_id: "1",
    flag: "metadata",
  };
  const result = await Web3Api.token.reSyncMetadata(options);
  console.log(result);
};
```
{% endtab %}

{% tab title="curl" %}
```bash
curl -X 'PUT' \
  'https://deep-index.moralis.io/api/v2/nft/0x7de3085b3190b3a787822ee16f23be010f5f8686/sync?chain=eth' \
  -H 'accept: */*' \
  -H 'X-API-Key: MY-API-KEY'
```
{% endtab %}

{% tab title="Unity" %}
```csharp
using System.Collections.Generic;
using MoralisUnity.Web3Api.Models;
using MoralisUnity;

  public async void resyncMetadata()
  {
    await Moralis.Web3Api.Token.ReSyncMetadata(address: "0x7de3085b3190b3a787822ee16f23be010f5f8686", tokenId: "1", chain: ChainList.eth);
  }
```
{% endtab %}
{% endtabs %}

#### Example result:

```javascript
{
  status: "Request initiated";
}
```
