---
description: Frequently Asked Questions
---

# FAQ

## My Moralis app ID is publicly visible on the frontend. Is this a security risk?

No. The app ID ("Application ID") is meant to be public and is included in all requests to Moralis. It's ok to include the app ID in a public code repository, such as GitHub. With the app ID, a user can create accounts and call any type of cloud function. To control when new users can sign up, use a "[beforeSave" trigger](https://docs.moralis.io/triggers#beforesave) on the user collection. To learn more about how to lock down your app, see the [Security docs](https://docs.moralis.io/security).

Please Note: You want to protect the master key since it can override all permissions and has full access to read, write, and delete. It's best to use the master key only on the server (i.e., cloud functions). Never use the master key on the frontend.

## FRPC

### What version should I use on Mac?

* frp_x.xx.x_darwin_amd64.tar.gz

### How should I run frpc on Mac?

* Open a terminal.
* Navigate to frpc directory.
* Type `./frpc -c frpc.ini`.

### Why does Mac say that `frpc` is from an "Unidentified Developer"?

This is because `frpc` is not signed for Mac. To allow it to run, follow these steps:

* Navigate to `frpc` folder in Finder.
* Right-click on the `frpc` executable while pressing the "**Ctrl" **key.
* Select **Open**.

Mac will give you information about the risks of overriding the system's security. Please read it carefully and click "**Open"** in the pop-up if you agree.

### What are the risks of giving permissions to `frpc`, even if it is from an "Unidentified Developer"?

`frpc` executable is not signed by Apple. It means that the OS cannot know what kind of code it's going to execute. By accepting to override, or skip the system security, you're telling the OS that you trust the app. Please do so at your own responsibility and risk. To avoid any issues, make sure you've downloaded `frpc` from the official repository at [https://github.com/fatedier/frp/releases](https://github.com/fatedier/frp/releases).

## OpenSea NFTs

### Why are number of NFTs different on OpenSea compared to Moralis API?

Our API can only read data that is public onchain. Lazy minted NFTs on an OpenSea shared contract are stored only in a centralized OpenSea database until the first transfer.

You can find more information on the OpenSea blog: [https://opensea.io/blog/announcements/introducing-the-collection-manager/](https://opensea.io/blog/announcements/introducing-the-collection-manager/).

Another reason may be that the NFTs from the OpenSea inventory are in different chains. In this case, you need to make API requests for each network.

