---
description: An npm Package with Custom React Hooks for Moralis SDK Features.
---

# React Moralis

This project is a thin React wrapper around Moralis, to easily call SDK functions and display data.

## Quick Start

Make sure to have `react`, `react-dom` and `moralis` installed as dependencies. Then install react-moralis:

```
npm install react-moralis
```

or:

```
yarn add react-moralis
```

Then wrap your app in a `<MoralisProvider>`:

```javascript
import React from "react";
import ReactDOM from "react-dom";
import { MoralisProvider } from "react-moralis";

ReactDOM.render(
  <MoralisProvider appId="xxxxxxxx" serverUrl="xxxxxxxx">
    <App />
  </MoralisProvider>,
  document.getElementById("root"),
);
```

And call the hooks inside your app:

```javascript
import React from "react";
import { useMoralis } from "react-moralis";

function App() {
  const { authenticate, isAuthenticated, user } = useMoralis();

  if (!isAuthenticated) {
    return (
      <div>
        <button onClick={() => authenticate()}>Authenticate</button>
      </div>
    );
  }

  return (
    <div>
      <h1>Welcome {user.get("username")}</h1>
    </div>
  );
}
```

## GitHub

For more info see the extended documentation at the [GitHub repo](https://github.com/MoralisWeb3/react-moralis).

