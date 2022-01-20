---
description: >-
  Authenticate with Ethereum, Binance Smart Chain, Polygon, Arbitrum, Elrond,
  and Many Others to Come!
---

# Crypto Login

## Moralis Authentication

Moralis allows you to authenticate users on any blockchain with just one line of code. All assets, tokens, and NFTs of your users are automatically synced into your Moralis database and are updated in real-time as your users make on-chain transactions.

## Ethereum, BSC, and Polygon Login

### MetaMask

Authenticating users is simple:

```javascript
Moralis.authenticate().then(function (user) {
    console.log(user.get('ethAddress'))
})
```

This will connect [MetaMask](https://metamask.io) and request a signature (no gas required!).

We use the signature as proof the user is owner of account, if no signature is provided, anyone can gain the credentials necessary to read /write to users private data in Moralis Database. The signing is no different than entering a username and password. If a user wants to use the authenticated features of an app they need to “log in”. They choose when to do so by pressing the “login” button.

![](<../../.gitbook/assets/Metamask\_Login (1).png>)

It works the same way for all Ethereum Virtual Machine (EVM) compatible chains such as Binance Smart Chain and Polygon (Matic), as they all share the same Ethereum addresses.

As soon as the user is logged in all their assets, tokens, NFTs and past transactions are instantly synced into your Moralis Database. The database updates if the users are moving assets on-chain.

![Once the user logs in - all their assets are seen in the database. The database updates if the users move assets on chain.](<../../.gitbook/assets/image (117) (1) (1).png>)

{% embed url="https://youtu.be/SYWdSg9KLCQ" %}
Logging in your first users on Ethereum, Polygon, Avalanche (or any other chain) using Moralis.
{% endembed %}

### WalletConnect

{% embed url="https://www.youtube.com/watch?v=UP6MfkU3Bkg" %}
Practical Demo how to login your users to you dapp with any wallet.
{% endembed %}

Moralis also supports authentication using WalletConnect. First add the provider by adding the script (make sure to use the latest version, see [https://github.com/WalletConnect/walletconnect-monorepo/releases](https://github.com/WalletConnect/walletconnect-monorepo/releases)):

*When you imported moralis via CDN:*
```javascript
<script src="https://github.com/WalletConnect/walletconnect-monorepo/releases/download/1.7.1/web3-provider.min.js"></script>
```
> Make sure to check if you use the latest stable version of the WalletConnect web3-provider, and update the version accordingly. Check for their latest release the [releases on Github](https://github.com/WalletConnect/walletconnect-monorepo/releases/)

*When you imported moralis via NPM or another package manager:*

```bash
npm install @walletconnect/web3-provider
```

Then call authenticate like above, but with a provider option:

```javascript
const user = await Moralis.authenticate({ provider: "walletconnect" })
```

#### Specify the `chainId`.

You might want to specify the chain id that WalletConnect will use by default. You can do this by providing the `chainId` as an extra option:&#x20;

```javascript
const user = await Moralis.authenticate({ provider: "walletconnect", chainId: 56 })
```

#### Filter Mobile Linking Options&#x20;

If you would like to reduce the number of mobile linking options or customize its order, you can provide an array of wallet names to the `mobileLinks` option .&#x20;

```javascript
const user = await Moralis.authenticate({ 
    provider: "walletconnect", 
    mobileLinks: [
      "rainbow",
      "metamask",
      "argent",
      "trust",
      "imtoken",
      "pillar",
    ] 
})
```

### Custom Wallet Login
Although Moralis offers native support for MetaMask and WalletConnect, it's possible to use any Web3 provider. The scope of this guide is to demonstrate how to supply any provider.

This guide will use `Tourus` and Binance Smart Chain. The `Tourus` documentation is available at this url: [https://docs.tor.us/](https://docs.tor.us).

Moralis connects to a provider via a `connector`, to implement your own connector you can extend the `AbstractConnector` and implement the functions.

**Import the Provider**

```javascript
<script src="https://cdn.jsdelivr.net/npm/@toruslabs/torus-embed"></script>
```


**Extending the Moralis AbstractWeb3Connector class**
```javascript
class TorusConnector extends Moralis.AbstractWeb3Connector {
  // A name for the connector to reference it easy later on
  type = "Torus";

  /**
   * A function to connect to the provider
   * This function should return an EIP1193 provider (which is the case with most wallets)
   * It should also return the account and chainId, if possible
   */
  async activate() {
    this.torus = new Torus();

    await this.torus.init({
      enableLogging: true,
      network: {
        host: "https://speedy-nodes-nyc.moralis.io/7ac501390ca7c7a0b65f90e7/bsc/testnet",
        networkName: "Smart Chain - Testnet",
        chainId: 97,
        blockExplorer: "https://testnet.bscscan.com",
        ticker: "BNB",
        tickerName: "BNB",
      },
    });

    // Store the EIP-1193 provider, account and chainId
    const accounts = await this.torus.login();
    this.account = accounts[0]
    this.chainId = "0x61" // Should be in hex format
    this.provider = this.torus.provider;

    // Call the subscribeToEvents from AbstractWeb3Connector to handle events like accountsChange and chainChange
    this.subscribeToEvents(this.provider);

    // Return the provider, account and chainId
    return {
      provider: this.provider,
      chainId: this.chainId,
      account: this.account,
    };
  }

  // Cleanup any references to torus
  async deactivate() {
    // Call the unsubscribeToEvents from AbstractWeb3Connector to handle events like accountsChange and chainChange
    this.unsubscribeToEvents(this.provider);

    if (this.torus) {
      await this.torus.cleanUp();
    }

    this.account = null;
    this.chainId = null;
    this.torus = null;
    this.provider = null
  }
}
```

**Call authenticate/enableWeb3 with the connector**

You are good to go, you can now enable Moralis and connect to Web3 by:

```javascript
window.web3 = await Moralis.enableWeb3({
    connector: TorusConnector
});
```

![](../../.gitbook/assets/custom.png)



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

After the signature is completed the promise will resolve with a `User` object. The shape of the created user looks like this:&#x20;

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

It will also create a new entry in the `Moralis._EthAddress` class, corresponding to the Ethereum address used for this user. The schema of the created entry in the `Moralis._EthAddress` object looks like this:&#x20;

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
