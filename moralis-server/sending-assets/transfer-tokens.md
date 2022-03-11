# Transfer Tokens

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