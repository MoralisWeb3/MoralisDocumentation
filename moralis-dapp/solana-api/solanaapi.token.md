---
description: You can query token-related data on Solana
---

# 🪙 SolanaAPI.token

All the methods extend from **Moralis.SolanaAPI.token**

### getTokenPrice

Returns the metadata of a SPL NFT.

#### Options:

* `network`: The network cluster to get data from. Valid values are listed on the [Supported Networks](supported-networks.md). Default value `mainnet`.
* `address`: A SPL Token address (i.e. `HsXZnAba2...`).

{% tabs %}
{% tab title="JS" %}
```javascript
// get devnet metadata for a given SPL NFT address
const options = {
  network: "mainnet",
  address: "4k3Dyjzvzp8eMZWUXbBCjEvwSkkk59S5iCNLY3QrkX6R",
};
const nftMetadata = await Moralis.SolanaAPI.token.getTokenPrice(options);
```
{% endtab %}

{% tab title="React" %}
```javascript
import { useMoralisSolanaApi, useMoralisSolanaCall } from "react-moralis";

const { nft } = useMoralisSolanaApi();

// get devnet SPL NFT metadata for a given address
const options = {
  network: "mainnet",
  address: "4k3Dyjzvzp8eMZWUXbBCjEvwSkkk59S5iCNLY3QrkX6R",
};
const { fetch, data, isLoading } = useMoralisSolanaCall(
  token.getTokenPrice,
  options
);
```
{% endtab %}

{% tab title="curl" %}
```bash
curl -X 'GET' \
  'https://solana-gateway.moralis.io/token/mainnet/4k3Dyjzvzp8eMZWUXbBCjEvwSkkk59S5iCNLY3QrkX6R/price' \
  -H 'accept: application/json' \
  -H 'X-API-Key: MY-API-KEY'
```
{% endtab %}
{% endtabs %}

#### Example result:

```javascript
{
  "usdPrice": 0.8892,
  "exchangeName": "Raydium",
  "exchangeAddress": "675kPX9MHTjS2zt1qfr1NYHuzeLXfQM9H24wFSUt1Mp8",
  "nativePrice": {
    "value": "20767681",
    "symbol": "WSOL",
    "name": "Wrapped Solana",
    "decimals": 9
  }
}
```
