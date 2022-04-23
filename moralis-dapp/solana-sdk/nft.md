---
description: You can query NFTs and get their metadata on Solana
---

# NFT

All the methods extend from **Moralis.SolanaAPI.nft**

### getNFTMetadata

Returns the metadata of a SPL NFT.

#### Options:

- `network`: The network cluster to get data from. Valid values are listed on the [Supported Networks](supported-networks.md). Default value `mainnet`.
- `address`: A SPL NFT address (i.e. `HsXZnAba2...`).

{% tabs %}
{% tab title="JS" %}

```javascript
// get devnet metadata for a given SPL NFT address
const options = {
  network: "devnet",
  address: "6XU36wCxWobLx5Rtsb58kmgAJKVYmMVqy4SHXxENAyAe",
};
const nftMetadata = await Moralis.SolanaAPI.nft.getNFTMetadata(options);
```

<<<<<<< HEAD
=======
<<<<<<< HEAD
=======
>>>>>>> 5f27048d33c8d54d958de32876f3ebf22dc5400f
{% endtab %}
{% tab title="React" %}

```javascript
import { useMoralisSolanaApi, useMoralisSolanaCall } from "react-moralis";

const { nft } = useMoralisSolanaApi();

// get devnet SPL NFT metadata for a given address
const options = {
  network: "devnet",
  address: "6XU36wCxWobLx5Rtsb58kmgAJKVYmMVqy4SHXxENAyAe",
};
const { fetch, data, isLoading } = useMoralisSolanaCall(
  nft.getNFTMetadata,
  options
);
```

{% endtab %}
{% tab title="curl" %}

```bash
curl -X 'GET' \
  'https://solana-gateway.moralis.io/nft/devnet/6XU36wCxWobLx5Rtsb58kmgAJKVYmMVqy4SHXxENAyAe/metadata' \
  -H 'accept: application/json' \
  -H 'X-API-Key: MY-API-KEY'
```

<<<<<<< HEAD
=======
>>>>>>> 6e9ea3f9ef5206577c91bb54795746c7bc117f61
>>>>>>> 5f27048d33c8d54d958de32876f3ebf22dc5400f
{% endtab %}

{% tab title="Unity" %}

```csharp
using System.Collections.Generic;
using Moralis.SolanaApi.Models;
using Moralis.SolanaApi;
using MoralisWeb3ApiSdk;

  // get mainnet metadata for a given SPL NFT address
  public async void GetSPLNftMetadata()
  {
    NftMetadata nftmetadata = await MoralisSolanaClient.SolanaApi.Nft.GetNFTMetadata(NetworkTypes.mainnet, "6XU36wCxWobLx5Rtsb58kmgAJKVYmMVqy4SHXxENAyAe");
    print(nftmetadata);
  }
```

{% endtab %}
{% endtabs %}

#### Example result:

```javascript
{
  "mint": "string",
  "standard": "string",
  "name": "string",
  "symbol": "string",
  "metaplex": {
    "metadataUri": "string",
    "masterEdition": true,
    "isMutable": true,
    "primarySaleHappened": true,
    "sellerFeeBasisPoints": 0,
    "updateAuthority": "string"
  }
}
```
