---
description: >-
  Authenticate with Ethereum, Binance Smart Chain, Polygon, Arbitrum, Elrond,
  and Many Others to Come!
---

# Web3 Authentication

## Moralis Authentication

Moralis allows you to authenticate users on any blockchain with just one line of code.&#x20;

**All assets, tokens, and NFTs of your users are automatically synced into your Moralis database and are updated in real-time as your users make on-chain transactions.**

## Easy Web3 Login Flow

Using `Moralis.authenticate()` you can login users on any chain using any wallet. Moralis supports WalletConnect, Web3Auth, Magic and other custom authentication methods.

### MetaMask

The easiest way to authenticate web3 users is via Metamask wallet. If you are new to Moralis you should start learning [how MetaMask authentication works](crypto-login.md#metamask).

### WalletConnect

{% embed url="https://www.youtube.com/watch?v=UP6MfkU3Bkg" %}
Practical Demo how to login your users to you dapp with any wallet.
{% endembed %}

Moralis is natively integrated with [WalletConnect](https://walletconnect.com). WalletConnect allows you to easily login to any dapp using your mobile wallet.

Learn how to do it by [reading this guide](crypto-login.md#walletconnect).

### Magic

![Login web3 users with their email or other social login.](<../../.gitbook/assets/Screenshot 2022-02-03 at 21.40.14.png>)

Moralis is fully integrated with [Magic](https://magic.link) which allows you to authenticate your users with their email or other types of social login such as Google or Twitter.

Learn how the integration works [here](crypto-login/magic.md).

## Non-EVM Chain Login

### Solana

You can login a user using Phantom wallet on Solana using the code below.

```javascript
Moralis.authenticate({type:'sol'}).then(function(user) {
    console.log(user.get('solAddress'))
})
```

### Elrond

**Prerequisites**: In order to log in with Elrond, you'll need a ledger that can be purchased [here](https://www.ledger.com). Once you have a ledger, follow the steps in the [Elrond Ledger documentation](https://docs.elrond.com/wallet/ledger/) to add the Elrond app to the device.

Once the above steps are completed, we can continue with the actual authentication process.

If using the CDN, you must add the "[erdjs](https://docs.elrond.com)" script to your `<head>`.

```javascript
<script src="https://npmcdn.com/@elrondnetwork/erdjs@4.0.3/out-browser/erdjs.js"></script>
```

Now that all dependencies are in place we can authenticate:

```javascript
Moralis.authenticate({ type: 'erd' }).then(function (user) {
    console.log(user.get('erdAddress'))
})
```

**Important**: Notice the prefix of the address has changed from `eth` to `erd`.

{% embed url="https://youtu.be/0dLNIbx4GbY" %}

## User Object

After the signature is completed the promise will resolve with a `User` object. The shape of the created user looks like this:

```javascript
{
  className: "_User"
  id: String
  _objCount: 0
  attributes: {
    accounts: Array,
    ACL: Object,
    authData: Object,
    createdAt: Date,
    email: String, // empty
    ethAddress: String,
    sessionToken: String, 
    updatedAT: Date,
    username: String, // random value
  }
}
```

For new users, the `username` will be a randomly generated alphanumeric string and the `email` property will not exist. This can be set or changed by the app. See the "[Objects](../database/objects.md#saving-objects)" and "[Intro](https://docs.moralis.io/users/intro)" docs.

It will also create a new entry in the `Moralis._EthAddress` class, corresponding to the Ethereum address used for this user. The schema of the created entry in the `Moralis._EthAddress` object looks like this:

```javascript
{
    "objectId": String,
    "transactions_synced": Number,
    "ACL": ACL,
    "last_eth_sync": Date,
    "data": String,
    "user": Pointer <_User>,
    "updatedAt": Date,
    "is_syncing": Boolean,
    "last_eth_sync_completed": Date,
    "signature": String,
    "last_eth_sync_block": Number,
    "transactions_total": Number,
    "createdAt": Date,
    "last_eth_sync_error": String
}
```

### Chain Specific User Properties

If you enable syncing data from chains other than Ethereum, additional user properties will be created. These will be prefixed with the chain. As an example, for Binance Smart Chain, the prefix is `bsc`. There will be an additional set of properties for each chain.

* `User.bscAddress` (String).
* `User.bscAccounts` (Array).

It's the same for chain-specific class tables on the Moralis Server. An additional set of classes will be created with the corresponding chain prefixes.

* BscTransactions
* BscTokenBalance
* BscTokenTransfers
* BscNFTOwners
* BscNFTTransfers

### Chain Prefixes

| Chain                                             | Prefix    |
| ------------------------------------------------- | --------- |
| Ethereum Mainnet, Ropsten, Goerli, Rinkeby, Kovan | `Eth`     |
| Binance Smart Chain Mainnet, Testnet              | `Bsc`     |
| Polygon (Matic) Mainnet, Mumbai Testnet           | `Polygon` |
| Elrond                                            | `Erd`     |

### Manually Deleting Users

During testing, if you manually delete a user, then make sure to delete the corresponding rows in the following collections (where "xxx" is the chain prefix). Not doing so may cause unexpected behavior when authenticating via Web3 from the address that was just deleted.

* Session
* xxxBalance
* \_xxxAddress

## Changing Sign-In Message

The message the user sees when signing in with Web3 can be changed by doing the following in the browser:

```javascript
Moralis.authenticate({signingMessage:"hello"})
```

![](<../../.gitbook/assets/image (57).png>)

## Changing App Icon in MetaMask

It's possible to change the icon a user sees when interacting with your smart contract. To accomplish this, you'll have to add a favicon to your dApp. Follow the instructions in the [MetaMask docs](https://docs.metamask.io/guide/defining-your-icon.html).
