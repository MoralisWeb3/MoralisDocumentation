# Resolve

## resolveDomain

Resolves an Unstoppable domain and returns the address (asynchronous).

#### Options:

* `currency`(optional): The currency to query. Available values : `eth`, `0x1`. Default value : `eth`.
*  `domain` (required): Domain to be resolved.

```javascript
// get polygon NFTs for address
const options = { currency: 'eth', domain: 'brad.crypto' };
const resolve = await Moralis.Web3API.resolve.resolveDomain(options );
```

#### Example result:

```javascript
{
  "address": "0x057Ec652A4F150f7FF94f089A38008f49a0DF88e"
}
```

## resolveAddress

Reverse resolves an address and returns the ENS domain (if exists).

#### Options:

*  `address` (required): Ethereum address to be reverse resolved.

```javascript
// get ENS domain of an address
const options = { address: '0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045' };
const resolve = await Moralis.Web3API.resolve.resolveAddress(options);
```

#### Example result:

```javascript
{
  "name": "vitalik.eth"
}
```

## Demo: Resolving ENS and Unstoppable Domains

{% embed url="https://www.youtube.com/watch?v=CvtEciDhcuY" %}
Practical example showing how ENS Domain resolution works in Ethereum. Also demonstrating Unstoppable Domains.
{% endembed %}
