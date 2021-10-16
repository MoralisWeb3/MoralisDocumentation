---
description: >-
  Video Tutorials on Setting Up Ganache and Hardhat and Connecting Them to Your
  Moralis Server.
---

# Ganache & Hardhat Setup

## Ganache

### Setting Up Ganache and MetaMask

{% embed url="https://youtu.be/nUEBAS5r4Og" %}

### Connecting Ganache to Moralis

A note for Mac users, download `frp_0.36.2_darwin_amd64.tar.gz` for the Ganache proxy server.

{% embed url="https://youtu.be/aRRS394is1U" %}

## Hardhat

### Connecting Hardhat to MetaMask - Ivan on Tech Explains

{% embed url="https://youtu.be/FTDEX3S1eqU" %}

### Connecting Hardhat to Moralis - Real-Time and Historical Transactions

{% embed url="https://youtu.be/LdNb3uW6Vl4" %}

### Solidity Writing to Console (console.log) - Hardhat Introduction

{% embed url="https://youtu.be/5V5vDJhafwk" %}

## Forking Mainnet (not supported)

Note, the "Mainnet Forking" feature in [Ganache CLI](https://github.com/trufflesuite/ganache-cli) and [Hardhat](https://hardhat.org/guides/mainnet-forking.html) is not yet supported. Transactions and contract events will not sync due to the block numbers starting at an unexpected value. We hope to have support for this in the future.

## Changing chains

Note, Moralis doesn't support changing local dev chains. Once you've used a local dev chain on a Moralis Server - you have to keep using it. Changing chains may result in unexpected results like transactions not being synced at all or just partially synced. If you need to switch chains you will have to create another server. We hope to support changing chains on the same server in the near future!

## Connect Through the moralis-admin-cli

You can connect your local dev chain to Moralis by making use of the `moralis-admin-cli`.

To get started, you need to install it by running the following code in the terminal: 

```bash
npm install -g moralis-admin-cli
```

You also need to have frpc on your computer. You can get it here: [https://github.com/fatedier/frp/releases](https://github.com/fatedier/frp/releases).

After you've installed the moralis-admin-cli, you can head to the admin panel and open up the details on any of your servers.

You will need the CLI API Key and CLI API secret shortly.

![](<../../.gitbook/assets/image (49).png>)

You can now run the following command to start the process (Insert your own key, secret, and path):

```bash
moralis-admin-cli connect-local-devchain --moralisApiKey FAM6pYzBKjcM
 --moralisApiSecret 7thQGNKKqX8s --frpcPath "C:\Program Files\frpc\frpc.exe"
 
```

To get more information you can write: 

```bash
moralis-admin-cli connect-local-devchain help
```

This will show you a more detailed description of the command and show how you can call the command without the need to specify the arguments each time.
