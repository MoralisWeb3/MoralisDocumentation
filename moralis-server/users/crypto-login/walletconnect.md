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

```javascript
const user = await Moralis.authenticate({ provider: "walletconnect" })
```

### 3. Specify the `chainId`

Specify the chain id that WalletConnect will use by default. You can do this by providing the **`chainId`** as an extra option:

```javascript
const user = await Moralis.authenticate({ provider: "walletconnect", chainId: 56 })
```

### 4. Filter Mobile Linking Options

To reduce the number of mobile linking options or customize its order, provide an array of [wallet names](https://walletconnect.com/registry/wallets) to the **`mobileLinks`** option.

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

## Tutorial

{% embed url="https://www.youtube.com/watch?feature=emb_title&v=UP6MfkU3Bkg" %}
Moralis WalletConnect Demo
{% endembed %}
