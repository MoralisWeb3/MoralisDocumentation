# Transfer ETH

## Transferring Native Asset (ETH/BNB/MATIC etc)

In order to transfer native assets of the blockchain (ETH on Ethereum, BNB on Binance Smart Chain, etc) you have to construct an`options`object where you set`type:native`to tell Moralis that you are sending the native asset (and not an ERC20 token or NFT).

Next, you have to tell Moralis the amount you want to transfer, to do that you need to use `Moralis.Units.ETH` (even when sending on BSC or Polygon) and specify the amount as a string denominated in ETH. `Moralis.Units.ETH`is a helper function that will convert your sting to wei which is required in order to construct the transaction.

Finally, you have to specify the`receiver`of the funds. You do so by specifying the address as a string as demonstrated below.

{% tabs %}
{% tab title="JS" %}

```javascript
// sending 0.5 ETH
const options = {
  type: "native",
  amount: Moralis.Units.ETH("0.5"),
  receiver: "0x..",
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
    amount: Moralis.Units.ETH(0.5),
    receiver: "0x0000000000000000000000000000000000000000",
    type: "native",
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
