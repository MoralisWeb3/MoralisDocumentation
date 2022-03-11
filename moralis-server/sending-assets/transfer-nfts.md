---
description: >-
  Transfer NFTs on any blockchain - ETH (Ethereum), BNB (Binance Smart Chain),
  MATIC  (Polygon)
---

# 🖼 Transfer NFTs

### Transferring ERC721 Tokens (Non-Fungible)

To transfer [ERC721](https://ethereum.org/en/developers/docs/standards/tokens/erc-721/) tokens, follow the steps:&#x20;

1. Construct an`options`object and set
   1. `type:"erc721"`&#x20;
   2. `receiver: "0x000..." //wallet address`
   3. `contractAddress: "0x..." //contract of the ERC721 token`
   4. `tokenId: 1` &#x20;
2. Call the Moralis transfer function as shown below

{% hint style="info" %}
As it's only possible to transfer one ERC721 at a time, no _**`amount`**_`is`needed.
{% endhint %}

{% tabs %}
{% tab title="JS" %}
```javascript
// sending a token with token id = 1
const options = {
  type: "erc721",
  receiver: "0x..",
  contractAddress: "0xc02aaa39b223fe8d0a0e5c4f27ead9083c756cc2",
  tokenId: 1,
};
let transaction = await Moralis.transfer(options);
```
{% endtab %}

{% tab title="React" %}
```javascript
import React from "react";
import { useWeb3Transfer } from "react-moralis";

const TransferNFT = () => {
  const { fetch, error, isFetching } = useWeb3Transfer({
    type: "erc721",
    receiver: "0x..",
    contractAddress: "0xc02aaa39b223fe8d0a0e5c4f27ead9083c756cc2",
    tokenId: 1,
  });

  return (
    // Use your custom error component to show errors
    <div>
      {error && <ErrorMessage error={error} />}
      <button onClick={() => fetch()} disabled={isFetching}>
        Transfer
      </button>
    </div>
  );
};
```
{% endtab %}
{% endtabs %}

### Transferring ERC1155 Tokens (Semi-Fungible)

To transfer [ERC1155](https://ethereum.org/en/developers/docs/standards/tokens/erc-1155/) tokens, follow the steps:&#x20;

1. Construct an`options`object and set
   1. `type:"erc721"`&#x20;
   2. `receiver: "0x000..." //wallet address`
   3. `contractAddress: "0x..." //contract of the ERC721 token`
   4. `tokenId: 1` &#x20;
   5. `amount: 15 //number of tokens to transfer`
2. Call the Moralis transfer function as shown below

{% tabs %}
{% tab title="JS" %}
```javascript
// sending 15 tokens with token id = 1
const options = {
  type: "erc1155",
  receiver: "0x..",
  contractAddress: "0x..",
  tokenId: 1,
  amount: 15,
};
let transaction = await Moralis.transfer(options);
```
{% endtab %}

{% tab title="React" %}
```javascript
import React from "react";
import { useWeb3Transfer } from "react-moralis";

const TransferNFT = () => {
  const { fetch, error, isFetching } = useWeb3Transfer({
    type: "erc1155",
    receiver: "0x..",
    contractAddress: "0x..",
    tokenId: 1,
    amount: 15,
  });

  return (
    // Use your custom error component to show errors
    <div>
      {error && <ErrorMessage error={error} />}
      <button onClick={() => fetch()} disabled={isFetching}>
        Transfer
      </button>
    </div>
  );
};
```
{% endtab %}
{% endtabs %}

### Resolving the results

`Moralis.transfer()` returns a [transaction response](https://docs.ethers.io/v5/api/providers/types/#providers-TransactionResponse) after it is executed. The below page shows how to consume the data returned.

{% content-ref url="resolve-transfer.md" %}
[resolve-transfer.md](resolve-transfer.md)
{% endcontent-ref %}
