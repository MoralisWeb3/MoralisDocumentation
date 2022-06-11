---
description: The list of chains supported by Web3API
---

# ⛓ Supported Chains

### Supported Chains

All helper functions have a **`chain`** option to specify which blockchain to get data from. The following are the currently supported values for the **`chain`** option. Any of the "Lookup Values" will match the corresponding chain.&#x20;

{% hint style="info" %}
If not specified the chain will default to Ethereum Mainnet.
{% endhint %}

| Chain                                                                                                           | Lookup Values                                                           |
| --------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------- |
| Ethereum Mainnet                                                                                                | `eth`, `mainnet`, `0x1`                                                 |
| Goerli _(Ethereum Testnet_)                                                                                     | `goerli`, `0x5`                                                         |
| Binance Smart Chain Mainnet                                                                                     | `bsc`, `binance`, `binance smart chain`, `0x38`                         |
| Binance Smart Chain Testnet                                                                                     | `bsc testnet`, `binance testnet`, `binance smart chain testnet`, `0x61` |
| Polygon _(Matic)_ Mainnet                                                                                       | `matic`, `polygon`, `0x89`                                              |
| Mumbai _(Matic Testnet)_                                                                                        | `mumbai`, `matic testnet`, `polygon testnet`, `0x13881`                 |
| Avalanche Mainnet                                                                                               | `avalanche`, `0xa86a`                                                   |
| Avalanche Testnet                                                                                               | `avalanche testnet`, `0xa869`                                           |
| Fantom Mainnet                                                                                                  | `fantom`, `0xfa`                                                        |
| Cronos Mainnet                                                                                                  | `cronos`, `0x19`                                                        |
| <p>Local Dev Chain <em>(Ganache, Hardhat)</em></p><p><strong>(doesn't work currently with Web3API)</strong></p> | `ganache`, `hardhat`, `localdevchain`, `local devchain` ,`dev`, `0x539` |

{% hint style="warning" %}
Ropsten, Rinkeby and Kovan are no longer supported starting June 2022
{% endhint %}

{% hint style="warning" %}
Local Dev Chain (Ganache, Hardhat) doesn't work currently with Web3API.
{% endhint %}
