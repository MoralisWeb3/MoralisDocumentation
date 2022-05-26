# Why use Moralis?

## What is Moralis?

**Moralis provides a single workflow for building high performance dapps. Fully compatible with your favorite web3 tools and services.** Moralis provides managed backend for blockchain projects. Automatically syncing the balances of your users into the database, allowing you to set up on-chain alerts, watch smart contract events, build indexes, and so much more. All features are accessed through an easy-to-use SDK. All features Moralis provides are cross-chain by default 🤯.

## Why use Moralis?

Moralis is the fastest way to build and deploy dApps on Ethereum, BSC, Polygon, Solana, and Elrond (more coming). All Moralis dApps are cross-chain by default. Building on Moralis ensures that your dApp is future-proof. Even if new blockchains are invented, your dApp will instantly work on any chain.

Whether you are building your first blockchain project or are already a seasoned developer - Moralis will make your projects easier to build, maintain and improve.

The video below explains more in-depth what Moralis is and how it helps your project.

{% embed url="https://www.youtube.com/watch?v=txHnWDRB728&ab_channel=MoralisWeb3" %}
Short lecture explaining the value-proposition of Moralis.
{% endembed %}

## What are Moralis Servers?

At the core of every dApp built with Moralis is a Moralis Server. Together with the Moralis SDK, it's what allows you to quickly create a dApp with user authentication and blockchain data such as user token balances, NFTs, transactions, and events.

Let's quickly summarize the different components of a Moralis Server that you will be using.

### Database

Here is where all of your data will be stored. For example, when a user signs in to your dApp using [crypto wallet authentication](https://docs.moralis.io/moralis-dapp/users/crypto-login), that wallet address will automatically be saved to your database together with any data you have configured, such as token balances, historical transactions, or events.

You can then use this data instantaneously in your dApp frontend.

You can read more under the sections: [Moralis Server Database](moralis-dapp/database/) and [User Authentication](moralis-dapp/users/web3-login.md).

### Cloud Code

If you need to execute backend code in your dApp, you can do so by using [Moralis' Cloud Code](moralis-dapp/cloud-code/) feature. Maybe you need to do aggregation or filtering on data that requires computation on the backend. By using cloud code, you can write functions in JavaScript, which can then be triggered by either calling it from your dApp, when certain events happen or triggered by a scheduled job.

You can read more about this in the [cloud code section](moralis-dapp/cloud-code/).

### The Moralis SDK

Moralis' SDK is how we tie all of this together. Our JavaScript SDK is how your dApp interacts with your Moralis Server. Using the SDK, you can authenticate users, either through username and password or through a crypto wallet like MetaMask You can also use the SDK to get and set user data to fetch balances, NFTs, events, or transactions.

You can read more about the SDK by[ clicking here](moralis-dapp/connect-the-sdk/).

## Welcome to the Moralis Documentation

{% embed url="https://www.youtube.com/watch?v=TAJnP8SLVI4" %}

We're excited for you to discover all the cool things you can build using Moralis with just a few lines of code. Before we get started, let's go over some important information.

## Expectations

* The docs assume that you have some programming knowledge.
* The docs are a work in progress and receive regular updates.
* If you find something confusing in the docs or have suggestions for improvements let us know by posting in the [Moralis forum](https://forum.moralis.io).
* If you find a bug in the Moralis SDK or server dashboard, please report it in [this GitHub repo](https://github.com/MoralisWeb3/issue-tracker), along with a detailed description and steps to reproduce the issue. For technical questions, or if you need help with your code, then please create a post in the [Moralis forum.](https://forum.moralis.io) If you're unsure where to post, then please ask by creating a post in the [Moralis forum](https://forum.moralis.io) or on the [FAQ & How to Get Help](https://forum.moralis.io/c/faq/12) page.

## Prerequisites

The documentation assumes that you have some type of knowledge with JavaScript, working with objects, querying databases, and some Web3 development. See the [Prerequisites](introduction/pre-requisites.md) page for more details.

{% content-ref url="introduction/pre-requisites.md" %}
[pre-requisites.md](introduction/pre-requisites.md)
{% endcontent-ref %}

If you're new to Web3 development, then please watch the following video:

{% embed url="https://youtu.be/hy7jCQdC2Wg" %}

For more experienced developers, then make sure to watch the following video on how to build a dApp with Moralis using Truffle and Ganache.

{% embed url="https://youtu.be/rd0TTLjQLy4" %}

## Setup Your First dApp with Moralis

See the [Getting Started ](https://docs.moralis.io/moralis-dapp/getting-started)section to guide you through setting up your first server with Moralis and how to integrate it with your dApp:

{% content-ref url="moralis-dapp/getting-started/" %}
[getting-started](moralis-dapp/getting-started/)
{% endcontent-ref %}

Or follow our guide on how to make a simple dApp in three minutes:

{% content-ref url="guides/build-a-simple-dapp-in-3-minutes.md" %}
[build-a-simple-dapp-in-3-minutes.md](guides/build-a-simple-dapp-in-3-minutes.md)
{% endcontent-ref %}
