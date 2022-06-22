---
description: How to install and use plugins from Moralis Store.
---

# Install and Remove Plugins

## Open Plugin List <a href="#open-plugin-list" id="open-plugin-list"></a>

Each Moralis Server instance has its own set of plugins which are managed separately. Expand the server view and click on the "Plugins" button.​

<p align="center">
  <img src="../../.gitbook/assets/Plugins1.png">
</p>

<p align="center">
  <img src="../../.gitbook/assets/Plugins2.png">
</p>

This will bring up the list of plugins. The "Installed" tab will list the currently installed plugins, where the "Browse" tab lists all available plugins.‌

## Plugin List <a href="#installing-plugins" id="installing-plugins"></a>

Here you can find the plugins already installed. If you want to install plugins click the "Go to Plugin Store!" button.

![](<../../.gitbook/assets/Plugins3.png>)

Already installed plugins will have an "Installed" badge.‌

## Moralis Plugin Store

In the Moralis Plugin Store, you can find a bunch of plugins that will make your dApp development easier. This tab provides information about the plugin author, its purpose and rating.

![](<../../.gitbook/assets/image (102).png>)

If you want to know more details about the plugin or install it click on the "Read More" button.

## Plugin Description and Installing

On this tab, you can read a detailed description of the plugin and instructions for installing it.

![](<../../.gitbook/assets/image (103).png>)

![](<../../.gitbook/assets/image (104).png>)

If you want to add a plugin to your server click the "Install the plugin" button.

## Plugin Installing

After clicking the "Install the plugin" button, you will see the "Install Plugin" modal window. You need to select the server to which you want to add the plugin.

![](<../../.gitbook/assets/Plugins4.png>)

## Fiat Plugin Installing

After you choose the server to install the plugin on. You will need to provide "The Onramper API key". You can get it on the [https://onramper.com/](https://onramper.com) website.

![Provide the api key in the special field](<../../.gitbook/assets/Plugins5.png>)

After the plugin installation is finished, your server will automatically reboot:

![](<../../.gitbook/assets/Plugins7.png>)

And you will see the plugin in your Plugins tab:

![](<../../.gitbook/assets/Plugins6.png>)

After installing the plugin you can start using it with the following code:

`Moralis.Plugins.fiat.buy()`

This will open up a new browser window and guide the user through the steps to purchase cryptocurrencies.

## Removing Plugins <a href="#removing-plugins" id="removing-plugins"></a>

Go to the Plugins tab. Scroll down the list until you find the plugin you want to remove and press the "Garbage Can" button.​

![](<../../.gitbook/assets/Plugins8.png>)
