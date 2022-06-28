---
description: Smart contracts interactions in Unity with the Moralis Unity SDK.
---

# Blockchain Interactions Unity

## smart contracts

what are smart contracts ?

* Simple definition : Smart contracts are simply programs stored on a blockchain that run when predetermined conditions are met. [Learn more about smart contracts](https://www.ibm.com/topics/smart-contracts)

## Execute smart contract function

ExecuteContractFunction calls smart contract functions

### options

* `contractAddress` : The address of the deployed smart contract
* `abi` : The abi of the contract or just of that specific function. [You can convert the ABI to a string here](https://tools.knowledgewalls.com/json-to-string) to pass it as a string in code or it can be passed through the inspector
* `functionName`: The name of the smart contract function to be called
* `args`: The arguments passed into the function
* `value` : msg.value of a contract function
* `gas`: Transaction fee for the transaction
* `gasPrice`: This is the amount in wei the sender is willing to pay for the transaction

```csharp
//using directives
using MoralisUnity;
using MoralisUnity.Web3Api.Models;
using Nethereum.Hex.HexTypes;


//function
public async void executeContractFunction()
    {
        string ABI = "[{\"inputs\":[{\"internalType\":\"int256\",\"name\":\"p\",\"type\":\"int256\"}],\"name\":\"test\",\"outputs\":[],\"stateMutability\":\"nonpayable\",\"type\":\"function\"}]";
        string ContractAddress = "0xD3622d5eDA04B0A393EA10513239A1fD50A61B65";
        string FunctioName = "test";
        object[] inputParams = { 3 };
        HexBigInteger value = new HexBigInteger("0x0");
        HexBigInteger gas = new HexBigInteger("800000");
        HexBigInteger gasprice = new HexBigInteger("230000");
        try
        {
            string result = await Moralis.ExecuteContractFunction(contractAddress: ContractAddress, abi: ABI, functionName: FunctioName, args: inputParams, value: value, gas: gas, gasPrice: gasprice);
            Debug.Log("Txhash :" + result);
        }
        catch (Exception error)
        {
            Debug.Log("Error :" + error);
        }
    }
```

### Example result:

Returns the hash of the transaction

```csharp
"0x17b920d425938c0796f7fdbc4dec4f8ad37344cb62ef5df8b20d07bacbbaed22"
```

{% hint style="info" %}
`ExecuteContractFunction` is currently the only accepted way to call smart contract functions, there are other methods like [SendEvmTransactionAsync](https://github.com/MoralisWeb3/web3-unity-sdk/blob/main/Runtime/Core/Moralis.cs#L835), [SendTransactionAndWaitForReceiptAsync](https://github.com/MoralisWeb3/web3-unity-sdk/blob/main/Runtime/Core/Moralis.cs#L876) but they only work for platform and are deprecated
{% endhint %}
