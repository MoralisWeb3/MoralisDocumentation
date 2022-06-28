---
description: >-
  Transfer Native assets, NFTs, ERC20 tokens in unity on any blockchain - ETH (Ethereum), BNB (Binance Smart
  Chain), MATIC  (Polygon) e.t.c
---

# 🪙 🖼 🎴 Transfer ETH, Tokens and NFTs

### Transfer Native Assets (ETH/BNB/MATIC etc)

To transfer native assets of the blockchain follow the steps:&#x20;

1. Construct a `TransactionInput` object and set
   1. `Data: "String.Empty"`&#x20;
   2. `From: "0xe1e8..."`
   3. `To: "0x000..."`
   4. `Value: "new HexBigInteger(5000)"`
2. Call the Moralis transfer function as shown below

```csharp
//using directives
using MoralisUnity;
using MoralisUnity.Web3Api.Models;
using Nethereum.Util;
using Nethereum.Hex.HexTypes;
using MoralisUnity.Platform.Objects;
using Nethereum.RPC.Eth.DTOs;

// function
 public async void SendRawETH()
    {
        // Retrieve from address, the address used to athenticate the user.
        MoralisUser user = await Moralis.GetUserAsync();
        int transferAmount = 1;
        string fromAddress = user.authData["moralisEth"]["id"].ToString();
        string toAddress = "0x000...";
        // Create transaction request.
        TransactionInput txnRequest = new TransactionInput()
        {
            Data = String.Empty,
            //current connected user
            From = fromAddress,
            //reciever address
            To = toAddress,
            // amount in  wei
            Value = new HexBigInteger(UnitConversion.Convert.ToWei(transferAmount)) // convert to WEI
        };
        try
        {
            // Execute the transaction.
            string txnHash = await Moralis.Web3Client.Eth.TransactionManager.SendTransactionAsync(txnRequest);

            Debug.Log($"Transfered {transferAmount} ETH from {fromAddress} to {toAddress}.  TxnHash: {txnHash}");
        }
        catch (Exception exp)
        {
            Debug.Log($"Transfer of {transferAmount} ETH from {fromAddress} to {toAddress} failed! with error {exp}");
        }
    }
```

OR

```csharp

   public async void SendRawETH()
    {
        string toAddress = "0x000...";
        int transferAmount = 1;
        try
        {
            string txnHash = await Moralis.SendTransactionAsync(toAddress, new HexBigInteger(UnitConversion.Convert.ToWei(transferAmount)));
            Debug.Log($"Transfered {transferAmount} ETH to {toAddress}.  TxnHash: {txnHash}");
        }
        catch (Exception exp)
        {
            Debug.Log($"Transfer of {transferAmount} ETH to {toAddress} failed! with error {exp}");
        }
    }
```

#### Example result:

Returns the hash of the transaction

```csharp
"0x17b920d425938c0796f7fdbc4dec4f8ad37344cb62ef5df8b20d07bacbbaed22"
```

Use `UnitConversion.Convert.ToWei` to specify the amount in ETH (same goes for BSC and Polygon).&#x20;

{% hint style="info" %}
[**`UnitConversion.Convert.ToWei`**](../tools/moralis-units.md#converting-native-asset-eth-bnb-matic-etc-to-wei) gotten from `using Nethereum.Util;` is a helper function that will convert your ETH amount to [_wei_](https://ethdocs.org/en/latest/ether.html#denominations) which is required in order to construct the transaction.
{% endhint %}

### Transfer ERC20 Tokens (Fungible)

There is currently no method for Token transfer, this will be done with `ExecuteContractFunction` to call smart contract function to transfer Tokens  
The abi used is inline with the EIP20 standard and all tokens that use that standard should be transferable by changing the contract address of the Token

- `contractAddress` : The address of the deployed smart contract
- `abi` : The abi of the contract or just of that specific function. [You can convert the ABI to a string here](https://tools.knowledgewalls.com/json-to-string) to pass it as a string in code or it can be passed through the inspector
- `functionName`: The name of the smart contract function to be called
- `args`: The arguments passed into the function
- `value` : msg.value of a contract function
- `gas`: Transaction fee for the transaction
- `gasPrice`: This is the amount in wei the sender is willing to pay for the transaction

```csharp
//using directives
using MoralisUnity;
using MoralisUnity.Web3Api.Models;
using Nethereum.Hex.HexTypes;
using Nethereum.Util;
using System.Numerics;

//function
public async void transferToken()
    {
        string EIPTransferTokenABI = "[{\"inputs\":[{\"internalType\":\"address\",\"name\":\"to\",\"type\":\"address\"},{\"internalType\":\"uint256\",\"name\":\"amount\",\"type\":\"uint256\"}],\"name\":\"transfer\",\"outputs\":[{\"internalType\":\"bool\",\"name\":\"\",\"type\":\"bool\"}],\"stateMutability\":\"nonpayable\",\"type\":\"function\"}]";
         // change the contract address to transfer different tokens
        string DAIContractAddress = "0x6B175474E89094C44Da98b954EedeAC495271d0F";
        string FunctioName = "transfer";
        string receiverAddress = "0x0EF9...";
        BigInteger DAIInWei = UnitConversion.Convert.ToWei(5, 18);
        // params - toAddress, amount
       object[] inputParams = { receiverAddress, DAIInWei };
        HexBigInteger value = new HexBigInteger("0x0");
        HexBigInteger gas = new HexBigInteger("800000");
        HexBigInteger gasprice = new HexBigInteger("230000");
        try
        {
            string result = await Moralis.ExecuteContractFunction(contractAddress: DAIContractAddress, abi: EIPTransferTokenABI, functionName: FunctioName, args: inputParams, value: value, gas: gas, gasPrice: gasprice);
            Debug.Log("Txhash :" + result);
        }
        catch (Exception error)
        {
            Debug.Log("Error :" + error);
        }
    }
```

#### Example result:

Returns the hash of the transaction

```csharp
"0x17b920d425938c0796f7fdbc4dec4f8ad37344cb62ef5df8b20d07bacbbaed22"
```

### Transfer ERC721 Tokens (Non-Fungible)

There is currently no method for NFT transfer, this will be done `ExecuteContractFunction` to call smart contract function to transfer NFTs  
The abi used is inline with the EIP721 standard and all tokens that use that standard should be transferable by changing the contract address of the NFT  
This can also be recreated for NFT minting

- `contractAddress` : The address of the deployed smart contract
- `abi` : The abi of the contract or just of that specific function. [You can convert the ABI to a string here](https://tools.knowledgewalls.com/json-to-string) to pass it as a string in code or it can be passed through the inspector
- `functionName`: The name of the smart contract function to be called
- `args`: The arguments passed into the function
- `value` : msg.value of a contract function
- `gas`: Transaction fee for the transaction
- `gasPrice`: This is the amount in wei the sender is willing to pay for the transaction

```csharp
//using directives
using MoralisUnity;
using MoralisUnity.Web3Api.Models;
using MoralisUnity.Platform.Objects;
using Nethereum.Hex.HexTypes;


//function
public async void transferNFT721()
    {
        string EIPTransferNFTABI = "[{\"inputs\":[{\"internalType\":\"address\",\"name\":\"from\",\"type\":\"address\"},{\"internalType\":\"address\",\"name\":\"to\",\"type\":\"address\"},{\"internalType\":\"uint256\",\"name\":\"tokenId\",\"type\":\"uint256\"}],\"name\":\"transferFrom\",\"outputs\":[],\"stateMutability\":\"nonpayable\",\"type\":\"function\"}]";
        MoralisUser user = await Moralis.GetUserAsync();
        string fromAddress = user.authData["moralisEth"]["id"].ToString().ToLower();
        string toAddress = "0xE6502...";
        // change the contract address to transfer different tokens
        string BAYCContractAddress = "0xBC4CA0EdA7647A8aB7C2061c2E118A18a936f13D";
        string FunctioName = "transferFrom";
        // params - fromAddress, toAddress, tokenId
        object[] inputParams = { fromAddress, toAddress, 2 };
        HexBigInteger value = new HexBigInteger("0x0");
        HexBigInteger gas = new HexBigInteger("800000");
        HexBigInteger gasprice = new HexBigInteger("230000");
        try
        {
            string result = await Moralis.ExecuteContractFunction(contractAddress: BAYCContractAddress, abi: EIPTransferNFTABI, functionName: FunctioName, args: inputParams, value: value, gas: gas, gasPrice: gasprice);
            Debug.Log("Txhash :" + result);
        }
        catch (Exception error)
        {
            Debug.Log("Error :" + error);
        }
    }
```

<details>
<summary> safeTransferFrom for NFT 1155 (Semi-Fungible) token transfers</summary>

```csharp
public async void transferNFT1155()
    {
        string EIPTransferNFTABI = "[{\"inputs\":[{\"internalType\":\"address\",\"name\":\"from\",\"type\":\"address\"},{\"internalType\":\"address\",\"name\":\"to\",\"type\":\"address\"},{\"internalType\":\"uint256\",\"name\":\"id\",\"type\":\"uint256\"},{\"internalType\":\"uint256\",\"name\":\"amount\",\"type\":\"uint256\"},{\"internalType\":\"bytes\",\"name\":\"data\",\"type\":\"bytes\"}],\"name\":\"safeTransferFrom\",\"outputs\":[],\"stateMutability\":\"nonpayable\",\"type\":\"function\"}]";
        MoralisUser user = await Moralis.GetUserAsync();
        string fromAddress = user.authData["moralisEth"]["id"].ToString().ToLower();
        string toAddress = "0xE6502...";
        // change the contract address to transfer different tokens
        string PWContractAddress = "0x5E627f2DEB8Cd6728c5386195E16a220b7bD43ac";
        string FunctioName = "transferFrom";
        // params - fromAddress, toAddress, tokenId, token amount
        object[] inputParams = { fromAddress, toAddress, 2, 100 };
        HexBigInteger value = new HexBigInteger("0x0");
        HexBigInteger gas = new HexBigInteger("800000");
        HexBigInteger gasprice = new HexBigInteger("230000");
        try
        {
            string result = await Moralis.ExecuteContractFunction(contractAddress: PWContractAddress, abi: EIPTransferNFTABI, functionName: FunctioName, args: inputParams, value: value, gas: gas, gasPrice: gasprice);
            Debug.Log("Txhash :" + result);
        }
        catch (Exception error)
        {
            Debug.Log("Error :" + error);
        }
    }
```

</details>

#### Example result:

Returns the hash of the transaction

```csharp
"0x17b920d425938c0796f7fdbc4dec4f8ad37344cb62ef5df8b20d07bacbbaed22"
```
