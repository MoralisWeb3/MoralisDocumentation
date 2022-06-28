---
description: Moralis is now fully integrated with Web3Auth.
---

# 🔑 Web3Auth.io

## Integrating Moralis and Web3Auth

Moralis supports authentication using [Web3Auth](https://web3auth.io). This allows for user onboarding through both social logins, and web3 wallets.

### 1. Create a Web3Auth account

To get started, make an account [here](https://dashboard.web3auth.io) and get the publishable clientId.

```javascript
clientId: 'ABC*****************'
```

### 2. Add the Web3Auth SDK

Import the SDK based on how moralis was imported into the project - CDN, npm, or yarn.

{% tabs %}
{% tab title="CDN" %}
```html
<script src="https://unpkg.com/@web3auth/web3auth@latest/dist/web3auth.umd.min.js"></script>
```
{% endtab %}

{% tab title="npm" %}
```bash
npm install --save @web3auth/web3auth
```
{% endtab %}

{% tab title="yarn" %}
```
yarn add @web3auth/web3auth
```
{% endtab %}
{% endtabs %}

### 3. Call the authenticate function
{% tabs %}
{% tab title="JS" %}
```javascript
const user = await Moralis.authenticate({
	provider: "web3Auth",
	clientId: "ABC*****************",
})
```
{% endtab %}
{% tab title="React" %}
```javascript
import { useMoralis } from "react-moralis";

function App() {
  const { authenticate, isAuthenticated } = useMoralis();

  const login = async () => {
    if (!isAuthenticated) {
      await authenticate({
        provider: "web3Auth",
        clientId: "ABC*****************",
      })
        .then(function (user) {
          console.log(user!.get("ethAddress"));
        })
        .catch(function (error) {
          console.log(error);
        });
    }
  };
}
```
{% endtab %}
{% endtabs %}
Then call authenticate like above, but with a provider option, and the required params. The **`clientId`** is the only **required param**.

## Parameters

Parameters that can be passed into **`Moralis.authenticate()`** when using the **web3Auth** provider

| Parameters          |                                                                                                                                                                  Values                                                                                                                                                                  |
| ------------------- | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------: |
| `clientId`          |                                                                                                                               The publishable clientId from the web3Auth [dashboard](https://web3auth.io).                                                                                                                               |
| `chainId`           |                                                                                                                   `(optional)`The `chainId` of the supported network to connect to. _By default Ethereum mainnet `0x1`_                                                                                                                  |
| `appLogo`           |                                                                                                                       `(optional)`URL of the logo is to be shown at the top of the modal. _By default Moralis Logo_                                                                                                                      |
| `loginMethodsOrder` | <p><code>(optional)</code> An array of strings, which contains the social logins that you want to allow, and the order in which they show up.<br><br>Default: <code>["google", "facebook", "twitter", "reddit", "discord", "twitch", "apple", "line", "github", "kakao", "linkedin", "weibo", "wechat", "email_passwordless"]</code></p> |
| `theme`             |                                                                                                                       `(optional)`The theme of the login modal. Can be one of `light` or `dark`. _By default dark_                                                                                                                       |

## Tutorial

{% embed url="https://www.youtube.com/watch?v=44ItBuw86AA" %}
Moralis and Web3Auth Integration
{% endembed %}
