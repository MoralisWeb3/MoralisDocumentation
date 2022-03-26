---
description: Authenticate Solana users with Phantom Wallet.
---

# ✨ Phantom (Solana)

If your user has [Phantom Wallet](https://phantom.app) installed you can use the following code in order to log them into your Moralis dapp.

```javascript
Moralis.authenticate({type: "sol"})
```

{% hint style="warning" %}
It's important to mention that Solana integration is still in development and Solana users won't get their transactions synced into the [Moralis database](../../database/).
{% endhint %}

