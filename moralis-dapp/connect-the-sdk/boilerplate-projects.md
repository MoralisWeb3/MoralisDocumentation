---
description: >-
  Moralis Boilerplate starter projects to help you get started in different
  frameworks.
---

# 🔥 Boilerplate Projects

## Web3 Vanilla Javascript Starter Project

This simple app logs in user, creates a user profile in Moralis Database and syncs user transactions into Moralis Database.

{% code title="index.html" %}

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Vanilla Boilerplate</title>
    <script src="https://cdn.jsdelivr.net/npm/web3@latest/dist/web3.min.js"></script>
    <script src="https://unpkg.com/moralis/dist/moralis.js"></script>
  </head>

  <body>
    <button id="btn-login">Moralis Metamask Login</button>
    <button id="btn-logout">Logout</button>
    <script type="text/javascript" src="./main.js"></script>
  </body>
</html>
```

{% endcode %}

{% code title="main.js" %}

```javascript
const serverUrl = "https://xxxxx.yourserver.com:2053/server";
const appId = "YOUR_APP_ID";
Moralis.start({ serverUrl, appId });

/** Add from here down */
async function login() {
  let user = Moralis.User.current();
  if (!user) {
    try {
      user = await Moralis.authenticate({ signingMessage: "Hello World!" });
      console.log(user);
      console.log(user.get("ethAddress"));
    } catch (error) {
      console.log(error);
    }
  }
}

async function logOut() {
  await Moralis.User.logOut();
  console.log("logged out");
}

document.getElementById("btn-login").onclick = login;
document.getElementById("btn-logout").onclick = logOut;
```

{% endcode %}

## Web3 React Boilerplate

This React Boilerplate has all the features to start your new dapp such as:

1. Authenticate user via their wallet
2. Full WalletConnect Support
3. Page for User Balances
4. Page for User NFTs
5. Page for User Transactions
6. Page or Wallet
7. Page for Decentralized Exchange (DEX)

{% embed url="https://github.com/ethereum-boilerplate/ethereum-boilerplate" %}
Cross-chain web3 boilerplate.
{% endembed %}

## Web3 Unity Boilerplate

The Unity Boilerplate include C# Moralis SDK and has an example Unity scene allowing you to login your users via their wallets, read their tokens and NFTs, interact with smart contracts and much more.

{% embed url="https://github.com/ethereum-boilerplate/ethereum-unity-boilerplate" %}
Web3 Unity Boilerplate
{% endembed %}

## Web3 React Native Boilerplate (alpha)

{% hint style="danger" %}
This boilerplate is not for production use.
{% endhint %}

This React Native Boilerplate has all the features to start your new iOS or Android web3 app such as:

1. Authenticate user via their wallet
2. Full WalletConnect Support
3. Page for User Balances
4. Page for User NFTs
5. Page for User Transactions
6. Page or Wallet
7. Page for Decentralized Exchange (DEX)

{% embed url="https://github.com/ethereum-boilerplate/ethereum-react-native-boilerplate" %}
Cross-chain mobile web3 boilerplate
{% endembed %}

## NFT Marketplace Boilerplate

A simple NFT Marketplace allowing user to do the following:

1. Search NFTs
2. Buy NFTs
3. Sell NFTs
4. Works cross-chain

{% embed url="https://github.com/ethereum-boilerplate/ethereum-nft-marketplace-boilerplate" %}
NFT Marketplace Boilerplate
{% endembed %}
