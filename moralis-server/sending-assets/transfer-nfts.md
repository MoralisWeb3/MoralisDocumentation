# Transfer NFTs

## Transferring ERC721 Tokens (Non-Fungible)

When sending an ERC721 token you have to specify the `contractAddress `of the NFT and the specific `tokenId `you are interested in.

As it's only possible to transfer one ERC721 at a time, no `amount `is needed.

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

## Transferring ERC1155 Tokens (Semi-Fungible)

When sending an ERC1155 token you have to specify the `contractAddress `of the NFT and the specific`tokenId`you are interested in. Additionally, you have to specify the`amount`of the token you want to transfer.

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

`Moralis.transfer()` returns a [transaction response](https://docs.ethers.io/v5/api/providers/types/#providers-TransactionResponse). This object contains all data about the transaction. If you need data about the **result** of the transation, then you need to wait for the transaction to be **confirmed**.

You can do this via `transaction.wait()` to wait for 1 confirmation (or `transaction.wait(5)` to wait for 5 confirmations for example):

```javascript
const transaction = await Moralis.transfer(options);
const result = await transaction.wait();
```