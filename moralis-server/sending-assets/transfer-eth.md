---
description: >-
  Transfer Native assets on any blockchain - ETH (Ethereum), BNB (Binance Smart
  Chain), MATIC  (Polygon)
---

# Transfer ETH

### Transfer Native Assets (ETH/BNB/MATIC etc)

To transfer native assets of the blockchain  follow the steps:&#x20;

1. Construct an`options`object and set
   1. `type:"native"`&#x20;
   2. `amount: "3425435345" //in wei`
   3. `receiver: "0x000..." //wallet address`
2. Call the Moralis transfer function as shown below

{% tabs %}
{% tab title="JS" %}
```javascript
// sending 0.5 ETH
const options = {
  type: "native",
  amount: Moralis.Units.ETH("0.5"),
  receiver: "0x.."
};
let result = await Moralis.transfer(options);
```
{% endtab %}

{% tab title="React" %}
```javascript
import React from "react";
import { useWeb3Transfer } from "react-moralis";

const TransferEth = () => {
  const { fetch, error, isFetching } = useWeb3Transfer({
    type: "native",
    amount: Moralis.Units.ETH(0.5),
    receiver: "0x0000000000000000000000000000000000000000"
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

Use `Moralis.Units.ETH` to specify the amount in ETH (same goes for BSC and Polygon).&#x20;

{% hint style="info" %}
**``**[**`Moralis.Units.ETH`**](../tools/moralis-units.md#converting-native-asset-eth-bnb-matic-etc-to-wei)is a helper function that will convert your ETH amount to _wei_ which is required in order to construct the transaction.
{% endhint %}
