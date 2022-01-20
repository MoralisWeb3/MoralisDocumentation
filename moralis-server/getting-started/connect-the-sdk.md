---
description: >-
  Once you have your Moralis Server launched it's time to connect to it via the
  Moralis SDK. This guide will show you how you can do it in just a few easy
  steps.
---

# Connect the SDK

This guide will show you the process using Vanilla Javascript. Moralis has dedicated boilerplates for React, Angular and other popular frameworks.

{% content-ref url="boilerplate-projects.md" %}
[boilerplate-projects.md](boilerplate-projects.md)
{% endcontent-ref %}

## Adding Moralis to Your Web Page

### Creating an empty page

The first step is to create an empty page we call `index.html` and `main.js` in the same directory and import the **moralis** script alongside our `main.js` file. We include two buttons on the page - one for logging in and one for logging out.

#### **`index.html`**
```html
<!DOCTYPE html>
<html>
  <head>
    <title>Vanilla Boilerplate</title>
    <script src="https://unpkg.com/moralis/dist/moralis.js"></script>
  </head>

  <body>
    <h1>Moralis Hello World!</h1>

    <button id="btn-login">Moralis Metamask Login</button>
    <button id="btn-logout">Logout</button>

    <script type="text/javascript" src="./main.js"></script>
  </body>
</html>
```

> The above example imports the **latest** version of Moralis. Be aware of this when running your code in production. It's always better to specify a version. That way, you can update the version yourself and check if there are any breaking changes. 
> 
> To do so:
> Change the `src` of the script tag to specify the latest stable version of Moralis like:
> `https://unpkg.com/moralis@<VERSION>/dist/moralis.js`
> For the latest release version, you can check the [Releases on GitHub](https://github.com/MoralisWeb3/Moralis-JS-SDK/releases)
> For example: `<script src="https://unpkg.com/moralis@1.0.3/dist/moralis.js"></script>`
> 

#### **`main.js`**
```javascript
/* TODO: Add Moralis init code */
```

### Initialize the SDK

In order to initialize the SDK you need to fetch _Server URL_ and _APP ID_ from your Moralis Dashboard. Go to your Moralis Dashboard and click on _View Details_ next to the server name of your server.

![](<../../.gitbook/assets/Screenshot 2021-10-15 at 17.10.09.png>)

Next you can initialize your server using _`Moralis.start`_ function.

#### **`main.js`**
```javascript
/* Moralis init code */
const serverUrl = "https://xxxxx/server";
const appId = "YOUR_APP_ID";
Moralis.start({ serverUrl, appId });

/* TODO: Add Moralis Authentication code */
```


### Authentication Demo

Now that the SDK is successfully connected we can use the power of Moralis. Let's login a user and instantly get all their tokens, transactions and NFTs from all chains in your Moralis Database.

#### **`main.js`**
```javascript
/* Moralis init code */
const serverUrl = "https://xxxxx/server";
const appId = "YOUR_APP_ID";
Moralis.start({ serverUrl, appId });

/* Authentication code */
async function login() {
  let user = Moralis.User.current();
  if (!user) {
    user = await Moralis.authenticate({ signingMessage: "Log in using Moralis" })
      .then(function (user) {
        console.log("logged in user:", user);
        console.log(user.get("ethAddress"));
      })
      .catch(function (error) {
        console.log(error);
      });
  }
}

async function logOut() {
  await Moralis.User.logOut();
  console.log("logged out");
}

document.getElementById("btn-login").onclick = login;
document.getElementById("btn-logout").onclick = logOut;
```

### View the page from localhost

Run `index.html` on `localhost` as a web page. The easiest way is by using the [live server extension](https://marketplace.visualstudio.com/items?itemName=ritwickdey.LiveServer)  Visual Studio Code. Just right click on `index.html` and select `Open with Live Server`.

![Serve your page on localhost](<../../.gitbook/assets/Screenshot 2021-10-15 at 17.42.14.png>)

### Login with Metamask

Visit the webpage and click `Login`. Your Metamask will popup and ask you to sign in.

![Metamask popping up when user clicks Login.](<../../.gitbook/assets/Screenshot 2021-10-15 at 17.54.03.png>)

### See all User Assets in the Moralis Database

As soon as the user logs in Moralis fetches all the on-chain data about that user from all chains and puts it into the Moralis Database. To see the Moralis Database go your server and click on _Dashboard_.

![Click on Dashboard in order to see the database of your server.](<../../.gitbook/assets/Screenshot 2021-10-15 at 18.38.52.png>)

You will see the database of that server once you click _Dashboard_. Moralis fetches data from all blockchain where the address of the user has been active and you can see and query all tokens, NFTs and past transactions of the user all in one database.

![Moralis Database fetches all user data from all chains and updates it in real time in case users move their assets on chain.](<../../.gitbook/assets/Screenshot 2021-10-15 at 18.44.04 (1).png>)

### Move Assets

Try moving the assets in your Metamask Wallet and observe how the Moralis Database will update the records in real time.

### Tip of the iceberg

As you can probably already see Moralis is true superpower for blockchain developers. But this small demo is just the tip of the iceberg. Moralis provides endless tools and features for any blockchain use-case. Most importantly, every thing is cross-chain by default.



Feel free to explore the rest of the documentation in order to grasp the full power of Moralis.

## Connecting via NPM

### Install Moralis NPM Package

For larger projects use the [npm module](https://www.npmjs.com/moralis).

```
npm install moralis
```

### Browser

Then include it as usual.

```javascript
const Moralis = require('moralis');
```

### NodeJs

For server-side applications or NodeJs command-line tools, include:

```javascript
// In a node environment
const Moralis  = require('moralis/node');
```



###

###
