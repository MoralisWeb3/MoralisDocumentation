# Transfer Tokens

## Transferring ERC20 Tokens

When sending an ERC20 you need to know the contract address of the token and the number of decimals.&#x20;

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

The contract address and the number of decimals for a token can normally be found on Etherscan or on the website of the project.

By using `Moralis.Units.Token`helper function you can multiply the amount you want to send by the number of decimals. This is required to construct an ERC20 transaction.

If you need to programmatically get the address and decimals of a token you can use Moralis SDK [Moralis.Web3API.token.getTokenMetadataBySymbol()](https://docs.moralis.io/moralis-server/web3-sdk/token#gettokenmetadatabysymbol).

