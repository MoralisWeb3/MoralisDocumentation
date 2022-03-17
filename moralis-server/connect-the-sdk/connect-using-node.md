---
description: >-
  Once you have your Moralis Server launched it's time to connect to it via the
  Moralis SDK. This guide will show you how you can do it in just a few easy
  steps.
---

## Adding Moralis to Node.js

### Difference between Node.js and Front-end

#### Authentication

Authentication in node.js is performed via seed i.e. private key and not with browser based wallets such as metamask and walletconnect because wallets are on browser side and node.js runs on server side. For example follow the tutorial mentioned below.


#### API usage

With node.js you can call web3/Solana API directly without making request to the Moralis server since there is a rate limit to the number of request you can make from front-end.

You need to initialize Moralis SDK with the following syntax in node.js:

```javascript
  /* Moralis init code */
const serverUrl = "YOUR-SERVER-URL";
const appId = "YOUR-APP-ID";
const moralisSecret = "YOUR MORALIS SECRET";

await Moralis.start({ serverUrl, appId, moralisSecret });

```
with `moralisSecret` all API calls go directly to the API instead of passing through the Moralis Server.

Note: While making request for web3/Solana API from front-end you can set rate limits for users, check [here](https://docs.moralis.io/moralis-server/web3-sdk/rate-limit) for more info

To get `moralisSecret` you need to go to account settings as shown in image below

![](<images/moralisSecret1.png>)

then API and copy your `moralisSecret` key

![](<images/moralisSecret2.png>)


#### Node.js version

Check your node.js version, Open up a terminal (Mac/Linux) or a command prompt (Windows) and type the following command to check version:


```
node --version
```

Node.js version should be greater than 16

If you get an error, or the version of Node.js you have is less than version 14, you’ll need to install Node.js. On Mac or Linux, I recommend you first install nvm and use nvm to install Node.js. For more information regarding nodejs installation you can refer [here](https://nodejs.dev/learn/how-to-install-nodejs)


### Create Your Node.js project

After ensuring you have a recent version of Node.js installed, create a folder for your project.

```
mkdir moralis-app
cd moralis-app
```

Use `npm` to initialize a `package.json` file.

```
npm init -y
```

In this node application, [Express](https://expressjs.com/) is used to serve web pages and implement an API. Dependencies are installed using npm. Add Express to your project with the following command.

```
npm install express@4
```

### Add Typescript to your project

The first step is to add the TypeScript compiler. You can install the compiler as a developer dependency using the `--save-dev` flag.


```
npm install --save-dev typescript@4
```

The next step is to add a `tsconfig.json` file. This file instructs TypeScript how to compile (transpile) your TypeScript code into plain JavaScript.

```javascript
{
    "compilerOptions": {
        "module": "commonjs",
        "esModuleInterop": true,
        "target": "es6",
        "noImplicitAny": true,
        "moduleResolution": "node",
        "sourceMap": true,
        "outDir": "dist",
        "baseUrl": ".",
        "paths": {
            "*": [
                "node_modules/*"
            ]
        }
    },
    "include": [
        "src/**/*"
    ]
}
```

Based on this `tsconfig.json` file, the TypeScript compiler will (attempt to) compile any files ending with .ts it finds and store the results in a folder named `dist`. Node.js uses the CommonJS module system, so the value for the `module` setting is `commonjs`. Also, the target version of JavaScript is ES6 (ES2015), which is compatible with modern versions of Node.js.

It’s also a great idea to add `tslint` and create a `tslint.json` file that instructs TypeScript how to lint your code. If you’re not familiar with linting, it is a code analysis tool to alert you to potential problems in your code beyond syntax issues.

Install `tslint` as a developer dependency.

```
npm install --save-dev tslint

```

Next, create a new file in the root folder named `tslint.json` file and add the following configuration.

```javascript
{
    "defaultSeverity": "error",
    "extends": [
        "tslint:recommended"
    ],
    "jsRules": {},
    "rules": {
        "trailing-comma": [ false ],
        "no-console": false
    },
    "rulesDirectory": []
}
```

Next, update your `package.json` to change `main` to point to the new `dist` folder created by the TypeScript compiler. Also, add a couple of scripts to execute TSLint and the TypeScript compiler just before starting the Node.js server.

```
  "main": "dist/index.js",
  "scripts": {
    "prebuild": "tslint -c tslint.json -p tsconfig.json --fix",
    "build": "tsc",
    "prestart": "npm run build",
    "start": "node .",
    "test": "echo \"Error: no test specified\" && exit 1"
  }
```  

### Installing Moralis SDK

Run the following command to install Moralis SDK

```
npm install moralis
```

### Authentication Demo

Create a folder named src, create a file named `auth.ts` in your `src` folder and add the following code:

```javascript
import Moralis from "moralis/node";

const Auth = async () => {

    /* Moralis init code */
    const serverUrl = "YOUR-SERVER-URL";
    const appId = "YOUR-APP-ID";
    const masterKey = "YOUR-MASTER-KEY";
    const moralisSecret = "YOUR MORALIS SECRET";

    await Moralis.start({ serverUrl, appId, masterKey, moralisSecret });

    const web3Provider = await Moralis.enableWeb3({ privateKey: "YOUR-PRIVATE-KEY" });
    console.log(web3Provider);

}

export default Auth;
```

Create a file named `index.ts` in your `src` folder and add the following code:


```javascript
import express from "express";
import path from "path";

const app = express();
const port = 8080; // default port to listen

// define a route handler for the default home page
app.get( "/", ( req, res ) => {
    Auth();
});

```


Then run the following command in your terminal

```
npm run start
```

You will see the server running at port `8080`

![](<images/localhost.png>)

Go to your favorite browser and enter the following url:

`http://localhost:8080/`

Go back to your terminal you will see the following result:

![](<images/result2.png>)


Note: With `Moralis.enableWeb3` you get access to only `provider` functions in ethersjs library and not to all `Providers` functions


#### API usage


Create a file named `web3api.ts` in your `src` folder and add the following code:

#### **`web3api.ts`**
```javascript

import Moralis from "moralis/node";

const web3API = async () => {

/* Moralis init code */
const serverUrl = "YOUR-SERVER-URL";
const appId = "YOUR-APP-ID";
const moralisSecret = "YOUR MORALIS SECRET";

await Moralis.start({ serverUrl, appId, moralisSecret });

    //calling `getTokenPrice({address:"tokenAddress", chain:"chainID"})` from web3API
    const price = await Moralis.Web3API.token.getTokenPrice(
    {address: "0xe9e7cea3dedca5984780bafc599bd69add087d56", chain: "bsc"})
    console.log(price);
}

export default web3API;
```

In `index.ts` do the following changes:

```javascript
import express from "express";
import path from "path";

const app = express();
const port = 8080; // default port to listen

// define a route handler for the default home page
app.get( "/", ( req, res ) => {
    web3API();
});

```

Then run the following command in your terminal

```
npm run start
```

You will see the server running at port `8080`

![](<images/localhost.png>)

Go to your favorite browser and enter the following url:

`http://localhost:8080/`

Go back to your terminal you will see the following result:

![](<images/result1.png>)

###

###
