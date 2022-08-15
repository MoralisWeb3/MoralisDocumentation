---
description: Authenticate Solana users with Phantom Wallet.
---

# ✨ Phantom (Solana)

## Integrate Moralis with Phantom Wallet&#x20;

If your user has [Phantom Wallet](https://phantom.app) installed you can use the following code in order to log them into your Moralis dapp.

{% tabs %}
{% tab title="JS" %}
```javascript
Moralis.authenticate({type: "sol"})
    .then(function (user) {
        console.log("logged in user:", user);
    })
    .catch(function (error) {
        console.log(error);
    });
```
{% endtab %}
{% tab title="React" %}
```javascript
import React, {useEffect } from "react";
import { useMoralis } from "react-moralis";

function App(){
    const { authenticate, isAuthenticated, isAuthenticating, user, account, logout } = useMoralis();

    useEffect(() => {
        if (isAuthenticated) {
            // add your logic here
        }
        // eslint-disable-next-line react-hooks/exhaustive-deps
    }, [isAuthenticated]);

    const login = async () => {
        if (!isAuthenticated) {
            await authenticate({signingMessage: "Log in using Moralis", type: "sol"})
                .then(function (user) {
                    console.log("logged in user:", user);
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
            <button onClick={login}>
                {isAuthenticated ? "Logged In" : "Moralis Phantom-Wallet Login"}
            </button>
            <button onClick={logOut} disabled={isAuthenticating}>Logout</button>
        </div>
    );
}

export default App;
```
{% endtab %}
{% endtabs %}

{% hint style="warning" %}
It's important to mention that Solana integration is still in development and Solana users won't get their transactions synced into the [Moralis database](../../database/).
{% endhint %}

