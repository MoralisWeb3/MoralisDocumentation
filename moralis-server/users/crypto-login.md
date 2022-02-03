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

## Changing Sign-In Message

The message the user sees when signing in with Web3 can be changed by doing the following in the browser:

```javascript
Moralis.authenticate({signingMessage:"hello"})
```

![](<../../.gitbook/assets/image (57).png>)

## Changing App Icon in MetaMask

It's possible to change the icon a user sees when interacting with your smart contract. To accomplish this, you'll have to add a favicon to your dApp. Follow the instructions in the [MetaMask docs](https://docs.metamask.io/guide/defining-your-icon.html).

### Manually Deleting Users

During testing, if you manually delete a user, then make sure to delete the corresponding rows in the following collections (where "xxx" is the chain prefix). Not doing so may cause unexpected behavior when authenticating via Web3 from the address that was just deleted.

* Session
* \_xxxAddress

