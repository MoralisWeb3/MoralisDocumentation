---
description: Frequently Asked Questions
---

# FAQ

## My Moralis app ID is publicly visible on the frontend. Is this a security risk?

No. The app ID ("Application ID") is meant to be public and is included in all requests to Moralis.

**What can someone do if they have App ID and Server URL?**

1.  **Sign up as a new user.**&#x20;

    Each dapp by default welcomes any user to sign up. If you have app id and server url you can try to sign up as a user.&#x20;

    To control how new users are created you can use use a "[beforeSave" trigger](https://docs.moralis.io/triggers#beforesave) on the user collection.
2.  **Try to call Cloud Functions**.&#x20;

    Think of Cloud Functions as endpoints in an API.&#x20;

    Anyone can try calling them. You can setup [Roles](../moralis-server/database/security.md) in your dapp in order to control which Cloud Functions can be called by which users.&#x20;

    For example you can ensure that some Cloud Functions can be called by anyone - even if they don't have an account in your dapp.&#x20;

    Then you can specify that some other Cloud Functions can only called by registered users.

    And finally you can specify that some specific Cloud Functions can only be called by registered users with a specific role. See the [Security docs](https://docs.moralis.io/security) for more details.
3.  **Try to add new Classes or Columns to the database.**&#x20;

    By default Moralis allows clients to create new Classes in the database and to add columns to existing Classes. This speeds up the development a lot.&#x20;

    Of course this should be disabled when you go to production.

    Read here about disabling [Client Class Creation](https://docs.moralis.io/moralis-server/database/security#client-class-creation).

To learn more about how to lock down your app, see the [Security docs](https://docs.moralis.io/security).

### ALWAYS PROTECT THE MASTER KEY

<mark style="color:red;">**Please Note: You want to protect the**</mark>** **<mark style="color:red;background-color:yellow;">**master key**</mark>** **<mark style="color:red;">**since it can override all permissions and has full access to read, write, and delete. It's best to use the master key only on the server (i.e., cloud functions). Never use the master key on the frontend.**</mark>

## Why do you use the signing messages and other Dapps don't?

This is the general standard for verifying that you really own the wallet. It is used, for example, by Opensea, Rarible and so on.

Authorization through Moralis gives for user access to his [user object](https://docs.moralis.io/moralis-server/users/crypto-login#user-object), in which, for example, private information can be stored, and the user also gets the right to change his data. For this we use signing messages. This is an absolutely safe way: It does not export private keys, does not allow the spending of tokens, and does not require gas fees.&#x20;

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

![A contract like this won't show up in the NFT API. ](<../.gitbook/assets/Screenshot 2021-12-14 at 12.49.43.png>)

## Why is metadata null for some NFTs?

The goal of Moralis is to always offer you fully resolved metadata so that you don't have to resolve it yourself and save load time in your app.

### Why is it null?

Some NFTs have their metadata is hosted on centralised servers. These servers sometimes have rate limits preventing Moralis to fully index the collections that have their metadata stored on such servers. In such cases the metadata may not be resolved. We are working all the time to extend our coverage of metadata.

### How to get metadata?

Even though Moralis can't always resolve the metadata for you we will always give you `token_uri` so that you can always resolve it yourself.

When working with Moralis NFT API always check if metadata is given to you.&#x20;

If yes - use it! We just saved you load time and improved the performance of your app 🙌

If no - make a request to `token_uri` provided and get the metadata yourself.

## Why is metadata outdated?

There are millions of NFTs across the different blockchains that Moralis supports. The vast majority of NFTs never change metadata, therefore Moralis doesn't currently re-sync metadata. This is something we may do in the future!

In the near future we will add a way for you to trigger metadata re-sync on a specific token. We know that this feature is important when you develop your NFT and you may be experimenting with different metadata and update it often during the development of your app.

Join our [Discord](https://moralis.io/mage) to be updated when this feature is released!&#x20;

If you really want us to add the auto re-sync feature - add it or upvote it here: [https://roadmap.moralis.io/b/feature-requests/](https://roadmap.moralis.io/b/feature-requests/)

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

Thankfully we found a very simple solution 🤩&#x20;

If you are a free tier user you need to confirm that you are still using your servers on a weekly basis. This is how it works 👇

### 😴 Prevent Your Servers from Sleeping&#x20;

Once a week you will get an email prompting you to login to your Moralis account and prevent your server from going to sleep by clicking the _**"Prevent Sleep"**_ button.&#x20;

You have 48 hours to prevent your Moralis server from sleeping. If you don't prevent your server from sleeping it will be temporarily shut down (it will go to sleep 🥱).&#x20;

When a server is shutdown you will experience down time until you wake it up by clicking the _**"Wake Up"**_.

Don't worry - the wake up process will take approximately 30 seconds and your server will be restored to exactly the same state that it was before.

### 🛑 If Your Server Stays Shutdown for 48h

If a server stays shutdown for a period longer than 48 hours it will be terminated.&#x20;

But don't worry - we will back up your server configuration and save it in archive. You will need to recover it by clicking the _**"Recover"**_ button.&#x20;

This process is a bit longer than a normal "Wake Up" and can take a few minutes.

Your server will be restored but it will be a fresh server with a new IP and a new URL.

### 🤩 How to Avoid Sleeping Server

To avoid the Sleeping Servers please upgrade to a paid plan.&#x20;

This will support the Moralis developers so that we can continue delivering world-class open source web3 tools without wasting money on inactive servers 🙏

You can upgrade by [clicking here](https://moralis.io/pricing).

As you know we are also working on a self-hosted version of the Moralis Server. When it's out you will be able to host your server on your own server in the future 🏆

### 🤝 If You Need Help

If you want more information regarding sleeping servers or need help please email us at hello@moralis.io and we will help asap!

Keep BUIDLing 👷‍♂️
