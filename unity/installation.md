---
description: >-
  Setting up and installing the sdk
---

### Web3 Unity Boilerplate

- The Unity Boilerplate include C# Moralis SDK and has an example Unity scene allowing you to login your users via their wallets, read their tokens and NFTs, interact with smart contracts and much more.

{% embed url="https://github.com/ethereum-boilerplate/ethereum-unity-boilerplate" %}
Web3 Unity Boilerplate
{% endembed %}

### using upm (unity package manager)

#### installing the SDK

Here are the steps:

- In your open unity project.
- Open Edit -> Preferences, make sure that 'Git packages' is checked.
- Open Windows -> Package Manager
- Find the '+' icon at the top left of the Package Manager Window, click it and select 'Add package from git URL ...'
- Enter https://github.com/metaversesdk/web3unity.git#upm and click 'ADD'.
- When the package loads, expand Samples and import the demo.
- Close the Package Manager.

#### Configuring the project

A setup wizard should appear to configure your AppID and SetupUrl

If you dont have one, you can create a new dapp here
{% content-ref url="moralis-dapp/getting-started/create-a-moralis-dapp.md" %}
[🚀 Create a Moralis Dapp](moralis-dapp/getting-started/create-a-moralis-dapp.md)
{% endcontent-ref %}

When done, there will be a example scene to try out.
