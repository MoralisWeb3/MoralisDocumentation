---
description: Moralis is fully integrated with Magic (also known as Magic Link)
---

# 🪄 Magic

## Integrating Moralis and Magic

Moralis supports authentication using [Magic](https://go.magic.link/moralis-web3-magic). That way a user can use a crypto-login by using only an email.

To get started, you will need to make an account on [here](https://go.magic.link/moralis-web3-magic) to get an publishable api-key. This key looks like:

```javascript
pk_xxxxxxx
```

> Note: do not use the secret api-key, that one should never be used on the client-side of your app. This key starts with sk\_xxxxxx

Next, add the sdk by adding the script:

_When you imported moralis via CDN:_

```javascript
<script src="https://auth.magic.link/sdk"></script>
```

_When you imported moralis via NPM or another package manager:_

```bash
npm install magic-sdk
```

Then call authenticate like above, but with a provider option, and the required params. The `email`, `apiKey` and `network` are all required params.

* `email`: the email of the user that wants to login
* `apiKey` the publishable api-key that you can get in your Magic dashboard on http://magic.link
* `network`: one of mainnet, rinkeby, kovan, or ropsten

```javascript
const user = await Moralis.authenticate({ 
  provider: "magicLink",
  email: "example@email.com",
  apiKey: "pk_xxxxx",
  network: "kovan",
})
```
