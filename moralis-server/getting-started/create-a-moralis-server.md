---
description: >-
  Each dapp has an onchain part (smart contracts) and an offchain part (server).
  The server is used in order to extract data from the blockchain and serve it
  to clients such as web and mobile apps.
---

# Create a Moralis Server

## What is a Moralis Server?

Each dapp is usually divided into 2 parts:

1. **On-chain part:** Smart Contracts, On-Chain Assets like tokens and NFTs, On-Chain transactions etc
2. **Off-chain part: **Backend infrastructure that extract data from the blockchain, offers API to clients like web apps and mobile apps, indexes the blockchain, provides real-time alerts, co-ordinates events that are happening on different chains, handles user life-cycle and so much more

Moralis Server is used in order to speed up the implementation of the **off-chain part**. Moralis Server is a bundled solution of all the features most dapps need in order to get going as soon as possible.

{% embed url="https://www.youtube.com/watch?v=txHnWDRB728" %}
See this video if you want to understand the power of Moralis Server and how it can help you build cross-chain dapps.
{% endembed %}

## Launching a Moralis Server

### Sign up for a free account

Go to [Moralis](https://moralis.io) and sign up for a free account.

### Create a Moralis Server

Click _Create new server_ in the top right corner. You can use Moralis to develop dapps for mainnets, testnets and local devchains (for example Hardhat and Ganache).

For now, please select _Mainnet Server_.

![](<../../.gitbook/assets/Screenshot 2021-10-15 at 16.00.55.png>)

Next, select which networks you want your dapp to fetch data from. For the purpose of this demo we select Ethereum, Polygon, BSC and Avalanche.

![](<../../.gitbook/assets/Screenshot 2021-10-15 at 16.07.28.png>)

Now you will see your server in your dashboard and we can move on and create a web app that talks to the server and is able to login users, fetch user data (tokens, NFTs, historic transactions) and so much more! All cross chain by default of course 🤯

![](<../../.gitbook/assets/Screenshot 2021-10-15 at 16.14.30.png>)
