---
description: >-
  Moralis is easily integrated with any EIP1193 provider. This articles explains
  how it's done!
---

# 🤖 Custom Auth (any EIP1193 provider)

## Connect any EIP1993 Provider

You can implement your own `connector`, which extends the [AbstractConnector](https://github.com/MoralisWeb3/Moralis-JS-SDK/blob/beta/src/Web3Connector/AbstractWeb3Connector.js) class.

This class should implement

* An `activate()` function that resolves to an object with:
* provider: A valid [EIP-1193 provider ](https://eips.ethereum.org/EIPS/eip-1193)
* chainId (optional): the chain that is being connected to (in hex)
* account (optional): the account of the user that is being connected
* Subscribes to the EIP-1193 events. This should be mostly done automatically by calling `this.subscribeToEvents(provider)` in the `activate` function.
* the `type` field to indicate a name for the connector
* (optionally) a `deactivate` function that extends the default deactivate function. Implement this when you need to clean up data/subscriptions upon ending/switching the connection.

Then you can include this `CustomConnector` in the authenticate/enableWeb3 call as an option:

```
Moralis.authenticate({ connector: CustomConnector })
```

See for example implementations:

* The [WalletConnectConnector](https://github.com/MoralisWeb3/Moralis-JS-SDK/blob/beta/src/Web3Connector/WalletConnectWeb3Connector.js), that is used when you specify `provider: "walletconnect"`.
* The [InjectedWeb3Connector](https://github.com/MoralisWeb3/Moralis-JS-SDK/blob/beta/src/Web3Connector/InjectedWeb3Connector.js) (metamask), that is used when you don’t specify any connector.
