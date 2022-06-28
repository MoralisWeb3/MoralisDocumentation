---
description: Moralis is natively integrated with WalletConnect.
---

# 📲 WalletConnect

## Integrate Moralis with WalletConnect

WalletConnect lets you connect via QR code, mobile wallets and desktop wallets. You can check out more in the [WalletConnect Documentation](https://docs.walletconnect.com).

### 1. Add the WalletConnect provider

Add the provider script based on how moralis was imported into the project - CDN, npm, or yarn.

{% tabs %}
{% tab title="CDN" %}
```html
<script src="https://github.com/WalletConnect/walletconnect-monorepo/releases/download/1.7.1/web3-provider.min.js"></script>
```
{% endtab %}

{% tab title="npm" %}
```bash
npm install @walletconnect/web3-provider
```
{% endtab %}

{% tab title="yarn" %}
```
yarn add @walletconnect/web3-provider
```
{% endtab %}
{% endtabs %}

{% hint style="warning" %}
Make sure to check if you use the latest stable version of the WalletConnect web3-provider, and update the version accordingly. Check for their latest releases here [releases on Github](https://github.com/WalletConnect/walletconnect-monorepo/releases/)
{% endhint %}

### 2. Call the authenticate function

Call authenticate function, but with a **provider** option:

{% tabs %}
{% tab title="JS" %}
```javascript
const user = await Moralis.authenticate({ provider: "walletconnect" })
```
{% endtab %}

{% tab title="React" %}
```javascript
import { useMoralis } from "react-moralis";

function App() {

    const { authenticate, isAuthenticated, user } = useMoralis();

    const login = async () => {
      if (!isAuthenticated) {

        await authenticate({ provider: "walletconnect" })
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

### 3. Specify the `chainId`

Specify the chain id that WalletConnect will use by default. You can do this by providing the **`chainId`** as an extra option:

{% tabs %}
{% tab title="JS" %}
```javascript
const user = await Moralis.authenticate({ provider: "walletconnect", chainId: 56 })
```
{% endtab %}
<<<<<<< HEAD
=======

>>>>>>> d82bb09ff1610e9b0e24a7f8db9118fc66e13d2b
{% tab title="React" %}
```javascript
import { useMoralis } from "react-moralis";

function App() {

    const { authenticate, isAuthenticated, user } = useMoralis();

    const login = async () => {
      if (!isAuthenticated) {

        await authenticate({ provider: "walletconnect", chainId: 56 })
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
<<<<<<< HEAD

=======
>>>>>>> d82bb09ff1610e9b0e24a7f8db9118fc66e13d2b

### 4. Filter Mobile Linking Options

To reduce the number of mobile linking options or customize its order, provide an array of [wallet names](https://walletconnect.com/registry?type=wallet) to the **`mobileLinks`** option.

{% tabs %}
{% tab title="JS" %}
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
{% endtab %}
<<<<<<< HEAD
=======

{% tab title="React" %}
```javascript
import { useMoralis } from "react-moralis";

function App() {

    const { authenticate, isAuthenticated, user } = useMoralis();

    const login = async () => {
      if (!isAuthenticated) {

        await authenticate({ 
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
>>>>>>> d82bb09ff1610e9b0e24a7f8db9118fc66e13d2b

{% tab title="React" %}
```javascript
import { useMoralis } from "react-moralis";

function App() {

    const { authenticate, isAuthenticated, user } = useMoralis();

    const login = async () => {
      if (!isAuthenticated) {

        await authenticate({ 
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
## Tutorial

{% embed url="https://www.youtube.com/watch?feature=emb_title&v=UP6MfkU3Bkg" %}
Moralis WalletConnect Demo
{% endembed %}
