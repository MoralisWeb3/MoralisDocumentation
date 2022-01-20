---
description: Web3 Specific Functionality of the Moralis SDK.
---

# Web3 Provider

Let's talk about Web3 integration!

Moralis comes with a set of plugins to help you focus on the development of your app, leaving the rest to us.

Similar to `window.ethereum.enable()` but returns a promise that resolves to a `Web3` instance from Ethers.js. Use this when you need a fully functional `Web3` instance, such as for making contract calls.

> ❗️ Note: make sure to have v1.0.0 or higher of the SDK installed. Otherwise Moralis.enableWeb3() will return an instance of web3.js instead of ethers.js

*Example*
```javascript
const web3Provider = await Moralis.enableWeb3();
```

See for more info on how to use Ethers.js, in the [Ethers.js docs](https://docs.ethers.io/)

### Web3.js
You might want to use another library, like [Web3.js](https://web3js.readthedocs.io/). To do so, you need to include this library to your code.

*When you import javascript scripts via html:*
```html
<script src="https://cdn.jsdelivr.net/npm/web3@latest/dist/web3.min.js"></script>
```
*Or when you use a package manager:*

```sh
npm install web3
```

Then in your code, use `Moralis.provider` to initialize the web3.js library. `Moralis.provider` will only be set after calling Moralis.authenticate or Moralis.enableWeb3.
```javascript
import Web3 from "web3"; // Only when using npm/yarn

// Enable web3 and get the initialized web3 instance from Web3.js
await Moralis.enableWeb3();
const web3 = new Web3(Moralis.provider)
```

See for more info on how to use Ethers.js, in the  [Web3.js docs](https://web3js.readthedocs.io)

### Connectors
You enable web3 with any connector (such as WalletConnect, Metamask, Network etc.), the [same way as with Moralis.authenticate](https://docs.moralis.io/moralis-server/users/crypto-login#ethereum-bsc-and-polygon-login)), with the `provider` option:

```javascript
const web3 = await Moralis.enableWeb3({ provider: "walletconnect" });
```

### In Cloud Functions

It's also possible to get a Web3 instance inside cloud functions, but the syntax is a bit different. See the [Cloud Functions Web3](../cloud-code/cloud-functions.md#web3) page for more info.

This instance is a **Web3.js instance**

## executeFunction

Runs a smart contract function. You can execute `.call()` or`send()` smart contract methods.&#x20;

#### Options:

* `contractAddress` (required): A smart contract address
* `abi` (required): Contract's or function ABI(should be provided as an array)
* `functionName` (required): A function name
* `params` (required): Parameters needed for your specific function
* `msgValue`(optional): Number|String|BN|BigNumber. The value transferred for the transaction in wei.

```javascript
const ABI = [
  {
    constant: true,
    inputs: [
      {
        internalType: "address",
        name: "owner",
        type: "address"
      },
      {
        internalType: "address",
        name: "spender",
        type: "address"
      }
    ],
    name: "allowance",
    outputs: [
      {
        internalType: "uint256",
        name: "",
        type: "uint256"
      }
    ],
    payable: false,
    stateMutability: "view",
    type: "function"
  }
];

const options = {
  contractAddress: "0xe...56",
  functionName: "allowance",
  abi: ABI,
  params: {
    owner: "0x2...45",
    spender: "0x3...49"
  },
};

const allowance = await Moralis.executeFunction(options);
```

{% hint style="info" %}
Requires an enabled web3 provider. Before executing the function, make sure that the correct network is selected in your wallet.&#x20;
{% endhint %}

{% hint style="info" %}
If you use WalletConnect, check [#callbacks-promises-events](web3.md#callbacks-promises-events "mention") to be able recieve tsHash, receipt and etc.
{% endhint %}

{% hint style="info" %}
You can recieve this information without an active web3 provider using our Web3API: [Moralis.Web3API.token.getAllowance()](https://docs.moralis.io/moralis-server/web3-sdk/token#gettokenallowance)
{% endhint %}

### Example of calling a send contract method

An example of calling an ERC20 standard method `approve` . It sets `amount` as the allowance of `spender` over the caller’s tokens.

```javascript
const ABI = [
  {
    "constant": false,
    "inputs": [
      {
        "internalType": "address",
        "name": "spender",
        "type": "address"
      },
      {
        "internalType": "uint256",
        "name": "amount",
        "type": "uint256"
      }
    ],
    "name": "approve",
    "outputs": [
      {
        "internalType": "bool",
        "name": "",
        "type": "bool"
      }
    ],
    "payable": false,
    "stateMutability": "nonpayable",
    "type": "function"
  },
];

const options = {
  contractAddress: "0xe...56",
  functionName: "approve",
  abi: ABI,
  params: {
    spender: "0x2...45",
    amount: Moralis.Units.Token("0.5", "18")
  },
};

const receipt = await Moralis.executeFunction(options);
console.log(receipt)
```

### Transfering native crypto in contract method example&#x20;

For example, you want to call a smart contract method that exchanges native cryptocurrency for tokens. This case is different from the usual situations where you use ERC20 tokens and specify the number of tokens to use as a method parameter. Action with a native cryptocurrency in a smart contract assumes that you will transfer some value along with the transaction. In solidity you can know it as a `msg.value`

```javascript
const options = {
  contractAddress: "0xe...56",
  functionName: "swapNativeForTokens",
  abi: ABI,
  msgValue: Moralis.Units.ETH("0.1")
};

const receipt = await Moralis.executeFunction(options);
console.log(receipt)
```

### Example of creating reusable options

You can use this construction to create reusable options for calling multiple contract methods.

```javascript
const options = {
  contractAddress: "0xe...56",
  abi: ABI,
};

const symbol = await Moralis.executeFunction({ functionName: 'symbol', ...options })
const decimals = await Moralis.executeFunction({ functionName: 'decimals', ...options })
const name = await Moralis.executeFunction({ functionName: 'name', ...options })
```
### Resolve transaction result

`Moralis.executeFunction()` returns a [transaction response](https://docs.ethers.io/v5/api/providers/types/#providers-TransactionResponse). This object contains all data about the transaction. If you need data about the **result** of the transation, then you need to wait for the transaction to be **confirmed**.

You can do this via `transaction.wait()` to wait for 1 confirmation (or `transaction.wait(5)` to wait for 5 confirmations for example):

```javascript
const options = {
  contractAddress: "0xe...56",
  functionName: "swapNativeForTokens",
  abi: ABI,
  msgValue: Moralis.Units.ETH("0.1"),
}
const transaction = await Moralis.executeFunction(options)
const result = await transaction.wait()
```

## Events

There are several events, that you can listen to on Moralis:

* onConnect
* onDisconnect
* onWeb3Enabled
* onWeb3Deactivated
* onChainChanged
* onAccountChanged

```javascript
const unsubscribe = Moralis.onAccountChanged(function(account) {
  console.log(accounts);
});
// "0x0f7056a5778b15ebd522f39ab9782abf3ee37f08"
```

## Linking

Link (unlink) an address to the current user.

* link(address)
* unlink(address)

The user may have multiple addresses they wish to associate with their profile.  This can be done with the `link` function after the user has been authenticated.

```javascript
Moralis.onAccountChanged(async function (account) {
  const confirmed = confirm('Link this address to your account?');
  if (confirmed) {
    await Moralis.link(account);
    alert('Address added!');
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
await Moralis.link(accounts[0], { signingMessage: "Custom Linking Message"} );
```

## ensureWeb3IsInstalled

Detect if web3 provider is activated.

Returns: `True` or `False`

```javascript
const isWeb3Active = Moralis.ensureWeb3IsInstalled()

if (isWeb3Active) {
  console.log("Activated");
} else {
  await Moralis.enable();
}
```

## connector / connectorType

Returns details of the connector or the connector type, that is used to authenticate or enable web3:
```javascript
const connectorType =  Moralis.Web3.connectorType;
if(connectorType === 'injected'){
  console.log("Metamask or an injected provider is used")
}

const connector = Moralis.Web3.connector;
console.log(connector)

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

* `chain`(required): The chain id to switch to. Accepts values in numbers or in hex strings. Valid values are listed on the [intro page in the Transactions and Balances section](https://docs.moralis.io/moralis-server/web3-sdk/intro#supported-chains). Examples: `56`,  `"0x38"`

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

