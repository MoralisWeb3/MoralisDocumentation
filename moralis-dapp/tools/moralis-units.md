# Moralis Units

## Intro

All crypto transactions are made in the smallest value - Wei. But to improve the user experience, on the frontend the transfer amounts are usually inputed in an `ETH `style. Users want to input value `0.0001 TokenA` instead of `100000000000000 Wei TokenA`. Standard web3 methods are inconvenient for easy converting. The most convenient conversion method is to use the `Moralis.Units` helper functions.

{% hint style="info" %}
Wei is the smallest denomination of ether, and you should always make calculations in Wei and convert only for display reasons.
{% endhint %}

{% hint style="info" %}
Due to the limitations of C#(csharp) `int` and `long` (32 and 64 bits) bit size been exceeded, `BigInteger` from `using System.Numerics;` was used due to it large bit size. `int` and `long` can be used but not recommended (`long` can be used as it is larger than `int` but it will still be easily exceeded).
{% endhint %}

```csharp
long value = (long)UnitConversion.Convert.ToWei(5);

// expected output: 5000000000000000000 Wei
// max length = 19
// long length > 19 will yield Overflow Exception error
```

## Converting ERC20 Token to Wei

To convert ERC20 token to Wei, you need to specify the amount of tokens and number of decimals.

{% tabs %}

{% tab title="Javascript" %}

```javascript
//Example: We want to convert 0.5 BUSD. It has 18 decimals
const busdInWei = Moralis.Units.Token("0.5", "18");
// expected output: 500000000000000000 Wei
```

{% endtab %}
{% tab title="Unity" %}

```csharp
// using namespace
using Nethereum.Util;
using System.Numerics;

//conversion code
//Example: We want to convert 0.5 BUSD. It has 18 decimals
BigInteger busdInWei = UnitConversion.Convert.ToWei(0.5, 18);
// expected output: 500000000000000000 Wei
```

{% endtab %}
{% endtabs %}

The number of decimals for a token can normally be found on Etherscan, on the website of the project or from Moralis API.

## Converting Native Asset (ETH/BNB/MATIC etc) to Wei

To convert native asset to Wei, you need to specify the amount of native crypto.

{% tabs %}

{% tab title="Javascript" %}

```javascript
//Example: We want to convert 0.5 ETH to Wei
const ethInWei = Moralis.Units.ETH("0.5");
// expected output: 500000000000000000 Wei
```

{% endtab %}
{% tab title="Unity" %}

```csharp
// using namespace
using Nethereum.Util;
using System.Numerics;

//conversion code
//Example: We want to convert 0.5 ETH to Wei
BigInteger ethInWei = UnitConversion.Convert.ToWei(0.5);
// expected output: 500000000000000000 Wei
```

{% endtab %}
{% endtabs %}

## Converting token value from Wei

All token values are shown in Wei. If you want to display token values in the "Eth" style, you can use:

{% tabs %}
{% tab title="Javascript" %}

```javascript
//Convert token value to ETH style with 6 decimals
const tokenValue = Moralis.Units.FromWei("2000000000000000000", 6);

//Convert token value to ETH style with 18 decimals
//If you do not specify decimals, 18 decimals will be automatically used
const tokenValue = Moralis.Units.FromWei("2000000000000000000");
```

{% endtab %}
{% tab title="Unity" %}

```csharp
// using namespace
using Nethereum.Util;
using System.Numerics;

//conversion code
//Convert token value to ETH style with 6 decimals
BigInteger tokenValue = UnitConversion.Convert.FromWei(2000000000000000000,6);

//Convert token value to ETH style with 18 decimals
//If you do not specify decimals, 18 decimals will be automatically used
BigInteger tokenValue = UnitConversion.Convert.FromWei(2000000000000000000);
```

{% endtab %}
{% endtabs %}

Note that this function is not available in the cloud code yet. You can use it in the SDK only. We are very soon adding it to the cloud code.

Web3 API responses for token balances have **decimals** and **balance** in Wei fields:

```javascript
  {
    "token_address": "0x...b0",
    "name": "name",
    "symbol": "NAME",
    "logo": null,
    "thumbnail": null,
    "decimals": "18",
    "balance": "2000000000000000000"
  },
```
