---
description: >-
  Each dApp has an on-chain part (smart contracts) and an off-chain part
  (server). The server is used in order to collect data from the blockchain and
  serve it to clients such as web and mobile apps.
---

# Create a Moralis Server

## Create a Moralis Server

### What is a Moralis Server?

Each dApp is usually divided into 2 parts:

1. **On-chain part:** Smart Contracts, On-Chain Assets like tokens and NFTs, On-Chain transactions etc.
2. **Off-chain part:** Backend infrastructure that collect data from the blockchain, offers an API to clients like web apps and mobile apps, indexes the blockchain, provides real-time alerts, co-ordinates events that are happening on different chains, handles the user life-cycle and so much more.

Moralis Server is used in order to speed up the implementation of the **off-chain part**. Moralis Server is a bundled solution of all the features most dApps need in order to get going as soon as possible.

{% embed url="https://www.youtube.com/watch?v=txHnWDRB728" %}
See this video if you want to understand the power of Moralis Server and how it can help you build cross-chain dApps.
{% endembed %}

### Launching a Moralis Server

{% embed url="https://youtu.be/SYWdSg9KLCQ" %}
If you prefer to follow a video tutorial - this video covers a lot of what's covered in this article.
{% endembed %}

#### Sign up for a free account

Go to [Moralis](https://moralis.io) and sign up for a free account.

#### Create a Moralis Server

Click **Create new server** in the top right corner. You can use Moralis to develop dApps for mainnets, testnets and local devchains (for example Hardhat and Ganache).

For now, please select **Mainnet Server**.

![](<../../.gitbook/assets/Screenshot 2021-10-15 at 16.00.55.png>)

Next, select which networks you want your dApp to fetch data from. For the purpose of this demo we select Ethereum, Polygon, BSC and Avalanche.

![](<../../.gitbook/assets/Screenshot 2021-10-15 at 16.07.28.png>)

Now you will see your server in your dashboard, and we can move on and create a web app that talks to the server and is able to login users, fetch user data (tokens, NFTs, historical transactions), and so much more! All cross-chain by default, of course 🤯

![](<../../.gitbook/assets/Screenshot 2022-02-18 at 13.07.35.png>)

The server displays several important **indicators** as shown in the image above:

* `Network`: Network traffic per second
* `CPU`: Server's CPU Usage
* `RAM`: Server's RAM Usage
* `DISK`: Server's Disk Usage
* `Number of Users`: The number of users that has been authenticated in the server

## Migration from Legacy to Nitro Server

Every new server created in Moralis now will be Nitro by default. However, for those servers created before the launch of Moralis Nitro might still be using the legacy server. In order to upgrade the server to Nitro, simply install the coreservices plugin by clicking [here](https://admin.moralis.io/install/plugin/coreservices).

Keep in mind that once the coreservices plugin has been added, it can't be removed. This means that the migration from Legacy to Nitro will be **irreversible**. From your server, you can see `coreservices` as one of the plugins.

![](<../../.gitbook/assets/Screenshot 2022-02-26 at 20.52.10.png>)

Moralis Nitro server has a number of breaking changes that that are listed [here](https://forum.moralis.io/t/moralis-nitro-is-out/9267). One **important** changes when migrating from legacy to Nitro would be the removal of `TokenBalance` and `NFTOwners` tables from the database. This means once the server has been migrated, these tables will disappear, therefore you should instead replace any queries to these tables with some of our available [Web3APIs](https://docs.moralis.io/moralis-server/web3-sdk) to do the same job:

* `TokenBalance` -> [`getTokenBalances`](https://docs.moralis.io/moralis-server/web3-sdk/account#gettokenbalances)
* `NFTOwners` -> [`getNFTOwners`](https://docs.moralis.io/moralis-server/web3-sdk/token#getnftowners)
