---
description: Moralis is fully integrated with Magic (also known as Magic Link)
---

# 🪄 Magic

## Integrating Moralis and Magic

Moralis is fully integrated with [Magic](https://magic.link) which allows you to authenticate your users with their email or other types of social login such as Google or Twitter. __ Learn how the integration works [here](magic.md).

![Login web3 users with their email or other social login.](<../../../.gitbook/assets/Screenshot 2022-02-03 at 21.40.14.png>)

### 1. Create a Magic account

To get started, you will need to make an account on [here](https://go.magic.link/moralis-web3-magic) to get a publishable api-key. This key looks like this:

```javascript
pk_xxxxxxx
```

{% hint style="danger" %}
Note: Do not use the **`secret api-key`**, that one should never be used on the client-side of your app. This key starts with sk\_xxxxxx
{% endhint %}

### 2. Add the Magic SDK

Import the SDK based on how moralis was imported into the project - CDN, npm, or yarn.

{% tabs %}
{% tab title="CDN" %}
```html
<script src="https://auth.magic.link/sdk"></script>
```
{% endtab %}

{% tab title="npm" %}
```bash
npm install magic-sdk
```
{% endtab %}

{% tab title="yarn" %}
```
yarn add magic-sdk
```
{% endtab %}
{% endtabs %}

### 3. Call the authenticate function

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

## Tutorial

{% embed url="https://youtu.be/gLJ4YejmG2E" %}
Moralis and Magic Integration
{% endembed %}

