---
description: Moralis is now fully integrated with Web3Auth.
---

# 🔑 Web3Auth

## Integrating Moralis and Web3Auth

Moralis supports authentication using [Web3Auth](https://web3auth.io). This allows for user onboarding through both social logins, and web3 wallets.

{% embed url="https://www.youtube.com/watch?v=44ItBuw86AA" %}

To get started, make an account [here](https://dashboard.web3auth.io) and get the publishable clinetId.

```javascript
clinetId: 'ABC*****************'
```

Next, add the sdk into your app.

_If you import moralis via a CDN:_

```javascript
<script  src="https://unpkg.com/@web3auth/web3auth@0.2.3/dist/web3auth.umd.min.js"></script>
```

_If you import moralis via a NPM or another package manager:_

```bash
npm install --save @web3auth/web3auth
```

Then call authenticate like above, but with a provider option, and the required params. The `clientId` is the only required param.

* `clinetId`: The publishable clientId that you got from the web3Auth [dashboard](https://web3auth.io).
* `chainId`: (Optional) The chainId of the supported network you are looking to connect to. _By default Ethereum mainet `0x1`_
* `theme`: (Optional) The theme of the login modal. Can be one of `light` or `dark`. _By default dark_
* `appLogo` :(Optional) Url of the logo to be shown at the top of the modal. _By default Moralis Logo_
* `loginMethodsOrder`:(Optional) An array of strings, which containes the social logins that you want to allow, and the order of which they show up.
  * Default: `["google", "facebook", "twitter", "reddit", "discord", "twitch", "apple", "line", "github", "kakao", "linkedin", "weibo", "wechat", "email_passwordless"]`

```javascript
const user = await Moralis.authenticate({
	provider: "web3Auth",
	clientId: "ABC*****************",
})
```
