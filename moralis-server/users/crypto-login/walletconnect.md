---
description: Moralis is natively integrated with WalletConnect.
---

# 📲 WalletConnect

## Integrating Moralis with WalletConnect

{% embed url="https://www.youtube.com/watch?feature=emb_title&v=UP6MfkU3Bkg" %}
Moralis WalletConnect Demo
{% endembed %}

Moralis supports authentication using WalletConnect. First add the provider by adding the script (make sure to use the latest version, see [https://github.com/WalletConnect/walletconnect-monorepo/releases](https://github.com/WalletConnect/walletconnect-monorepo/releases)):

_When you imported moralis via CDN:_

```javascript
<script src="https://github.com/WalletConnect/walletconnect-monorepo/releases/download/1.7.1/web3-provider.min.js"></script>
```

> Make sure to check if you use the latest stable version of the WalletConnect web3-provider, and update the version accordingly. Check for their latest release the [releases on Github](https://github.com/WalletConnect/walletconnect-monorepo/releases/)

_When you imported moralis via NPM or another package manager:_

```bash
npm install @walletconnect/web3-provider
```

Then call authenticate like above, but with a provider option:

```javascript
const user = await Moralis.authenticate({ provider: "walletconnect" })
```

#### Specify the `chainId`.

You might want to specify the chain id that WalletConnect will use by default. You can do this by providing the `chainId` as an extra option:

```javascript
const user = await Moralis.authenticate({ provider: "walletconnect", chainId: 56 })
```

#### Filter Mobile Linking Options

If you would like to reduce the number of mobile linking options or customize its order, you can provide an array of wallet names to the `mobileLinks` option .

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
