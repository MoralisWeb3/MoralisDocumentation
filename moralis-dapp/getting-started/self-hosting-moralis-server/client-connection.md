---
title: "Client connection"
slug: "client-connection"
sidebar_position: 5
---

{% hint style="info" %}

### Overview

This guide will teach you how to **connect to** your **self-hosted Moralis Server** from different **client environments**. We will **help you set up those**.

{% endhint %}

{% hint style="warning" %}

### Important

The completion of the [**Local Environment Setup**](https://v1docs.moralis.io/moralis-dapp/getting-started/self-hosting-moralis-server/local-environment-setup) is required to continue.
The completion of the [**Production Environment Setup**](https://v1docs.moralis.io/moralis-dapp/getting-started/self-hosting-moralis-server/production-environment-setup) is **NOT required** but it is **strongly recommended**.

{% endhint %}

## React App

### Get the sample project

{% hint style="info" %}

To speed up the connection process, we have the [`parse-server-migration-react-client`](https://github.com/MoralisWeb3/Moralis-JS-SDK/tree/main/demos/parse-server-migration-react-client) project ready for you.

{% endhint %}

{% hint style="info" %}

This project uses [`react-moralis`](https://github.com/MoralisWeb3/react-moralis), which is a thin wrapper on [`Moralis-JS-SDK-v1`](https://github.com/MoralisWeb3/Moralis-JS-SDK-v1).

{% endhint %}

[**Download**](https://moralisweb3.github.io/Moralis-JS-SDK/downloads/parse-server-migration-react-client.zip) it and open it with your code editor:

![](/img/content/client-1.webp)

### Installation

Open the terminal and **install dependencies** by running:

```shell
yarn install
```

### Configuration

Run the following command to get the `.env` file:

```shell
cp .env.example .env
```

![](/img/content/client-2.webp)

Set the `REACT_APP_SERVER_URL` to your **`SERVER_URL`**:

```shell
REACT_APP_SERVER_URL = 'http://localhost:1337/server'
```

{% hint style="info" %}

Your `SERVER_URL` will be different if you're already [running your Moralis Server in a hosting service](/web3-data-api/self-hosting-moralis-server/deployment).

{% endhint %}

Set the `REACT_APP_APPLICATION_ID` to your **`APPLICATION_ID`**:

```shell
REACT_APP_APPLICATION_ID = 001
```

### Testing

{% hint style="info" %}

Make sure your **self-hosted Moralis Server** is running, **locally** or in a **hosting service**.

{% endhint %}

Run the client locally:

```shell
yarn start
```

Now you can try to **_Authenticate_** and for example **_getBlock_** data:

![](/img/content/client-3.webp)

{% hint style="success" %}

You have connected your **self-hosted Moralis Server** with a **React App**.

{% endhint %}

### Note about `Authentication`

{% hint style="info" %}

The following information serves as a _good-to-know_. The [`parse-server-migration-react-client`](https://github.com/MoralisWeb3/Moralis-JS-SDK/tree/main/demos/parse-server-migration-react-client) project already implements it.

{% endhint %}

The **authentication flow differs slightly** between the **self-hosted Moralis Server** and the **Moralis-hosted Server**. This is because the first is using the [**Auth Api**](https://docs.moralis.io/reference/requestchallengeevm).

The following **code snippets** show how to handle it on a **`react-moralis` app** and how you would do it with **`Moralis-JS-SDK-v1`** alone.

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

<Tabs>
  <TabItem value="react" label="react-moralis">

```javascript React
import { useMoralis } from "react-moralis";
import Moralis from "moralis-v1";

export const Example = () => {
  const { authenticate, enableWeb3 } = useMoralis();
  const [authError, setAuthError] = useState(null);
  const [isAuthenticating, setIsAuthenticating] = useState(false);

  const handleAuth = async (provider) => {
    try {
      setAuthError(null);
      setIsAuthenticating(true);

      // Enable web3 to get user address and chain
      await enableWeb3({ throwOnError: true, provider });
      const { account, chainId } = Moralis;

      if (!account) {
        throw new Error(
          "Connecting to chain failed, as no connected account was found"
        );
      }
      if (!chainId) {
        throw new Error(
          "Connecting to chain failed, as no connected chain was found"
        );
      }

      // Get message to sign from the auth api
      const { message } = await Moralis.Cloud.run("requestMessage", {
        address: account,
        chain: parseInt(chainId, 16),
        network: "evm",
      });

      // Authenticate and login via parse
      await authenticate({
        signingMessage: message,
        throwOnError: true,
      });
    } catch (error) {
      setAuthError(error);
    } finally {
      setIsAuthenticating(false);
    }
  };

  return (
    <div>
      <button onClick={() => handleAuth("metamask")}>
        Authenticate via metamask
      </button>
    </div>
  );
};
```

  </TabItem>
  <TabItem value="javascript" label="Moralis-JS-SDK-v1" default>

```javascript
async function handleAuth(provider) {
  await Moralis.enableWeb3({
    throwOnError: true,
    provider,
  });

  const { account, chainId } = Moralis;

  if (!account) {
    throw new Error(
      "Connecting to chain failed, as no connected account was found"
    );
  }
  if (!chainId) {
    throw new Error(
      "Connecting to chain failed, as no connected chain was found"
    );
  }

  const { message } = await Moralis.Cloud.run("requestMessage", {
    address: account,
    chain: parseInt(chainId, 16),
    network: "evm",
  });

  await Moralis.authenticate({
    signingMessage: message,
    throwOnError: true,
  }).then((user) => {
    console.log(user);
  });
}

// Example
handleAuth("metamask");
```

  </TabItem>
</Tabs>

## Unity client

{% hint style="info" %}

### Overview

We will use the last release of the [**`unity-web3-game-kit`**](https://github.com/MoralisWeb3/unity-web3-game-kit) which is a wrapper around [`web3-unity-sdk`](https://github.com/MoralisWeb3/web3-unity-sdk).

{% endhint %}

### Installation

[Download the `.unitypackage`](https://github.com/MoralisWeb3/unity-web3-game-kit/releases/download/v1.2.11/moralisweb3sdk_v1_2_11.unitypackage) and drag it into your Unity project:

![](/img/content/unity-1.webp)

Import all the files:

![](/img/content/unity-2.webp)

The **_Moralis Web3 Setup_** panel will appear. Enter your `SERVER_URL` and your `APPLICATION_ID` and choose **_Done_**:

![](/img/content/unity-3.webp)

{% hint style="info" %}

Your `SERVER_URL` will be different if you're already [running your Moralis Server in a hosting service](/web3-data-api/self-hosting-moralis-server/deployment).

{% endhint %}

### Configuration

{% hint style="info" %}

### Overview

To use the **`unity-web3-game-kit`** with your **self-hosted Moralis Server** you need to **apply some modifications** to the package.

This section goes through **all the steps** but if you need help you can head over to the [**related forum thread**](https://forum.moralis.io/t/using-unity-sdk-with-self-hosted-server/20527).

{% endhint %}

Open `Packages > Moralis Web3 Unity SDK > Runtime > Kits > AuthenticationKit > Scripts > AuthenticationKit.cs`:

![](/img/content/unity-4.webp)

Replace the code block from **lines [`257`](https://github.com/MoralisWeb3/web3-unity-sdk/blob/fe522579772bb8eaeabd412710f764cb8bfbf4a7/Runtime/Kits/AuthenticationKit/Scripts/AuthenticationKit.cs#L257) to [`263`](https://github.com/MoralisWeb3/web3-unity-sdk/blob/fe522579772bb8eaeabd412710f764cb8bfbf4a7/Runtime/Kits/AuthenticationKit/Scripts/AuthenticationKit.cs#L263)** (both included) for the following:

```csharp
if (serverTimeResponse != null)
{
    Debug.LogError("Failed to retrieve server time from Moralis Server!");
}

IDictionary<string, object> requestMessageParams = new Dictionary<string, object>();

requestMessageParams.Add("address", address);
requestMessageParams.Add("chain", Web3GL.ChainId());

Dictionary<string, object> authMessage = await Moralis.Cloud.RunAsync<Dictionary<string, object>>("requestMessage", requestMessageParams);

string signMessage = authMessage["message"].ToString();
```

Replace the code block from **lines [`361`](https://github.com/MoralisWeb3/web3-unity-sdk/blob/fe522579772bb8eaeabd412710f764cb8bfbf4a7/Runtime/Kits/AuthenticationKit/Scripts/AuthenticationKit.cs#L361) to [`367`](https://github.com/MoralisWeb3/web3-unity-sdk/blob/fe522579772bb8eaeabd412710f764cb8bfbf4a7/Runtime/Kits/AuthenticationKit/Scripts/AuthenticationKit.cs#L367)** (both included) for the following:

```csharp
if (serverTimeResponse != null)
{
    Debug.LogError("Failed to retrieve server time from Moralis Server!");
}

IDictionary<string, object> requestMessageParams = new Dictionary<string, object>();

requestMessageParams.Add("address", address);
requestMessageParams.Add("chain", session.ChainId);

Dictionary<string, object> authMessage = await Moralis.Cloud.RunAsync<Dictionary<string, object>>("requestMessage", requestMessageParams);

string signMessage = authMessage["message"].ToString();
```

Open `Packages > Moralis Web3 Unity SDK > Runtime > Core > Platform > Services > ClientServices > MoralisUserService.cs` and at **line [`101`](https://github.com/MoralisWeb3/web3-unity-sdk/blob/fe522579772bb8eaeabd412710f764cb8bfbf4a7/Runtime/Core/Platform/Services/ClientServices/MoralisUserService.cs#L101)** add:

```csharp
user.authData.Clear();
```

Open `Moralis Web3 Unity SDK > Runtime > Web3Api > Client > ApiClient.cs` and under existing default headers (**line [`40`](https://github.com/MoralisWeb3/web3-unity-sdk/blob/fe522579772bb8eaeabd412710f764cb8bfbf4a7/Runtime/Web3Api/Client/ApiClient.cs#L40)**) add:

```csharp
_defaultHeaderMap.Add("x-parse-application-id", Moralis.DappId);
```

Instead of this last modification, you could add the following to your server's **`src/index.ts`**:

```js
app.use("/server/functions/:functionName", (req, res, next) => {
  req.headers["x-parse-application-id"] = "[YOUR_APPLICATION_ID]";
  next();
});

app.use(`/server`, parseServer);
```

### Testing

Open `Assets > Moralis Web3 Unity SDK > Demos > Introduction > Introduction.unity` scene and run it. Now choose **_Connect_**:

![](/img/content/unity-5.webp)

After signing the **authentication message** you should see the following screen:

![](/img/content/unity-6.webp)

{% hint style="info" %}

This setup is also tested and working on **WebGL**.

{% endhint %}

{% hint style="success" %}

You have connected your **self-hosted Moralis Server** with a **Unity project**.

{% endhint %}
