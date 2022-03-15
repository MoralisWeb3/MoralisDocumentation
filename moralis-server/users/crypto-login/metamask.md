---
description: Moralis Metamask Authentication
---

# 🦊 Metamask

Authenticating users using MetaMask is simple:

```javascript
Moralis.authenticate().then(function (user) {
    console.log(user.get('ethAddress'))
})
```

This will connect [MetaMask](https://metamask.io) and request a signature (no gas required!).

We use the signature as proof the user is owner of account, if no signature is provided, anyone can gain the credentials necessary to read /write to users private data in Moralis Database. The signing is no different than entering a username and password. If a user wants to use the authenticated features of an app they need to “log in”. They choose when to do so by pressing the “login” button.

![](<../../../.gitbook/assets/Metamask\_Login (1).png>)

It works the same way for all Ethereum Virtual Machine (EVM) compatible chains such as Binance Smart Chain and Polygon (Matic), as they all share the same Ethereum addresses.

As soon as the user is logged in all their assets, tokens, NFTs and past transactions are instantly synced into your Moralis Database. The database updates if the users are moving assets on-chain.

![Once the user logs in - all their assets are seen in the database. The database updates if the users move assets on chain.](<../../../.gitbook/assets/image (117) (1) (1).png>)

### Change MetaMask App Icon

It's possible to change the icon a user sees when interacting with your smart contract. To accomplish this, you'll have to add a favicon to your dApp. Follow the instructions in the [MetaMask docs](https://docs.metamask.io/guide/defining-your-icon.html).

### Tutorial

{% embed url="https://youtu.be/SYWdSg9KLCQ" %}
Logging in your first users on Ethereum, Polygon, Avalanche (or any other chain) using Moralis.
{% endembed %}
