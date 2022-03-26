---
description: >-
  Moralis is infinitely extendable via third-party plugins. If your dapp is
  built on Moralis it's future-proof as plugin developers ensure that all new
  tech is available with just one line of code.
---

# 🔌 Plugins

New technologies, services, and APIs are created each and every day. Many developers spend months learning, integrating, and maintaining connections to third-party services.

Using Moralis you save months of development work as Moralis allows any to develop, distribute and monetize plugins. Meaning that your Moralis dapp can be infinitely extended to work with other technologies via plugins.

Whether you need to integrate fiat onramps, connect your Moralis dapp to KYC providers, integrate DEXes, lending protocols, other dapps - there is probably a plugin for that 🤯.

### What are plugins?

Plugins are extensions to your Moralis server that can execute their own code, use any packages and libraries, connect to external APIs and so much more. This gives them massive flexibility and power.

The plugins can have different permissions. For example, they can request to access your database, update data in your database, listen for changes in your database etc

This opens up many new use-cases, imagine a KYC plugin that can run KYC for all your existing users in the DB. Or a DEX plugin that can allow your users to swap their assets on-chain.

Below is an example of a simple plugin allowing your users to buy crypto with fiat.

{% embed url="https://www.youtube.com/watch?v=5MlTnoBm7YQ&ab_channel=MoralisWeb3" %}
Example of a fiat gateway being installed as a Moralis Plugin.
{% endembed %}

When you watch the video above you see the smooth integration plugins have with the SDK. As soon as a new plugin is installed you have all the functions and features of the plugin visible in the SDK with code completion.

### How to get started?

Get started by installing your first plugin and interacting with it straight from the SDK.

{% content-ref url="untitled.md" %}
[untitled.md](untitled.md)
{% endcontent-ref %}

### Security Considerations

When installing plugins you need to ensure that you know what you are installing - especially when the plugin is requesting permission to your database. If you are unsure what the plugin does and if you can't see open source code - it's recommended to try to contact the developer of the plugin or simply not to install that plugin.

Remember that you have the responsibility for the plugins you allow to run in your Moralis dapps.

###

###
