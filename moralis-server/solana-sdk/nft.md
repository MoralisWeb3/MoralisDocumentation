# NFT

## getNFTMetadata

Returns the metadata of a SPL NFT.

#### Options:

- `network`: The network cluster to get data from. Valid values are listed on the [intro page in Supported Networks section](https://docs.moralis.io/moralis-server/solana-sdk/intro). Default value `mainnet`.
- `address`: A SPL NFT address (i.e. `HsXZnAba2...`).

```javascript
// get devnet metadata for a given SPL NFT address
const options = {
  network: "devnet",
  address: "HsXZnAba2...",
};
const nftMetadata = await Moralis.SolanaAPI.nft.getNFTMetadata(options);
```

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
