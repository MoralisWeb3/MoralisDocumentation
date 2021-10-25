---
description: Web3 Specific Functionality of the Moralis SDK.
---

# Web3 Provider

Let's talk about Web3 integration!

Moralis comes with a set of plugins to help you focus on the development of your app, leaving the rest to us.

## Web3.js

### In Browser

Similar to `window.ethereum.enable()` but returns a promise that resolves to a `Web3` instance. Use this when you need a fully functional `Web3` instance, such as for making contract calls.

```javascript
const web3 = await Moralis.enableWeb3();
const contract = new web3.eth.Contract(contractAbi, contractAddress);
```

If you want to use providers like [WalletConnect](https://docs.moralis.io/moralis-server/users/crypto-login#walletconnect) you need to specify the `provider`option

```javascript
const web3 = await Moralis.enableWeb3({ provider: "walletconnect" });
const contract = new web3.eth.Contract(contractAbi, contractAddress);
```

If you only need to access `web3.utils` without Web3 connectivity, use the following:

```javascript
const web3 = new Moralis.Web3();
const amountInEth = web3.utils.fromWei(amountInWei, 'ether');
```

Just beware that calls like `web3.eth.getAccounts()` will not work. You can re-assign the variable using `Moralis.enableWeb3()` later if you need full Web3 connectivity.

See the [Web3.js docs](https://web3js.readthedocs.io) for more info on the Web3 object.

### In Cloud Functions

It's also possible to get a Web3 instance inside cloud functions, but the syntax is a bit different. See the [Cloud Functions Web3](../cloud-code/cloud-functions.md#web3) page for more info.

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

## Events

There are wrappers for the standard Web3 change events.

* onConnect
* onDisconnect
* onChainChanged
* onAccountsChanged

```javascript
const unsubscribe = Moralis.onAccountsChanged(function(accounts) {
  console.log(account);
});
// ["0x0f7056a5778b15ebd522f39ab9782abf3ee37f08"]
```

## Linking

Link (unlink) an address to the current user.

* link(address)
* unlink(address)

The user may have multiple addresses they wish to associate with their profile.  This can be done with the `link` function after the user has been authenticated.

```javascript
Moralis.onAccountsChanged(async function (accounts) {
  const confirmed = confirm('Link this address to your account?');
  if (confirmed) {
    await Moralis.link(accounts[0]);
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

The message the user sees when links an address to the current user can be changed by providing the `signingMessage `option:

```javascript
await Moralis.link(accounts[0], { signingMessage: "Custom Linking Message"} );
```

## ensureWeb3IsInstalled

Detect if web3 provider is activated.

Returns: `True `or `False`

```javascript
const isWeb3Active = Moralis.ensureWeb3IsInstalled()

if (isWeb3Active) {
  console.log("Activated");
} else {
  await Moralis.enable();
}
```

## isMetaMaskInstalled

Detect if MetaMask web3 provider is installed.

Returns: `True `or `False`

```javascript
const isMetaMaskInstalled= await Moralis.isMetaMaskInstalled();
console.log(isMetaMaskInstalled) 
```

## getChainId

Returns the current chain ID

```javascript
const chainId = await Moralis.getChainId();
console.log(chainId); // 56
```

## switchNetwork

Function to change the current network

#### Options:

* `chain`(required): The chain id to switch to. Accepts values in numbers or in hex strings. Valid values are listed on the [intro page in the Transactions and Balances section](https://docs.moralis.io/transactions-and-balances/intro). Examples: `56`,  `"0x38"`

```javascript
const chainId = "0x1"; //Ethereum Mainnet
const chainIdHex = await Moralis.switchNetwork(chainId); 
```

{% hint style="info" %}
You can only switch to the network that is in the wallet. How to add a new network you can find below.
{% endhint %}

## addNetwork

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

