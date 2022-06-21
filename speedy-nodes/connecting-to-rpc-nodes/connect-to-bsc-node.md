---
description: >-
  Fast and Reliable Full Nodes and Full Archive Nodes for Binance Smart Chain
  Mainnet and Testnet.
---

# Connect to BSC Node

Need a fast and reliable full node, but already have your own backend? Then Moralis Speedy Nodes are for you! Get direct WebSocket and JSON RPC access to Moralis full nodes and archive full nodes. Sign up for a free account at [moralis.io](https://moralis.io) to get started!

## Get Your BSC Node URL

Once you log into your account, go to the "Speedy Nodes" section. Click on the "Endpoints" button for the Binance Smart Chain Network. You will see separate URLs for:

- Mainnet.
- Mainnet Archive.
- Testnet.
- Testnet Archive.

![](<../../.gitbook/assets/image (82).png>)

### JSON RPC

Click on the "HTTP" tab and copy the link for the mainnet (or your desired environment). It will look something like this:

```
https://speedy-nodes-nyc.moralis.io/1a2b3c4d5e6f1a2b3c4d5e6f/bsc/mainnet
```

### WebSockets

Click on the "WS" tab and copy the link for your desired network.

```
wss://speedy-nodes-nyc.moralis.io/1a2b3c4d5e6f1a2b3c4d5e6f/bsc/mainnet/ws
```

## Connect to Your Speedy Node

With your Speedy Node URL in hand, it's time to put it to use!

### Web3 JS

First import the [web3.js ](https://web3js.readthedocs.io)library.

```markup
<script src="https://cdn.jsdelivr.net/npm/web3@latest/dist/web3.min.js"></script>
```

Or via `npm` then import.

```javascript
npm install web3
```

```javascript
const Web3 = require("web3");
```

Now that the library is imported, a provider can be created.

```javascript
const NODE_URL = "YOUR SPEEDY NODE URL HERE";
const provider = new Web3.providers.HttpProvider(NODE_URL);
const web3 = new Web3(provider);
```

That's it! See the [web3.js docs](https://web3js.readthedocs.io) for more details on how to use the `web3` object.

### Ethers JS

First import the [ethers.js](https://docs.ethers.io) library.

```markup
<script src="https://cdn.ethers.io/lib/ethers-5.2.umd.min.js"
        type="application/javascript"></script>
```

Or via `npm` then import in the browser or NodeJS

```
npm install ethers
```

```javascript
// JavaScript, NodeJS
const { ethers } = require("ethers");

// ES6 or typescript
import { ethers } from "ethers";
```

#### JSON RPC

Next, create a provider and if needed a signer to sign transactions.

```javascript
const NODE_URL = "YOUR SPEEDY NODE URL HERE";
const provider = new ethers.providers.JsonRpcProvider(NODE_URL);

// provider is read-only get a signer for on-chain transactions
const signer = provider.getSigner();
```

#### WebSockets

```javascript
const NODE_URL = "YOUR SPEEDY NODE URL HERE";
const provider = new ethers.providers.WebSocketProvider(NODE_URL);

// provider is read-only get a signer for on-chain transactions
const signer = provider.getSigner();
```

And there you have it! See the [ethers.js docs](https://docs.ethers.io) for more details on how to use providers and signers.

## Tutorials

{% hint style="info" %}
Legacy UI might be present in the videos, some things might be different
{% endhint %}

### How to Connect to a BSC Node? Full Archive Binance Smart Chain Node

{% embed url="https://youtu.be/o5hIlOuRPeA" %}

### Building a BSC Web App - BSC Node Connection

{% embed url="https://youtu.be/_vy5oIXg9uA" %}

### Create a Token on BSC - Launch a Binance Smart Chain BEP-20 Token

{% embed url="https://youtu.be/h4gQD7R1r94" %}
