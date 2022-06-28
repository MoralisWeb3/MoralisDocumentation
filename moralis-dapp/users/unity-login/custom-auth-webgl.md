---
description: Custom code for Authentication on the Moralis Unity SDK
---

## Custom Authentication

{% embed url="https://github.com/MoralisWeb3/web3-unity-sdk" %}
Code for the Moralis Unity SDK
{% endembed %}

### WEBGL authentication for unity WEBGL

The platform authentication uses wallet connect and requires scan qr code and confirm sign in message to be logged in

1). Create two empty game objects, attach the [Wallet Connect script](https://github.com/MoralisWeb3/web3-unity-sdk/blob/main/Runtime/External/WalletConnect/WalletConnectSharp.Unity/WalletConnect.cs) to one and the [Wallet Connect QR image script](https://github.com/MoralisWeb3/web3-unity-sdk/blob/main/Runtime/External/WalletConnect/WalletConnectSharp.Unity/UI/WalletConnectQRImage.cs) to the other, These scripts are already made and can be attached from the inspector.  
2). Reference the gameObject with the Wallet connect script in the Wallet Connect QR image script variable slot.
{% hint style="info" %}
This will generate a QR code on start to be scanned, you can hook it up to a button to activate it when needed.
{% endhint %}
{% hint style="info" %}
When the QR code is scanned, the Wallet Connect script has various events to trigger sign message (which is need to be authenticated in your Dapp), These events can be used to handle different wallet connect states in your program
{% endhint %}
3). When the QR code is scanned, A function will be hooked to the `Connected Event Session (WCSessionData)` event that will require to sign a message and log us into our moralis dapp.

- Create a new script and attach it to a game object, copy and paste the following code below

<details open>
<summary>Sign moralis authentication message</summary>
The code below is for referencing the sign message function in the inspector (not through code), this can be done, <a target = "_blank" href = "https://docs.unity3d.com/ScriptReference/Events.UnityEvent.html">Learn more about accessing events through code</a>

```csharp
    public WalletConnect _walletConnect;
    public void Start()
    {
        Moralis.Start();
    }
    public async void WalletConnect_OnConnectedEventSession(WCSessionData wcSessionData)
    {
        //Debug.Log($"WalletConnect_OnConnectedEventSession() wcSessionData = {wcSessionData}");
        // WalletConnect can resume a session and trigger this event on start
        // So double check if we already go a user and are connected
        if (await Moralis.GetUserAsync() != null)
        {

            return;
        }
        // Extract wallet address from the Wallet Connect Session data object.
        string address = wcSessionData.accounts[0].ToLower();
        string appId = Moralis.DappId;
        long serverTime = 0;

        // Retrieve server time from Moralis Server for message signature
        Dictionary<string, object> serverTimeResponse = await Moralis.Cloud
            .RunAsync<Dictionary<string, object>>("getServerTime", new Dictionary<string, object>());

        if (serverTimeResponse == null || !serverTimeResponse.ContainsKey("dateTime") ||
            !long.TryParse(serverTimeResponse["dateTime"].ToString(), out serverTime))
        {
            Debug.LogError("Failed to retrieve server time from Moralis Server!");
        }

        string signMessage = $"Moralis Authentication\n\nId: {appId}:{serverTime}";
        string signature = null;
        // Try to sign and catch the Exception when a user cancels the request
        try
        {
            signature = await _walletConnect.Session.EthPersonalSign(address, signMessage);
        }
        catch
        {
            // Disconnect and start over if a user cancels the singing request or there is an error
          //  Disconnect();
            return;
        }
        // Create Moralis auth data from message signing response.
        Dictionary<string, object> authData = new Dictionary<string, object>
            {
                { "id", address }, { "signature", signature }, { "data", signMessage }
            };

        // Attempt to login user.
        MoralisUser user = await Moralis.LogInAsync(authData, wcSessionData.chainId.Value);

        if (user != null)
        {
            Debug.Log("connected");
        }
    }
    public async void disconnect()
    {
        // Disconnect wallet subscription.
        await _walletConnect.Session.Disconnect();
        // CLear out the session so it is re-establish on sign-in.
        _walletConnect.CLearSession();
        // Logout the Moralis User.
        await Moralis.LogOutAsync();
    }
```

The function `WalletConnect_OnConnectedEventSession(WCSessionData wcSessionData)` will be hooked up in the inspector in your Wallet Connect script on the `Connected Event Session (WCSessionData)` event.

functions can be created to be hooked up to the other different Events on the Wallect Connect script.

</details>
4). Save and Press play
5). If things are setup correctly, The QR code should pop up and be scanned followed my a sign request and when Logged in should print `connected` to the console.

### Code breakdown

#### Wallet Connect reference

```csharp
 public WalletConnect _walletConnect;
```

This should create a public reference for the gameobject attached to the wallet Connect script to be dragged and dropped in the inspector.

#### Start Function

```csharp
public void Start()
    {
        Moralis.Start();
    }
```

This should initailize/start Moralis apis and services when the program starts, not including this will return the error `MoralisFailureException: Moralis must be started before accessing this object`

#### Event Function

```csharp
 if (await Moralis.GetUserAsync() != null){return;}
```

This is to check if there is a logged in user and stop executing this function when there is a logged in user (when `user != null`).

---

```csharp
    string address = wcSessionData.accounts[0].ToLower();
    string appId = Moralis.DappId;
    long serverTime = 0;
```

This is to get the address of the logged in user from the qrcode sign in, get the DappId from the configs and reset server time.

---

```csharp
    // Retrieve server time from Moralis Server for message signature
    Dictionary<string, object> serverTimeResponse = await Moralis.Cloud.RunAsync<Dictionary<string, object>>("getServerTime", new Dictionary<string, object>());
    if (serverTimeResponse == null || !serverTimeResponse.ContainsKey("dateTime") ||
            !long.TryParse(serverTimeResponse["dateTime"].ToString(), out serverTime))
    {
            Debug.LogError("Failed to retrieve server time from Moralis Server!");
    }

    string signMessage = $"Moralis Authentication\n\nId: {appId}:{serverTime}";
    string signature = null;
```

This gets the server time from moralis cloud, create a signMessage and set the signature to null (empty)

---

```csharp
 // Try to sign and catch the Exception when a user cancels the request
        try
        {
            signature = await _walletConnect.Session.EthPersonalSign(address, signMessage);
        }
        catch
        {
            // Disconnect and start over if a user cancels the singing request or there is an error
          //  Disconnect();
            return;
        }
```

This sends the sign message to your wallet to confirm and sign, it also catches the error

---

```csharp
        // Create Moralis auth data from message signing response.
        Dictionary<string, object> authData = new Dictionary<string, object>
            {
                { "id", address }, { "signature", signature }, { "data", signMessage }
            };
        // Attempt to login user.
        MoralisUser user = await Moralis.LogInAsync(authData, wcSessionData.chainId.Value);

        if (user != null)
        {
            Debug.Log("connected");
        }
```

This creates a Dictionary `authData` consisitng of the address, signature and signMessage, Logs them in with the `authData` and the chainId.
Print "connected" to unity console if succesfully logged in.

#### Disconnect function

```csharp
    public async void disconnect()
    {
        // Disconnect wallet subscription.
        await _walletConnect.Session.Disconnect();
        // CLear out the session so it is re-establish on sign-in.
        _walletConnect.CLearSession();
        // Logout the Moralis User.
        await Moralis.LogOutAsync();
    }
```

This function disconnect the wallet connect session, clears it and log the user out of your dapp.

{% hint style="info" %}
The process above is a basic implementation of custom authentication (writing your authentication code), it can be expanded upon.
{% endhint %}
