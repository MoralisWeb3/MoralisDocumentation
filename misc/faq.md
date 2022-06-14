---
description: Frequently Asked Questions
---

# FAQ

## My Moralis app ID is publicly visible on the frontend. Is this a security risk?

No. The app ID ("Application ID") is meant to be public and is included in all requests to Moralis.

**What can someone do if they have App ID and Server URL?**

1.  **Sign up as a new user.**

    Each dapp by default welcomes any user to sign up. If you have app id and server url you can try to sign up as a user.

    To control how new users are created you can use use a "[beforeSave" trigger](https://docs.moralis.io/triggers#beforesave) on the user collection.
2.  **Try to call Cloud Functions**.

    Think of Cloud Functions as endpoints in an API.

    Anyone can try calling them. You can setup [Roles](../moralis-dapp/database/security.md) in your dapp in order to control which Cloud Functions can be called by which users.

    For example you can ensure that some Cloud Functions can be called by anyone - even if they don't have an account in your dapp.

    Then you can specify that some other Cloud Functions can only called by registered users.

    And finally you can specify that some specific Cloud Functions can only be called by registered users with a specific role. See the [Security docs](../moralis-dapp/database/security.md) for more details.
3.  **Try to add new Classes or Columns to the database.**

    By default Moralis allows clients to create new Classes in the database and to add columns to existing Classes. This speeds up the development a lot.

    Of course this should be disabled when you go to production.

    Read here about disabling [Client Class Creation](https://docs.moralis.io/moralis-dapp/database/security#client-class-creation).

To learn more about how to lock down your app, see the [Security docs](../moralis-dapp/database/security.md).

### ALWAYS PROTECT THE MASTER KEY

<mark style="color:red;">**Please Note: You want to protect the**</mark>\*\* <mark style="color:red;background-color:yellow;">**master key**</mark> \*\*<mark style="color:red;">\*\*since it can override all permissions and has full access to read, write, and delete. It's best to use the master key only on the server (i.e., cloud functions). Never use the master key on the frontend.\*\*</mark>

## Why do you use the signing messages and other Dapps don't?

This is the general standard for verifying that you really own the wallet. It is used, for example, by Opensea, Rarible and so on.

Authorization through Moralis gives for user access to his [user object](https://docs.moralis.io/moralis-dapp/users/crypto-login#user-object), in which, for example, private information can be stored, and the user also gets the right to change his data. For this we use signing messages. This is an absolutely safe way: It does not export private keys, does not allow the spending of tokens, and does not require gas fees.

If you do not need users to be stored in the database, you can use `enableWeb3()` and get eth\_address of users using default web3 methods.

## How to ask for help on the forum and Discord

If you want to get help quickly you need to follow the next steps:

1. Provide your server subdomain (what is a server subdomain?)
2. Describe your issue in details
3. If you share your code - always send it as a formatted code (not screenshots) or github repository
4. Attach screenshots, error messages

### What is a server subdomain?

You can find it in the Moralis admin panel after clicking on the "View Details" button

![](<../.gitbook/assets/image (117).png>)

## I just minted an NFT - why is it not showing up in the API?

### Ensure you are compliant with the standards

In order for the NFT to show up in the API it needs to be compliant with [ERC721](https://eips.ethereum.org/EIPS/eip-721) or [ERC1155](https://eips.ethereum.org/EIPS/eip-1155) standards.

Both of these standards require the implementation of `supportsInterface` method from ERC165 standard.

If you use [OpenZeppelin contracts](https://docs.openzeppelin.com/contracts/2.x/api/token/erc721) this is done automatically for you.

A way to double check this is to open your contract in Etherscan and ensure it says `ERC721` or `ERC1155` on your contract page.

![A contract like this will show up in the NFT API.](<../.gitbook/assets/Screenshot 2021-12-14 at 12.51.44.png>)

![A contract like this won't show up in the NFT API.](<../.gitbook/assets/Screenshot 2021-12-14 at 12.49.43.png>)

## Why is metadata null for some NFTs?

The goal of Moralis is to always offer you fully resolved metadata so that you don't have to resolve it yourself and save load time in your app.

### Why is it null?

Sometimes the token_uri is invalid, and when we try to fetch the metadata we can not acceess it. In that case the metadata will be null.

## Why is metadata sometimes outdated?

Automatic re-sync feature was relased: [`https://forum.moralis.io/t/moralis-now-automatically-updates-nft-metadata/14816`](https://forum.moralis.io/t/moralis-now-automatically-updates-nft-metadata/14816). Now, the metadata is queued automatically for resync when a token id is accessed, but only if the token uri points to IPFS. If the token uri doesn't point to IPFS, you still have to use [`reSyncMetadata`](https://docs.moralis.io/moralis-dapp/web3-sdk/token#resyncmetadata) API that can help you to manually trigger metadata re-sync on specific token.

## What is the batch request limit on Moralis Speedy Nodes?

Currently, we have a batch limit of 50 requests in a batch. In most production cases, it might not be suitable for your needs. Therefore we recommend you to use our Web3 APIs to get all transactions in a block and all contents of a transaction:

* [getBlock](https://docs.moralis.io/moralis-dapp/web3-api/native#getblock)
* [getTransaction](https://docs.moralis.io/moralis-dapp/web3-api/native#gettransaction-new)
* or check [any other of our endpoints](https://docs.moralis.io/moralis-dapp/web3-api/native)

If Web3 API provided doesn't suit your needs and prefer to use nodes with larger batches - you can sign up at to our partner [here](https://moralis.io/largenodes).

## FRPC

### What version should I use on Mac?

* frp\_x.xx.x\_darwin\_amd64.tar.gz

### How should I run frpc on Mac?

* Open a terminal.
* Navigate to frpc directory.
* Type `./frpc -c frpc.ini`.

### Why does Mac say that `frpc` is from an "Unidentified Developer"?

This is because `frpc` is not signed for Mac. To allow it to run, follow these steps:

* Navigate to `frpc` folder in Finder.
* Right-click on the `frpc` executable while pressing the "**Ctrl"** key.
* Select **Open**.

Mac will give you information about the risks of overriding the system's security. Please read it carefully and click "**Open"** in the pop-up if you agree.

### What are the risks of giving permissions to `frpc`, even if it is from an "Unidentified Developer"?

`frpc` executable is not signed by Apple. It means that the OS cannot know what kind of code it's going to execute. By accepting to override, or skip the system security, you're telling the OS that you trust the app. Please do so at your own responsibility and risk. To avoid any issues, make sure you've downloaded `frpc` from the official repository at [https://github.com/fatedier/frp/releases](https://github.com/fatedier/frp/releases).

## OpenSea NFTs

### Why are number of NFTs different on OpenSea compared to Moralis API?

Our API can only read data that is public onchain. Lazy minted NFTs on an OpenSea shared contract are stored only in a centralized OpenSea database until the first transfer.

You can find more information on the OpenSea blog: [https://opensea.io/blog/announcements/introducing-the-collection-manager/](https://opensea.io/blog/announcements/introducing-the-collection-manager/).

Another reason may be that the NFTs from the OpenSea inventory are in different chains. In this case, you need to make API requests for each network.

## Sleeping Servers

### What are sleeping servers?

Moralis is on a mission to provide available tools to the web3 development community. This means that we have very generous free plans. This also means that we allow anyone to sign up and spin up Moralis Servers.

However the fact that Moralis Servers are free means that some of them are left running indefinitely without any use. We want all our resources to go to building amazing web3 tech and growing the Moralis Community - we don't want to waste resources running inactive servers.

Thankfully we found a very simple solution 🤩

If you are a free tier user you need to confirm that you are still using your servers every 3 days. This is how it works 👇

### 😴 Prevent Your Servers from Sleeping

Every 3 days you will get an email prompting you to login to your Moralis account and prevent your server from going to sleep by clicking the _**"Prevent Sleep"**_ button.

You have 24 hours to prevent your Moralis server from sleeping. If you don't prevent your server from sleeping it will be temporarily shut down (it will go to sleep 🥱).

When a server is shutdown you will experience down time until you wake it up by clicking the _**"Wake Up"**_.

Don't worry - the wake up process will take approximately 30 seconds and your server will be restored to exactly the same state that it was before.

### 🛑 If Your Server Stays Shutdown for 24h <a href="#servershutdown" id="servershutdown"></a>

If a server stays shutdown for a period longer than 24 hours it will be terminated.

But don't worry - **we will back up your server configuration and save it in archive for 1 month**. You will need to recover it by clicking the _**"Recover"**_ button.

This process is a bit longer than a normal "Wake Up" and can take a few minutes.

Your server will be restored but it will be a fresh server with a new IP and a new URL.

<mark style="color:red;">**If your server stays shutdown for over 1 month - the archive version of it will be deleted permanently and you won't be able to get the server back.**</mark>

### 🤩 How to Avoid Sleeping Server

To avoid the Sleeping Servers please upgrade to a paid plan.

This will support the Moralis developers so that we can continue delivering world-class open source web3 tools without wasting money on inactive servers 🙏

You can upgrade by [clicking here](https://moralis.io/pricing).

As you know we are also working on a self-hosted version of the Moralis Server. When it's out you will be able to host your server on your own server in the future 🏆

### 🤝 If You Need Help

If you want more information regarding sleeping servers or need help please email us at hello@moralis.io and we will help asap!

Keep BUIDLing 👷‍♂️
