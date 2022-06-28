---
description: >-
  Once you have your Moralis Server launched it's time to connect to it via the
  Moralis SDK. This guide will show you how you can do it in just a few easy
  steps.
---

# ⚛ Connect with React

### 1. Creating React App

To start a new Create React App project with TypeScript, you can run:

{% tabs %}
{% tab title="npx" %}
```
npx create-react-app my-app --template typescript
```
{% endtab %}

{% tab title="yarn" %}
```bash
yarn create react-app my-app --template typescript
```
{% endtab %}
{% endtabs %}

### 2. Install the SDK

Make sure to have **react**, **react-dom** and **moralis** installed as dependencies. Then install **react-moralis**:

{% tabs %}
{% tab title="npm" %}
```
npm install moralis react-moralis
```
{% endtab %}

{% tab title="yarn" %}
```
yarn add moralis react-moralis
```
{% endtab %}
{% endtabs %}

### 3. Initialize the SDK

You will see the following code:

{% code title="src/index.tsx" %}
```javascript
import React from "react";
import ReactDOM from "react-dom";
import "./index.css";
import App from "./App";
import reportWebVitals from "./reportWebVitals";

ReactDOM.render(
  <React.StrictMode>
    <App />
  </React.StrictMode>,
  document.getElementById("root")
);
```
{% endcode %}

Import Moralis Provider in your project and add **`<MoralisProvider>`** component as shown below

{% code title="src/index.tsx" %}
```javascript
import React from "react";
import ReactDOM from "react-dom";
import "./index.css";
import App from "./App";
import reportWebVitals from "./reportWebVitals";
import { MoralisProvider } from "react-moralis";

ReactDOM.render(
  <React.StrictMode>
    <MoralisProvider serverUrl="https://xxxxx/server" appId="YOUR_APP_ID">
      <App />
    </MoralisProvider>
  </React.StrictMode>,
  document.getElementById("root")
);
```
{% endcode %}

_Server (Dapp) URL_ and _APP ID_ you can get from your Moralis Dashboard. Go to your Moralis Dashboard and click on _View Details_ next to the server name of your server.

![Click on Settings below the server name of your server.](<../../.gitbook/assets/Server-dashboard.png>)

![Dapp Details](<../../.gitbook/assets/Server-credentials.png>)

### 4. Authenticate User

Now that the SDK is successfully connected we can use the power of Moralis. Let's login a user and instantly get all their tokens, transactions and NFTs from all chains in your Moralis Database.

Call the **`useMoralis`** hooks inside your app in `App.tsx` enter the below code:

{% code title="src/App.tsx" %}
```javascript
import React, { useEffect } from 'react';
import logo from './logo.svg';
import './App.css';
import { useMoralis } from "react-moralis";

function App() {

    const { authenticate, isAuthenticated, isAuthenticating, user, account, logout } = useMoralis();

    useEffect(() => {
    if (isAuthenticated) {
      // add your logic here
    }
    // eslint-disable-next-line react-hooks/exhaustive-deps
  }, [isAuthenticated]);

    const login = async () => {
      if (!isAuthenticated) {

        await authenticate({signingMessage: "Log in using Moralis" })
          .then(function (user) {
            console.log("logged in user:", user);
            console.log(user!.get("ethAddress"));
          })
          .catch(function (error) {
            console.log(error);
          });
      }
    }

    const logOut = async () => {
      await logout();
      console.log("logged out");
    }

  return (
    <div>
      <h1>Moralis Hello World!</h1>
      <button onClick={login}>Moralis Metamask Login</button>
      <button onClick={logOut} disabled={isAuthenticating}>Logout</button>
    </div>
  );
}

export default App;
```
{% endcode %}

### 5. View the page from localhost

Run your app on `localhost` with the following command in your project directory where `package.json` is located

{% tabs %}
{% tab title="npm" %}
```bash
npm start
```
{% endtab %}

{% tab title="yarn" %}
```bash
yarn start
```
{% endtab %}
{% endtabs %}

### 6. Login with Metamask

Visit the webpage and click `Login`. Your Metamask will popup and ask you to sign in.

{% hint style="success" %}
To connect other wallets other than MetaMask, check out: [**Web3 Authentication**](../users/web3-login.md)
{% endhint %}

![Metamask popping up when user clicks Login.](<../../.gitbook/assets/Screenshot 2022-03-16 at 12.46.56 PM.png>)

### 7. See all User Assets in the Moralis Database

As soon as the user logs in Moralis fetches all the on-chain data about that user from all chains and puts it into the [Moralis Database](../database/). To see the Moralis Database go to your server and click on _Dashboard_.

<p align="center">
  <img src="../../.gitbook/assets/Database-access.png">
</p>

<p align="center">
  <img src="../../.gitbook/assets/Database-access-2.png">
</p>


You will see the database of that server once you click _Dashboard_. Moralis fetches data from all blockchains where the address of the user has been active and you can see and query all tokens, NFTs and past transactions of the user all in one database.

![Moralis Database fetches all user data from all chains and updates it in real time in case users move their assets on chain.](<../../.gitbook/assets/Database-access-3.png>)

### Working code

{% embed url="https://codesandbox.io/embed/cool-ardinghelli-4qvpk3?fontsize=14&hidenavigation=1&theme=dark" %}
Live Demo
{% endembed %}

### Move Assets

Try moving the assets in your Metamask Wallet and observe how the Moralis Database will update the records in real-time.

## Tip of the iceberg

As you can probably already see Moralis is a true superpower for blockchain developers. But this small demo is just the tip of the iceberg. Moralis provides endless tools and features for any blockchain use-case. Most importantly, <mark style="color:green;">**everything is cross-chain by default**</mark>.

Feel free to explore the rest of the documentation in order to grasp the full power of Moralis.

{% hint style="success" %}
Check out [**React Boilerplate**](boilerplate-projects.md#web3-react-boilerplate) for instant setup and all features built-in! :rocket:
{% endhint %}
