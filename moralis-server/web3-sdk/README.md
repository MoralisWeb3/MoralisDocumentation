---
description: >-
  Moralis Web3 API is a very fast and powerful blockchain API allowing you to
  query data from any chain. Read more in order to understand how it works.
---

# 🪄 Web3 API

### When to use Web3API?

As a blockchain developer, you have to be able to fetch different information such as **block info**, **transaction info**, **NFT metadata**, **token prices**, **user balances**, **owner list of a particular NFT** and any other blockchain data!

You find all of these powerful functions in the <mark style="color:green;">**`Moralis.Web3API`**</mark> namespace in the Moralis SDK.

{% hint style="info" %}
To access Web3API, make sure to initialise the SDK by following the [**Connect with SDK**](../connect-the-sdk/) guide. **`Moralis.start()`** will automatically load the <mark style="color:green;">**`Moralis.Web3API`**</mark> module.
{% endhint %}

Topics are divided into categories, depending on what is the main parameter for obtaining information:

{% content-ref url="native.md" %}
[native.md](native.md)
{% endcontent-ref %}

{% content-ref url="account.md" %}
[account.md](account.md)
{% endcontent-ref %}

{% content-ref url="token.md" %}
[token.md](token.md)
{% endcontent-ref %}

{% content-ref url="resolve.md" %}
[resolve.md](resolve.md)
{% endcontent-ref %}

{% content-ref url="defi-new.md" %}
[defi-new.md](defi-new.md)
{% endcontent-ref %}

{% content-ref url="ipfs-storage-new.md" %}
[ipfs-storage-new.md](ipfs-storage-new.md)
{% endcontent-ref %}

{% hint style="success" %}
It is possible to fetch data using`REST API`by following the [**REST API**](moralis-web3-api-rest.md) guide. It has similar functionality as the SDK and can be used in the backend.
{% endhint %}

### Web3 API return format

Almost all `Web3 API` function methods have a return format:

```javascript
{
  "total": 1,
  "page": 1,
  "page_size": 1,
  "result": Array(1)
}
```

### Applications built using Web3API

A playlist of all the applications built using Web3API features

{% embed url="https://www.youtube.com/playlist?list=PLFPZ8ai7J-iR4F882O2mBjqydynG9iDZS" %}
Youtube playlist with all the Applications built using the Web3API
{% endembed %}

### Tutorial

Video demonstrating how the latest Web3 API can be accessed through the SDK. Can also be used in the backend through [**REST API**](moralis-web3-api-rest.md)****

{% embed url="https://www.youtube.com/watch?v=lX9A6yQXZ_8&ab_channel=MoralisWeb3" %}
Video demonstrating how the latest Web3 API can be accessed through the SDK.
{% endembed %}
