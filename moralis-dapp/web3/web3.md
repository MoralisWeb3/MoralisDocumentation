---
description: Web3 Specific Functionality of the Moralis SDK.
---

# Web3 Provider

Let's talk about Web3 integration!

Moralis comes with a set of plugins to help you focus on the development of your app, leaving the rest to us.

Similar to `window.ethereum.enable()` but returns a promise that resolves to a `web3Provider` instance from Ethers.js. Use this when you need a fully functional `web3Provider` instance, such as for making contract calls.

> ❗️ Note: make sure to have v1.0.0 or higher of the SDK installed. Otherwise Moralis.enableWeb3() will return an instance of web3.js instead of ethers.js

_Example_

```javascript
// Get a (ethers.js) web3Provider
const web3Provider = await Moralis.enableWeb3();
```

### Web3.js

You might want to use another library, like [Web3.js](https://web3js.readthedocs.io). To do so, you need to include this library to your code.

_When you import javascript scripts via html:_

```html
<script src="https://cdn.jsdelivr.net/npm/web3@latest/dist/web3.min.js"></script>
```

_Or when you use a package manager:_

```
npm install web3
```

Then in your code, use `Moralis.provider` to initialize the web3.js library. `Moralis.provider` will only be set after calling Moralis.authenticate or Moralis.enableWeb3.

```javascript
import Web3 from "web3"; // Only when using npm/yarn

// Enable web3 and get the initialized web3 instance from Web3.js
await Moralis.enableWeb3();
const web3Js = new Web3(Moralis.provider);
```

See for more info on how to use web3.js, in the [Web3.js docs](https://web3js.readthedocs.io)

> Note: Ethers.js is included in the Moralis SDK. So The function responses of Moralis.executeFunction and Moralis.transfer will always be formatted by Ethers.js (see below for more information)

### Get the ethers.js library

You can have access to the ethers.js library, that Moralis is using:

```javascript
const ethers = Moralis.web3Library;
```

With this instance, you can use any methods from ethers.js, for example:

```javascript
const ethers = Moralis.web3Library;

const daiAddress = "dai.tokens.ethers.eth";
const daiAbi = [
  "function name() view returns (string)",
  "function symbol() view returns (string)",
  "function balanceOf(address) view returns (uint)",
  "function transfer(address to, uint amount)",
  "event Transfer(address indexed from, address indexed to, uint amount)",
];
const daiContract = new ethers.Contract(daiAddress, daiAbi, provider);

const name = await daiContract.name();
console.log(name);
// 'Dai Stablecoin'
```

See for more info on how to use ethers.js, in the [ethers.js docs](https://docs.ethers.io)

### Connectors

You enable web3 with any connector (such as WalletConnect, Metamask, Network etc.), the [same way as with Moralis.authenticate](https://docs.moralis.io/moralis-dapp/users/crypto-login#ethereum-bsc-and-polygon-login)), with the `provider` option:

```javascript
const web3 = await Moralis.enableWeb3({ provider: "walletconnect" });
```

### NodeJs Enviroment

When using moralis in a nodejs environment, you can optionally use the `privateKey` option to sign transactions:

```javascript
const web3 = await Moralis.enableWeb3({ privateKey: "your_private_key" });
```

### In Cloud Functions

It's also possible to get a Web3 instance inside cloud functions, but the syntax is a bit different. See the [Cloud Functions Web3](../cloud-code/cloud-functions.md#web3) page for more info.

This instance is a **Web3.js instance**

## executeFunction

Via `executeFunction`, you can execute read-only (view) functions, and write methods. They both give a slightly different response. So we handle them seperately below.

Options:

* contractAddress (required): A smart contract address
* abi (required): Contract's or function ABI(should be provided as an array)
* functionName (required): A function name
* params (required): Parameters needed for your specific function
* msgValue(optional): Number|String|BN|BigNumber. The value transferred for the transaction in wei.

### Read-only functions

For read-only functions, you will need to wait for the execution to be completed. Then you get the results back directly.

```javascript
const ABI = [
  {
    inputs: [],
    name: "message",
    outputs: [
      {
        internalType: "string",
        name: "",
        type: "string",
      },
    ],
    stateMutability: "view",
    type: "function",
  },
  {
    inputs: [
      {
        internalType: "string",
        name: "_newMessage",
        type: "string",
      },
    ],
    name: "setMessage",
    outputs: [],
    stateMutability: "nonpayable",
    type: "function",
  },
];

const readOptions = {
  contractAddress: "0xe...56",
  functionName: "message",
  abi: ABI,
};

const message = await Moralis.executeFunction(readOptions);
console.log(message);
// -> "Hello World"
```

{% hint style="info" %}
Requires an enabled web3 provider. Before executing the function, make sure that the correct network is selected in your wallet.
{% endhint %}

{% hint style="info" %}
You can receive this information without an active web3 provider using our Web3API: [Moralis.Web3API.token.getAllowance()](https://docs.moralis.io/moralis-dapp/web3-sdk/token#gettokenallowance)
{% endhint %}

### Example of calling a write contract method

An example of calling setting a message via a `setMessage` function.

```javascript
const ABI = [
  {
    inputs: [],
    name: "message",
    outputs: [
      {
        internalType: "string",
        name: "",
        type: "string",
      },
    ],
    stateMutability: "view",
    type: "function",
  },
  {
    inputs: [
      {
        internalType: "string",
        name: "_newMessage",
        type: "string",
      },
    ],
    name: "setMessage",
    outputs: [],
    stateMutability: "nonpayable",
    type: "function",
  },
];

const sendOptions = {
  contractAddress: "0xe...56",
  functionName: "setMessage",
  abi: ABI,
  params: {
    _newMessage: "Hello Moralis",
  },
};

const transaction = await Moralis.executeFunction(sendOptions);
console.log(transaction.hash);
// --> "0x39af55979f5b690fdce14eb23f91dfb0357cb1a27f387656e197636e597b5b7c"

// Wait until the transaction is confirmed
await transaction.wait();

// Read new value
const message = await Moralis.executeFunction(readOptions);
console.log(message);
// --> "Hello Moralis"
```

You will get back a [transaction response from ethers.js](https://docs.ethers.io/v5/api/providers/types/#providers-TransactionResponse), that includes the `hash` and other information.

**Example transaction response**

```javascript
{
    "hash": "0x39af55979f5b690fdce14eb23f91dfb0357cb1a27f387656e197636e597b5b7c",
    "type": 2,
    "accessList": null,
    "blockHash": null,
    "blockNumber": null,
    "transactionIndex": null,
    "confirmations": 0,
    "from": "0xab5801a7d398351b8be11c439e05c5b3259aec9b",
    "gasPrice": {
        "type": "BigNumber",
        "hex": "0x04cf1ef09a"
    },
    "maxPriorityFeePerGas": {
        "type": "BigNumber",
        "hex": "0x908a9040"
    },
    "maxFeePerGas": {
        "type": "BigNumber",
        "hex": "0x04cf1ef09a"
    },
    "gasLimit": {
        "type": "BigNumber",
        "hex": "0x75c0"
    },
    "to": "0x73bceb1cd57c711feac4224d062b0f6ff338501e",
    "value": {
        "type": "BigNumber",
        "hex": "0x00"
    },
    "nonce": 13,
    "data": "0x368b877200000000000000000000000000000000000000000000000000000000000000200000000000000000000000000000000000000000000000000000000000000004486f6c6100000000000000000000000000000000000000000000000000000000",
    "r": "0x8b00442b141406a1b5d701c7ac6ab3b6adec13010fdc1218a42f8f9aa8e57718",
    "s": "0x1abc60ae4de7af8bcba4d1b44e470f40a04c3c632fd4a75f87a553820a43ddef",
    "v": 1,
    "creates": null,
    "chainId": 0
}
```

If you want to wait until the transaction has been confirmed by the chain, then you can call `transaction.wait()`. That will resolve into a [transaction receipt](https://docs.ethers.io/v5/api/providers/types/#providers-TransactionReceipt). This receipt will also include a log of all the events that have been fired in the transaction. For example here we wait for 3 confirmations:

```javascript
const receipt = await transaction.wait(3);
```

**Example receipt**

```javascript
{
    "to": "0x73bceb1cd57c711feac4224d062b0f6ff338501e",
    "from": "0xab5801a7d398351b8be11c439e05c5b3259aec9b",
    "contractAddress": null,
    "transactionIndex": 72,
    "gasUsed": {
        "type": "BigNumber",
        "hex": "0x75b2"
    },
    "logsBloom": "0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000",
    "blockHash": "0x746d7d42e8022f560577dfdc0150e1e8b009e59c537a9053a82e9ad6d20f6d06",
    "transactionHash": "0x39af55979f5b690fdce14eb23f91dfb0357cb1a27f387656e197636e597b5b7c",
    "logs": [],
    "blockNumber": 11847564,
    "confirmations": 2,
    "cumulativeGasUsed": {
        "type": "BigNumber",
        "hex": "0x6743ab"
    },
    "effectiveGasPrice": {
        "type": "BigNumber",
        "hex": "0x04623af60c"
    },
    "status": 1,
    "type": 2,
    "byzantium": true,
    "events": []
}
```

### Setting custom gas

Currently to set gas parameters, you will need to use ethers.js or web3.js directly. The following example uses the ethers.js library that comes with Moralis.

```javascript
const ethers = Moralis.web3Library; // get ethers.js library
const web3Provider = await Moralis.enableWeb3(); // Get ethers.js web3Provider
const gasPrice = await web3Provider.getGasPrice();

const signer = web3Provider.getSigner();

// wMATIC token on Mumbai
const contract = new ethers.Contract('0x9c3C9283D3e44854697Cd22D3Faa240Cfb032889', ABI, signer);

const transaction = await contract.deposit({
  value: Moralis.Units.ETH('1'),
  gasLimit: 100000000,
  gasPrice: gasPrice,
});
await transaction.wait();
```

### Transfering native crypto in contract method example

For example, you want to call a smart contract method that exchanges native cryptocurrency for tokens. This case is different from the usual situations where you use ERC20 tokens and specify the number of tokens to use as a method parameter. Action with a native cryptocurrency in a smart contract assumes that you will transfer some value along with the transaction. In solidity you can know it as a `msg.value`

```javascript
const options = {
  contractAddress: "0xe...56",
  functionName: "swapNativeForTokens",
  abi: ABI,
  msgValue: Moralis.Units.ETH("0.1"),
};

const transaction = await Moralis.executeFunction(options);
const receipt = await transaction.wait();
console.log(receipt);
```

### Example of creating reusable options

You can use this construction to create reusable options for calling multiple contract methods.

```javascript
const options = {
  contractAddress: "0xe...56",
  abi: ABI,
};

const symbol = await Moralis.executeFunction({
  functionName: "symbol",
  ...options,
});
const decimals = await Moralis.executeFunction({
  functionName: "decimals",
  ...options,
});
const name = await Moralis.executeFunction({
  functionName: "name",
  ...options,
});
```

### Resolve transaction result

`Moralis.executeFunction()` returns:

* The return value if the function is read-only
* A [transaction response](https://docs.ethers.io/v5/api/providers/types/#providers-TransactionResponse), if the function writes on-chain.

You can resolve the transaction response into a receipt, by waiting until the transaction has been confirmed. For example, to wait for 3 confirmations:

```javascript
const options = {
  contractAddress: "0xe...56",
  functionName: "swapNativeForTokens",
  abi: ABI,
  msgValue: Moralis.Units.ETH("0.1"),
};
const transaction = await Moralis.executeFunction(options);
const result = await transaction.wait();
```

## Events

There are several events, that you can listen to on Moralis, in order to check the web3 connection of the user:

* onWeb3Enabled
* onWeb3Deactivated
* onChainChanged
* onAccountChanged
* onConnect (fired from the EIP1193 provider)
* onDisconnect (fired from the EIP1193 provider)

### Moralis.onWeb3Enabled()

Listen to events when a user connects to a chain (via `Moralis.authenticate()` / `Moralis.enableWeb3()`):

```javascript
// Subscribe to onWeb3Enabled events
const unsubscribe = Moralis.onWeb3Enabled((result) => {
  console.log(result);
});

// Unsubscribe to onWeb3Enabled events
unsubscribe();
```

**Result object**

```javascript
{
  account: '0x1a2b3c4d....',
  chain: '0x1',
  connector: {} // the connector instance
  web3: {} // the Ethers.js web3 instance
  provider: {} // the EIP1193-provider, used to connect
}
```

### Moralis.onWeb3Deactivated()

Listen to events when a user disconnects to a chain (via `Moralis.deactivateWeb3()`):

```javascript
// Subscribe to onWeb3Deactivated events
const unsubscribe = Moralis.onWeb3Deactivated((result) => {
  console.log(result);
});

// Unsubscribe to onWeb3Deactivated events
unsubscribe();
```

**Result object**

```javascript
{
  connector: {
  } // the connector instance
  provider: {
  } // the EIP1193-provider, used to connect
}
```

### Moralis.onChainChanged()

Listen to events when the user changes a chain, after web3 has been enabled.

```javascript
// Subscribe to onChainChanged events
const unsubscribe = Moralis.onChainChanged((chain) => {
  console.log(chain);
  // returns the new chain --> ex. "0x1"
});

// Unsubscribe to onChainChanged events
unsubscribe();
```

### Moralis.onAccountChanged()

Listen to events when the user changes an account, after web3 has been enabled.

```javascript
// Subscribe to onAccountChanged events
const unsubscribe = Moralis.onAccountChanged((chain) => {
  console.log(chain);
  // returns the new account --> ex. "0x1a2b3c4d..."
});

// Unsubscribe to onAccountChanged events
unsubscribe();
```

### Moralis.onConnect()

_You might want to use Moralis.onWeb3Activated instead, as that is more consistent, and handled by Moralis, instead of the EIP-1193 provider_

Listen to events when the provider fires a connect event.

From [the specification](https://eips.ethereum.org/EIPS/eip-1193):

> The Provider emits connect when it:
>
> * first connects to a chain after being initialized.
> * first connects to a chain, after the disconnect event was emitted.

```javascript
// Subscribe to onConnect events
const unsubscribe = Moralis.onConnect((provider) => {
  console.log(provider);
  // returns the EIP-1193 provider
});

// Unsubscribe to onConnect events
unsubscribe();
```

### Moralis.onDisconnect()

_You might want to use Moralis.onWeb3Deactivated instead, as that is more consistent, and handled by Moralis, instead of the EIP-1193 provider_

Listen to events when the provider fires a disconnect event.

From [the specification](https://eips.ethereum.org/EIPS/eip-1193):

> The Provider emits disconnect when it becomes disconnected from all chains

```javascript
// Subscribe to onDisconnect events
const unsubscribe = Moralis.onDisconnect((error) => {
  console.log(error);
  // returns a ProviderRpcError
});

// Unsubscribe to onDisconnect events
unsubscribe();
```

To listen to these events you just need to call `Moralis.onXXX`. For example:

```javascript
const unsubscribe = Moralis.onAccountChanged(function (account) {
  console.log(account);
});
// "0x1a2b3c4d..."
```

## Metamask (and other wallet) events

If you want to listen for events **before** an user is connected (on metamask), then you might want to listen to the events from Metamask directly

https://docs.metamask.io/guide/ethereum-provider.html#events

For example

```javascript
window.ethereum.on('accountsChanged' (accounts) => {
  console.log(accounts)
  // --> [0x1a2b3c4d...]
})
```

Other wallets have similar implementations, please look at the documentation of the respective wallet.

## Linking

Link (unlink) an address to the current user.

* link(address)
* unlink(address)

The user may have multiple addresses they wish to associate with their profile. This can be done with the `link` function after the user has been authenticated.

```javascript
Moralis.onAccountChanged(async function (account) {
  const confirmed = confirm("Link this address to your account?");
  if (confirmed) {
    await Moralis.link(account);
    alert("Address added!");
  }
});
```

The `unlink` function removes the given address from the user's profile.

```javascript
await Moralis.unlink(address);
```

Also, see the [Crypto Login](../users/crypto-login.md) page for more info.

## Changing Linking Message

The message the user sees when links an address to the current user can be changed by providing the `signingMessage` option:

```javascript
await Moralis.link(accounts[0], { signingMessage: "Custom Linking Message" });
```

## ensureWeb3IsInstalled

Detect if web3 provider is activated.

Returns: `True` or `False`

```javascript
const isWeb3Active = Moralis.ensureWeb3IsInstalled();

if (isWeb3Active) {
  console.log("Activated");
} else {
  await Moralis.enable();
}
```

## deactivateWeb3

Deactivates current web3 connection

```javascript
      await Moralis.enableWeb3();
      console.log("ENABLED", Moralis.isWeb3Enabled());
      await Moralis.deactivateWeb3();
      console.log("DISABLED", Moralis.isWeb3Enabled());
```

## connector / connectorType

Returns details of the connector or the connector type, that is used to authenticate or enable web3:

```javascript
const connectorType = Moralis.Web3.connectorType;
if (connectorType === "injected") {
  console.log("Metamask or an injected provider is used");
}

const connector = Moralis.Web3.connector;
console.log(connector);
```

## chainId

Returns the current chain ID that is used to connect to web3

```javascript
const chainId = await Moralis.chainId;
console.log(chainId); // 56
```

## account

Returns the current account that is used to connect to web3

```javascript
const account = await Moralis.account;
console.log(account); // "0x...."
```

## switchNetwork

Function to change the current network

> Note: this method can only be used, when Metamask is used as a connector

#### Options:

* `chain`(required): The chain id to switch to. Accepts values in numbers or in hex strings. Valid values are listed on the [intro page in the Transactions and Balances section](https://docs.moralis.io/moralis-dapp/web3-api/supported-chains#supported-chains). Examples: `56`, `"0x38"`

```javascript
const chainId = "0x1"; //Ethereum Mainnet
const chainIdHex = await Moralis.switchNetwork(chainId);
```

{% hint style="info" %}
You can only switch to the network that is in the wallet. How to add a new network you can find below.
{% endhint %}

## addNetwork

> Note: this method can only be used, when Metamask is used as a connector

The function for adding a new network to the wallet. You can find the network configuration for each chain in their documentation sites.

#### Options:

* `chainid`(required): Network Chain Id
* `chainName`(required): Network name
* `currencyName`(required): Name of native currency
* `currencySymbol`(required): Currency Symbol
* `rpcUrl`(required): New RPC URL
* `blockExplorerUrl`(required): BLock Explorer URL

```javascript
const chainId = 43114;
const chainName = "Avalanche Mainnet";
const currencyName = "AVAX";
const currencySymbol = "AVAX";
const rpcUrl = "https://api.avax.network/ext/bc/C/rpc";
const blockExplorerUrl = "https://cchain.explorer.avax.network/";

await Moralis.addNetwork(
  chainId,
  chainName,
  currencyName,
  currencySymbol,
  rpcUrl,
  blockExplorerUrl
);
```
