---
description: >-
  Transfer ERC20 tokens on any blockchain - ETH (Ethereum), BNB (Binance Smart
  Chain), MATIC  (Polygon)
---

# 🎴 Transfer Tokens

### Transferring ERC20 Tokens

To transfer ERC20 tokens, follow the steps:&#x20;

1. Construct an`options`object and set
   1. `type:"erc20"`&#x20;
   2. `amount: Moralis.Units.Token("0.5", "18") //Converts to`[`wei`](https://ethdocs.org/en/latest/ether.html#denominations)``
   3. `receiver: "0x000..." //wallet address`
   4. `contractAddress: "0x..." //contract of the ERC20 token`
2. Call the Moralis transfer function as shown below

{% tabs %}
{% tab title="JS" %}
```javascript
// sending 0.5 tokens with 18 decimals
const options = {
  type: "erc20",
  amount: Moralis.Units.Token("0.5", "18"),
  receiver: "0x..",
  contractAddress: "0x..",
};
let result = await Moralis.transfer(options);
```
{% endtab %}

{% tab title="React" %}
```javascript
import React from "react";
import { useWeb3Transfer } from "react-moralis";

const TransferWeth = () => {
  const { fetch, error, isFetching } = useWeb3Transfer({
    amount: Moralis.Units.Token(20, 18),
    receiver: "0x0000000000000000000000000000000000000000",
    type: "erc20",
    contractAddress: "0xc02aaa39b223fe8d0a0e5c4f27ead9083c756cc2",
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

{% hint style="info" %}
By using [`Moralis.Units.Token`](../tools/moralis-units.md#converting-erc20-token-to-wei)helper function you can multiply the amount you want to send by the number of decimals. This is **required** to construct an ERC20 transaction.
{% endhint %}

{% hint style="info" %}
The _**contract address**_ and the _**number of decimals**_ for a token can normally be found on [Etherscan](https://etherscan.io) or on the website of the project.
{% endhint %}

{% hint style="success" %}
Get the address and decimals programmatically of a token by using Moralis SDK  [Moralis.Web3API.token.getTokenMetadataBySymbol()](https://docs.moralis.io/moralis-server/web3-sdk/token#gettokenmetadatabysymbol).&#x20;
{% endhint %}

### Resolving the results

`Moralis.transfer()` returns a [transaction response](https://docs.ethers.io/v5/api/providers/types/#providers-TransactionResponse) after it is executed. The below page shows how to consume the data returned.

{% content-ref url="resolve-transfer.md" %}
[resolve-transfer.md](resolve-transfer.md)
{% endcontent-ref %}
