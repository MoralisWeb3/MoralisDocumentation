# Account

## balance

Returns SOL balance of an address.

#### Options:

- `network`: The network cluster to get data from. Valid values are listed on the [intro page in Supported Networks section](https://docs.moralis.io/moralis-server/solana-sdk/intro#supported-networks). Default value `mainnet`.
- `address`: A user address (i.e. `HsXZnAba2...`). If specified, the user attached to the query is ignored and the address will be used instead.

{% tabs %}
{% tab title="JS" %}

```javascript
// get mainnet SOL balance for the current user
const solBalance = await Moralis.SolanaAPI.account.balance();

// get devnet SOL balance for a given address
const options = {
  network: "devnet",
  address: "HsXZnAba2...",
};
const solBalance = await Moralis.SolanaAPI.account.balance(options);
```

{% endtab %}
{% tab title="React" %}

```javascript
import { useMoralisSolanaApi, useMoralisSolanaCall } from "react-moralis";

const { account } = useMoralisSolanaApi();

// get mainnet SOL balance for the current user
const { fetch, data, isLoading } = useMoralisSolanaCall(account.balance);

// get devnet SOL balance for a given address
const options = {
  network: "devnet",
  address: "HsXZnAba2...",
};
const { fetch, data, isLoading } = useMoralisSolanaCall(
  account.balance,
  options
);
```

{% endtab %}
{% endtabs %}

#### Example result:

```javascript
{
  "lamports": "500000000",
  "solana": "0.5"
}
```

## getSPL

Returns SPL token balance of an address.

#### Options:

- `network`: The network cluster to get data from. Valid values are listed on the [intro page in Supported Networks section](https://docs.moralis.io/moralis-server/solana-sdk/intro#supported-networks). Default value `mainnet`.
- `address`: A user address (i.e. `HsXZnAba2...`). If specified, the user attached to the query is ignored and the address will be used instead.

{% tabs %}
{% tab title="JS" %}

```javascript
// get mainnet SPL token balance for the current user
const tokenBalance = await Moralis.SolanaAPI.account.getSPL();

// get devnet SPL token balance for a given address
const options = {
  network: "devnet",
  address: "HsXZnAba2...",
};
const tokenBalance = await Moralis.SolanaAPI.account.getSPL(options);
```

{% endtab %}
{% tab title="React" %}

```javascript
import { useMoralisSolanaApi, useMoralisSolanaCall } from "react-moralis";

const { account } = useMoralisSolanaApi();

// get mainnet SPL token balance for the current user
const { fetch, data, isLoading } = useMoralisSolanaCall(account.getSPL);

// get devnet SPL token balance for a given address
const options = {
  network: "devnet",
  address: "HsXZnAba2...",
};
const { fetch, data, isLoading } = useMoralisSolanaCall(
  account.getSPL,
  options
);
```

{% endtab %}
{% endtabs %}

#### Example result:

```javascript
[
  {
    associatedTokenAddress: "Dpmpwm93Amvj4uEFpYhjv8ZzfpgARq6zxKTi6mrj97gW",
    mint: "BXWuzb3jEuGsGUe29xdApu8Z3jVgrFbr3wWdsZmLWYk9",
    amountRaw: "100000000000",
    amount: "100",
    decimals: "9",
  },
];
```

## getNFTs

Returns SPL NFT balance of an address.

#### Options:

- `network`: The network cluster to get data from. Valid values are listed on the [intro page in Supported Networks section](https://docs.moralis.io/moralis-server/solana-sdk/intro#supported-networks). Default value `mainnet`.
- `address`: A user address (i.e. `HsXZnAba2...`). If specified, the user attached to the query is ignored and the address will be used instead.

{% tabs %}
{% tab title="JS" %}

```javascript
// get mainnet SPL NFT balance for the current user
const nftBalance = await Moralis.SolanaAPI.account.getNFTs();

// get devnet SPL NFT balance for a given address
const options = {
  network: "devnet",
  address: "HsXZnAba2...",
};
const nftBalance = await Moralis.SolanaAPI.account.getNFTs(options);
```

{% endtab %}
{% tab title="React" %}

```javascript
import { useMoralisSolanaApi, useMoralisSolanaCall } from "react-moralis";

const { account } = useMoralisSolanaApi();

// get mainnet SPL NFT balance for the current user
const { fetch, data, isLoading } = useMoralisSolanaCall(account.getNFTs);

// get devnet SPL NFT balance for a given address
const options = {
  network: "devnet",
  address: "HsXZnAba2...",
};
const { fetch, data, isLoading } = useMoralisSolanaCall(
  account.getNFTs,
  options
);
```

{% endtab %}
{% endtabs %}

#### Example result:

```javascript
[
  {
    associatedTokenAddress: "Dpmpwm93Amvj4uEFpYhjv8ZzfpgARq6zxKTi6mrj97gW",
    mint: "BXWuzb3jEuGsGUe29xdApu8Z3jVgrFbr3wWdsZmLWYk9",
    amountRaw: "100000000000",
    amount: "100",
    decimals: "9",
  },
];
```

## getPortfolio

Returns the portfolio (SOL balance, SPL token blanace, SPL NFT balance) of an address.

#### Options:

- `network`: The network cluster to get data from. Valid values are listed on the [intro page in Supported Networks section](https://docs.moralis.io/moralis-server/solana-sdk/intro#supported-networks). Default value `mainnet`.
- `address`: A user address (i.e. `HsXZnAba2...`). If specified, the user attached to the query is ignored and the address will be used instead.

{% tabs %}
{% tab title="JS" %}

```javascript
// get mainnet NFT balance for the current user
const portfolio = await Moralis.SolanaAPI.account.getPortfolio();

// get devnet NFT balance for a given address
const options = {
  network: "devnet",
  address: "HsXZnAba2...",
};
const portfolio = await Moralis.SolanaAPI.account.getPortfolio(options);
```

{% endtab %}
{% tab title="React" %}

```javascript
import { useMoralisSolanaApi, useMoralisSolanaCall } from "react-moralis";

const { account } = useMoralisSolanaApi();

// get mainnet SPL NFT balance for the current user
const { fetch, data, isLoading } = useMoralisSolanaCall(account.getPortfolio);

// get devnet SPL NFT balance for a given address
const options = {
  network: "devnet",
  address: "HsXZnAba2...",
};
const { fetch, data, isLoading } = useMoralisSolanaCall(
  account.getPortfolio,
  options
);
```

{% endtab %}
{% endtabs %}

#### Example result:

```javascript
{
  "nativeBalance": {
    "lamports": "6000000000",
    "solana": "6"
  },
  "nfts": [
    {
      "associatedTokenAddress": "6Fk96aAvPhV9gZvGXp9mMw1YwiScPX3pm1RwWn11eGrQ",
      "mint": "BpCcCVU4pK2rcvgduCzScaBhqKEKaMukcejkDhJWRyPv"
    }
  ],
  "tokens": [
    {
      "associatedTokenAddress": "Dpmpwm93Amvj4uEFpYhjv8ZzfpgARq6zxKTi6mrj97gW",
      "mint": "BXWuzb3jEuGsGUe29xdApu8Z3jVgrFbr3wWdsZmLWYk9",
      "amountRaw": "100000000000",
      "amount": "100",
      "decimals": "9"
    }
  ]
}
```