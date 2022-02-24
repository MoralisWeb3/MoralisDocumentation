# Account

## balance

Returns SOL balance of an address.

#### Options:

- `network`: The blockchain to get data from. Valid values are listed on the [intro page in the Transactions and Balances section](https://docs.moralis.io/transactions-and-balances/intro). Default value `Eth`.
- `address`: A user address (i.e. `HsXZnAba2...`). If specified, the user attached to the query is ignored and the address will be used instead.

```javascript
// get mainnet transactions for the current user
const balance = await Moralis.SolanaAPI.account.balance();

// get BSC transactions for a given address
// with most recent transactions appearing first
const options = {
  network: "mainnet",
  address: "HsXZnAba2...",
};
const balance = await Moralis.SolanaAPI.account.balance(options);
```

#### Example result:

```javascript
{
  "lamports": "500000000",
  "solana": "0.5"
}
```
