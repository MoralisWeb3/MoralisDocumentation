---
description: >-
  Moralis Solana API is a powerful way of getting all kinds of Solana blockchain data.
  You can see all of the different functions documented in the sections below.
---

# Intro

As a blockchain developer, you have to be able to fetch different information from the Solana Blockchain such as NFT metadata, user portfolio balances, and so much more!

You find all of these powerful functions in the `Moralis.SolanaAPI` namespace in the Moralis SDK.

It's also possible to access all of this functionality through REST API from your own backend.

### Configuring Web3API

After you have initialized your application with `Moralis.start()`, it will automatically load the SolanaAPI module. You don't need to make any additional steps to start using SolanaAPI.

{% hint style="info" %}
[Initialize Moralis guide](https://docs.moralis.io/moralis-server/getting-started/connect-the-sdk#initialize-the-sdk)
{% endhint %}

### Supported Networks

All helper functions have a `network` option to specify which Solana cluster to get data from. The following are the currently supported values for the `network` option. Any of the "Lookup Values" will match the corresponding network. If not specified the `network` value will default to `mainnet`.

| Network      | Lookup Values |
| ------------ | ------------- |
| Mainnet Beta | `mainnet`     |
| Devnet       | `devnet`      |
