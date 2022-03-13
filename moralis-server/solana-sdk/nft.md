# NFT

## getNFTMetadata

Returns the metadata of a SPL NFT.

#### Options:

- `network`: The network cluster to get data from. Valid values are listed on the [intro page in Supported Networks section](https://docs.moralis.io/moralis-server/solana-sdk/intro#supported-networks). Default value `mainnet`.
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
{% endtab %}
{% tab title="Unity"%}

```cs
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
