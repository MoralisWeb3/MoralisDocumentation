---
description: >-
  Sync user token balances, NFTs, historic transactions, native balances and so
  much more! Moralis tracks everything in real time and populates your database
  accordingly. This is true web3 magic! 🤩
---

# User Balances and Transactions

## Sync and Watch Address

### Defining

You can get all historical transactions and listen to new transactions in real-time. It requires the following information:

* `ChainId `(required): The chain to sync
* `Address `(required): The address you will listen to for this event.
* `Sync_historical` (optional): Sync Historical Data option. Default value `true`

![](<../../.gitbook/assets/image (115).png>)

![](<../../.gitbook/assets/image (116).png>)

### Historical Event Limit

If a sync job is created that would result in retrieving 500k or more historical events, then the `"Sync_historical (Optional)`option will be disabled and no historical data will be saved. It is possible to contact support to upgrade your account to enable saving it anyway but think hard about whether it's actually necessary before doing so. It's possible to handle the events in real-time without saving the data to the database. 

## Monitoring Authenticated Users

Moralis Server syncs all transactions and balances for users that at some point authenticated with your app in real-time. You don't have to do anything to enable this feature as it's enabled by default in all Moralis Servers.

As always we want you to focus on the user experience and leave this mundane and complex task of syncing blockchain data to us [🧙](https://emojipedia.org/mage/). 

Moralis Server will in real-time insert and update data into your database so that you can get all the latest transactions and balances of your users with a simple database query. 

{% embed url="https://www.youtube.com/watch?v=Z8Ik3TyubvU" %}
In this video we demonstrate this functionality.
{% endembed %}

## Monitoring Non-Authenticated Address

By default, only the transaction data for registered users will appear in the `EthTransactions` collection. To watch a specific address- like a centralized exchange hot wallet- add a new `Sync and Watch Address` plugin, enter the `Address` and then click the "Add Plugin" button.

This starts a sync job and adds an entry to the `WatchedXxxAddress` collection, where "Xxx" is one of the chain names defined [here](historical-transactions.md#chain-prefixes). Once the transactions are synced, they can be queried like any other transactions, including [Live Queries](../database/live-queries.md).

```javascript
const web3 = new Moralis.Web3();
const binanceWallet = "0x...";

// create query
const query = new Moralis.Query("EthTransactions");
query.equalTo("to_address", binanceWallet);

// subscribe for real-time updates
const subscription = await query.subscribe();
subscription.on("create", function(data) {
  const amountEth = web3.utils.fromWei(data.attributes.value);
  console.log(`${amountEth} deposited to Binance`);
});
```

Keep in mind that watching an address with a lot of transaction volume will result in a lot of data being stored if the `Sync_historical` option is enabled.

See the [Cloud Functions](../cloud-code/cloud-functions.md) and [Live Query](../database/live-queries.md) sections for more details on these topics.

### Historical Transaction Limit

If an address is watched that would result in retrieving 250k or more historical transactions, then the `Sync_historical` option will be disabled and no historical data will be saved. It is possible to contact support to upgrade your account to enable saving it anyway but think hard about whether it's actually necessary before doing so. It's possible to handle new transactions in real-time without saving the data to the database. You can also call the [Deep Index API](broken-reference) to selectively query the historical data. See below for more details.

### Watch Address From Code

The `Sync and Watch Address` plugin calls a [Cloud Function](../cloud-code/cloud-functions.md) called `watchXxxAddress`under the hood, where "Xxx" are the chain names [here](historical-transactions.md#chain-prefixes). These cloud functions can also be called directly from your own code!

```javascript
const results = await Moralis.Cloud.run("watchBscAddress", {address: "0x..."})
```

{% hint style="info" %}
The watch address functions return no value as they start a job. They are still asynchronous though! Once the promise returns the synced transactions, they should be in the XxxTransactions table for the corresponding chain.
{% endhint %}

## Unconfirmed Transactions

Transactions on Testnet and Mainnet can take a while to be confirmed. When Moralis detects a new transaction (or event) in an unconfirmed state, these get put into transaction tables like `EthTransactions` with `confirmed: false`. For aggregate collections like balances, the unconfirmed transaction entries are put in separate collections postfixed with "Pending".

* EthBalancePending
* EthNFTOwnersPending
* etc.

When the transaction gets confirmed, the status is updated to `confirmed: true` and any corresponding entries in pending collections are merged into their respective main collections.

### Consequences for Triggers

This means if you define an `afterSave` trigger on a collection with a `confirmed` property like `EthTransactions` or any "Sync and Watch Contract Event" collection, then the trigger can get fired TWICE- once for `confirmed: false` and again for `confirmed: true`! See the [Trigger](../cloud-code/triggers.md) section for more details.

## Collection Schema

Moralis Server will get all value transfers, token transfers (ERC20), and NFT transfers (ERC721, ERC1155) made from or to any authenticated user address (including linked addresses) or watched address. Right after a user is [created](https://docs.moralis.io/users/intro) (or an address is watched), Moralis Server will populate the following set of collections for each blockchain synced by the Moralis Server. The names will be prefixed by the chain they came from.

* xxxTransactions
* xxxTokenTransfers
* xxxTokenBalances
* xxxNFTTransfers
* xxxNFTOwners

These collections can be viewed in the "Moralis Dashboard."

### Chain Prefixes

| Chain                                                    | Prefix  |
| -------------------------------------------------------- | ------- |
| Ethereum Mainnet, Ropsten, Georli, Kovan, Local Devchain | `Eth`   |
| Binance Smart Chain Mainnet, Testnet                     | `Bsc`   |
| Polygon (Matic) Mainnet, Mumbai Testnet                  | `Matic` |
| Elrond                                                   | `Erd`   |

### Transactions

All transactions to or from a user or watched addresses on that chain does not include internal transfers within smart contracts, like token transfers.

```javascript
{
    "objectId": String,
    "block_hash": String,
    "gas_price": Number,
    "block_timestamp": Date,
    "receipt_cumulative_gas_used": Number,
    "ACL": ACL,
    "receipt_gas_used": Number,
    "input": String,
    "receipt_contract_address": String,
    "hash": String,
    "updatedAt": Date,
    "nonce": Number,
    "to_address": String,
    "transaction_index": Number,
    "value": String,
    "gas": Number,
    "receipt_status": Number,
    "createdAt": Date,
    "block_number": Number,
    "from_address": String,
    "confirmed": Boolean,
}
```

### TokenTransfers

All ERC20 token transfer events for user addresses and watched addresses will be found here.

```javascript
{
    "objectId": String,
    "block_hash": String,
    "block_timestamp": Date,
    "ACL": ACL,
    "updatedAt": Date,
    "token_address": String,
    "transaction_hash": String,
    "to_address": String,
    "transaction_index": Number,
    "value": String,
    "log_index": Number,
    "createdAt": Date,
    "block_number": Number,
    "from_address": String,
    "confirmed": Boolean,
}
```

### TokenBalances

A summary of a user and watched address token balances. This will be updated in real-time as new transactions are made.

```javascript
{
  "objectId": String,
  "decimals": String,
  "contract_type": String,
  "ACL": ACL,
  "name": String,
  "updatedAt": Date,
  "token_address": String,
  "address": String,
  "symbol": String,
  "createdAt": Date,
  "block_number": Number,
  "balance": String,
}
```

### NFTTransfers

Transfer events for NFTs (ERC721 and ERC1155 tokens) of user and watched addresses.

```javascript
{
  "objectId": String,
  "block_hash": String,
  "token_id": String,
  "block_timestamp": Date,
  "contract_type": String,
  "ACL": ACL,
  "updatedAt": Date,
  "token_address": String,
  "transaction_hash": String,
  "to_address": String,
  "transaction_index": Number,
  "log_index": Number,
  "amount": String,
  "createdAt": Date,
  "block_number": Number,
  "transaction_type": String,
  "from_address": String,
  "confirmed": Boolean,
}
```

### NFTOwners

A summary of NFT balances (ERC721 and ERC1155 tokens) for a user and watched addresses.

```javascript
{
  "objectId": String,
  "token_id": String,
  "owner_of": String,
  "token_uri": String,
  "contract_type": String,
  "ACL": ACL,
  "name": String,
  "updatedAt": Date,
  "token_address": String,
  "amount": Sring,
  "symbol": String,
  "createdAt": Date,
  "block_number": Number,
}
```
