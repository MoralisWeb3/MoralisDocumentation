---
description: Moralis is natively integrated with Metamask.
---

# 🦊 Metamask

## Integrate Moralis with MetaMask&#x20;

The default authentication in Moralis is the MetaMask wallet authentication. It is the most popular wallet to connect Dapps built on any EVM chain.&#x20;

### 1. Call the authenticate function

Authenticating users using MetaMask is simple:

{% tabs %}
{% tab title="JS" %}
```javascript
Moralis.authenticate().then(function (user) {
    console.log(user.get('ethAddress'))
})
```
{% endtab %}

{% tab title="React" %}
```javascript
import { useMoralis } from "react-moralis";

function App() {

    const { authenticate, isAuthenticated, user } = useMoralis();

    const login = async () => {
      if (!isAuthenticated) {

        await authenticate()
          .then(function (user) {
            console.log(user!.get("ethAddress"));
          })
          .catch(function (error) {
            console.log(error);
          });
      }
    }
}
```
{% endtab %}
{% endtabs %}
This will connect :fox: [MetaMask](https://metamask.io) and request a signature **(no** [**gas**](https://ethereum.org/en/developers/docs/gas/) **required!)**.

{% hint style="info" %}
We use the signature as proof that the user is the account owner
{% endhint %}

The signing is no different than entering a username and password. If a user wants to use the authenticated features of an app they need to “log in”.&#x20;

![MetMask login authentication](<../../../.gitbook/assets/MetaMask Authentication 2.gif>)

{% hint style="info" %}
It works the same way for all Ethereum Virtual Machine (EVM) compatible chains such as Binance Smart Chain and Polygon (Matic), as they all share the same Ethereum addresses.
{% endhint %}

As soon as the user is logged in all their **on-chain data** is instantly synced into your [Moralis Database](../../database/). The database updates if the users are moving assets on-chain.

![Once the user logs in - all their assets are seen in the database. The database updates if the users move assets on chain.](<../../../.gitbook/assets/Screenshot 2022-03-15 at 1.29.16 PM.png>)

### 2. Change MetaMask App Icon

It's possible to change the icon a user sees when interacting with your smart contract. To accomplish this, you'll have to add a favicon to your dApp. Follow the instructions in the [MetaMask docs](https://docs.metamask.io/guide/defining-your-icon.html).

### 3. Add custom Sign-in Message

To change the authentication message on MetaMask. Just follow:&#x20;

{% content-ref url="sign-in-message.md" %}
[sign-in-message.md](sign-in-message.md)
{% endcontent-ref %}

### 4. Example code

The following code demonstrates a working example

{% content-ref url="../../connect-the-sdk/connect-with-js.md" %}
[connect-with-js.md](../../connect-the-sdk/connect-with-js.md)
{% endcontent-ref %}

### Tutorial

{% hint style="info" %}
Legacy UI is present in this video, some things might be slightly different
{% endhint %}

{% embed url="https://youtu.be/SYWdSg9KLCQ" %}
Logging in your first users on Ethereum, Polygon, Avalanche (or any other chain) using Moralis.
{% endembed %}
