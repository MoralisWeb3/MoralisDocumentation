---
description: >-
  Once you have your Moralis Server launched it's time to connect to it via the
  Moralis SDK. This guide will show you how you can do it in just a few easy
  steps.
---

## Adding Moralis to Your React App

### Creating React App

To start a new Create React App project with TypeScript, you can run:

```
npx create-react-app my-app --template typescript
```

or

```
yarn create react-app my-app --template typescript
```

### Installing the SDK

Make sure to have react, react-dom and moralis installed as dependencies. Then install react-moralis:

```
npm install moralis react-moralis
```

or

```
yarn add moralis react-moralis
```

### Initialize the SDK

Go to following file in your folder

```
src/index.tsx
```

You will see the following code:

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

Import `Moralis Provider` in your project and add `<MoralisProvider>` component as shown below

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

_Server URL_ and _APP ID_ you can get from your Moralis Dashboard. Go to your Moralis Dashboard and click on _View Details_ next to the server name of your server.

![](<../../.gitbook/assets/Screenshot 2021-10-15 at 17.10.09.png>)

### Authentication Demo

Now that the SDK is successfully connected we can use the power of Moralis. Let's login a user and instantly get all their tokens, transactions and NFTs from all chains in your Moralis Database.

Call the `useMoralis` hooks inside your app in `App.tsx` enter the below code:

#### **`App.tsx`**

```javascript
import React from 'react';
import logo from './logo.svg';
import './App.css';
import { useMoralis } from "react-moralis";

function App() {

    const { authenticate, isAuthenticated, isAuthenticating, user, account, logout } = useMoralis();

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

### View the page from localhost

Run your app on `localhost` with following command in your project directory where `package.json` is located

```
npm run start
```

or

incase you are using `yarn`

```
yarn start
```


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
