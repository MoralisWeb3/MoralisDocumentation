# 🤝 Web3API.resolve

## resolveDomain

Resolves an Unstoppable domain and returns the address (asynchronous).

#### Options:

- `currency`(optional): The currency to query. Available values : `eth`, `0x1`. Default value : `eth`.
- `domain` (required): Domain to be resolved.

{% tabs %}
{% tab title="JS" %}

```javascript
// get polygon NFTs for address
const options = { currency: "eth", domain: "brad.crypto" };
const resolve = await Moralis.Web3API.resolve.resolveDomain(options);
```

{% endtab %}

{% tab title="React" %}

```javascript
import React from "react";
import { useMoralisWeb3Api } from "react-moralis";

const Web3Api = useMoralisWeb3Api();

const fetchDomain = async () => {
  // get polygon NFTs for address
  const options = { currency: "eth", domain: "brad.crypto" };
  const resolve = await Web3Api.resolve.resolveDomain(options);
  console.log(resolve);
};
```

{% endtab %}

{% tab title="curl" %}

```bash
curl -X 'GET' \
  'https://deep-index.moralis.io/api/v2/resolve/brad.crypto?currency=eth' \
  -H 'accept: application/json' \
  -H 'X-API-Key: MY-API-KEY'
```

{% endtab %}

{% tab title="Unity" %}

```csharp
using MoralisUnity;
using MoralisUnity.Web3Api.Models;
using UnityEngine;

public class Example
{
    public async void fetchDomain()
    {
        Resolve resolve = await Moralis.Web3Api.Resolve.ResolveDomain(domain: "brad.crypto" currency: "eth");
        Debug.Log(resolve.ToJson());
    }
}
```

{% endtab %}
{% endtabs %}

#### Example result:

```javascript
{
  "address": "0x057Ec652A4F150f7FF94f089A38008f49a0DF88e"
}
```

## resolveAddress

Reverse resolves an address and returns the ENS domain (if exists).

#### Options:

- `address` (required): Ethereum address to be reverse resolved.

{% tabs %}
{% tab title="JS" %}

```javascript
// get ENS domain of an address
const options = { address: "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045" };
const resolve = await Moralis.Web3API.resolve.resolveAddress(options);
```

{% endtab %}

{% tab title="React" %}

```javascript
import React from "react";
import { useMoralisWeb3Api } from "react-moralis";

const Web3Api = useMoralisWeb3Api();

const fetchAddress = async () => {
  // get ENS domain of an address
  const options = { address: "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045" };
  const resolve = await Web3Api.resolve.resolveAddress(options);
  console.log(resolve);
};
```

{% endtab %}

{% tab title="curl" %}

```bash
curl -X 'GET' \
  'https://deep-index.moralis.io/api/v2/resolve/0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045/reverse' \
  -H 'accept: application/json' \
  -H 'X-API-Key: MY-API-KEY'
```

{% endtab %}

{% tab title="Unity" %}

```csharp
using MoralisUnity;
using MoralisUnity.Web3Api.Models;
using UnityEngine;

public class Example
{
    public async void fetchAddress()
    {
        Ens result = await Moralis.Web3Api.Resolve.ResolveAddress(address: "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045");
        Debug.Log(result.ToJson());
    }
}
```

{% endtab %}
{% endtabs %}

#### Example result:

```javascript
{
  "name": "vitalik.eth"
}
```

## Tutorial

{% hint style="info" %}
Legacy UI is present in this video, some things might be slightly different
{% endhint %}

{% embed url="https://www.youtube.com/watch?v=CvtEciDhcuY" %}
Practical example showing how ENS Domain resolution works in Ethereum. Also demonstrating Unstoppable Domains.
{% endembed %}
