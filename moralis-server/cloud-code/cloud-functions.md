---
description: Offload Compute Intensive or Security Sensitive Functions to the Server.
---

# Cloud Functions

## Defining Cloud Functions

{% embed url="https://www.youtube.com/watch?v=rJ88MPODVRg&list=PLFPZ8ai7J-iSgXptMiGlnofw4YKgNITgY&index=1&ab_channel=MoralisWeb3" %}

For complex apps, sometimes you just need a bit of logic that isn’t running on a mobile device. Cloud Code makes this possible.

Cloud Code is easy to use because it’s built on the same Moralis JavaScript SDK that powers thousands of apps. The only difference is that this code runs in your Moralis Server rather than running on the user’s mobile device. When you update your Cloud Code, it becomes available to all mobile environments instantly. You don’t have to wait for a new release of your application. This lets you change app behavior on the fly and add new features faster.

Even if you’re only familiar with mobile development, we hope you’ll find Cloud Code straightforward and easy to use.

Let’s look at a slightly more complex example where Cloud Code is useful. One reason to do computation in the cloud is so that you don’t have to send a huge list of objects down to a device if you only want a little bit of information. For example, let’s say you’re writing an app that lets people review movies. A single `Review` object could look like:

```javascript
{
  "movie": "The Matrix",
  "stars": 5,
  "comment": "Too bad they never made any sequels."
}
```

If you wanted to find the average number of stars for The Matrix, you could query for all of the reviews, and the average amount of stars on the device. However, this uses a lot of bandwidth when you only need a single number. With Cloud Code, we can just pass up the name of the movie, and return the average star rating.

Cloud Functions accept a JSON parameters dictionary on the `request` object, so we can use that to pass up the movie name. The entire Moralis JavaScript SDK is available in the cloud environment, so we can use that to query over `Review` objects. Together, the code to implement `averageStars` looks like this:

```javascript
Moralis.Cloud.define("averageStars", async (request) => {
  const query = new Moralis.Query("Review");
  query.equalTo("movie", request.params.movie);
  const results = await query.find();
  let sum = 0;
  for (let i = 0; i < results.length; ++i) {
    sum += results[i].get("stars");
  }
  return sum / results.length;
});
```

The only difference between using `averageStars` and `hello` is that we have to provide the parameter that will be accessed in `request.params.movie` when we call the Cloud Function. Read on to learn more about how Cloud Functions can be called.

### Global Packages

The following packages are available globally within Cloud Function code and can be used without a `require` statement.

* `crypto` ([docs](https://nodejs.org/api/crypto.html))

### ⚠️ IMPORTANT - Don't create any global variable

You cloud function code cannot have any variables outside the function bodies.

Example below.

```javascript
let name = "Satoshi"; // NOT ALLOWED

Moralis.Cloud.define("functionName", async (request) => {
  let age = 20; // allowed
});
```

The reason is that your cloud code will get load balanced across many instances of your server so that Moralis can infinitely scale your app.

The different instances of your server won't share any variables defined outside the function bodies.

All instances of your server will share the same database.

Therefore if you need to share data across instances it's recommended you store it in the [database](../database/).

### Console.log

For debugging or informational purposes it's often useful to print messages. A logger can be obtained for this purpose. It will print messages to the Moralis Dashboard in the "Logs > Info" section.

```javascript
const logger = Moralis.Cloud.getLogger();
logger.info("Hello World"); 
```

### Printing Logs in Real-Time in the Console

{% embed url="https://youtu.be/r8kWG-9tAXo" %}
Video explaining how to use the CLI in order to get logs in real-time.
{% endembed %}

```javascript
moralis-admin-cli get-logs --moralisApiKey MORALIS_CLI_API_KEY --moralisApiSecret MORALIS_CLI_SECRET_KEY
```

To learn more about CLI, how to install CLI and how to work with CLI please check the CLI docs using the link below.

To get started, you need to install it by running the following code in the terminal:

{% content-ref url="../tools/moralis-admin-cli.md" %}
[moralis-admin-cli.md](../tools/moralis-admin-cli.md)
{% endcontent-ref %}

### IDE Setup

You can write your Cloud Functions in your preferred IDE by making use of the `moralis-admin-cli`.

{% embed url="https://youtu.be/rJ88MPODVRg?t=437" %}
Exact time-stamp where we explain how to setup an IDE on your local machine.
{% endembed %}

To get started, you need to install it by running the following code in the terminal:

```bash
npm install -g moralis-admin-cli
```

After you have installed the Moralis Admin CLI, you can head to the admin panel and open up the Cloud Functions on the server you want to work on.

In the lower part of the modal, there will be a code snippet that you need to run in the terminal.

The only thing you need to change is the path to the local JavaScript folder on your computer that contains the Cloud Functions.

```javascript
moralis-admin-cli watch-cloud-folder --moralisApiKey your_api_key --moralisApiSecret your_api_secret --moralisSubdomain subdomain.moralis.io --autoSave 1 --moralisCloudfolder /path/to/cloud/folder
```

After you've run the command, the cloud code will be updated automatically on the backend with each save!

## Calling Cloud Functions

```javascript
const params =  { movie: "The Matrix" };
const ratings = await Moralis.Cloud.run("averageStars", params);
// ratings should be 4.5
```

In general, two arguments will be passed into Cloud Functions:

1. **`request`** - The request object contains information about the request. The following fields are set:
2. **`params`** - The parameters object is sent to the function by the client.
3. **`user`** - The `Moralis.User` that is making the request. This will not be set if there was no logged-in user.

If the function is successful, the response in the client looks like this:

```javascript
{ "result": 4.8 }
```

If there is an error, the response in the client looks like this:

```javascript
{
  "code": 141,
  "error": "movie lookup failed"
}
```

\
Using the Master Key in Cloud Code <a href="#using-the-master-key-in-cloud-code" id="using-the-master-key-in-cloud-code"></a>
-----------------------------------------------------------------------------------------------------------------------------

Set `useMasterKey:true` in the requests that require the master key.

### Examples: <a href="#examples-3" id="examples-3"></a>

```javascript
query.find({useMasterKey:true});
object.save(null,{useMasterKey:true});
Moralis.Object.saveAll(objects,{useMasterKey:true});
```

Note that master key is accessible by default in a cloud function when you use `useMasterKey:true`

Important: Master key should not be used on your frontend code as it would be accessible in the user's browser.

In case if you are using master key from another backend server that you are controlling you can use the following code to intialize the master key:

```javascript
const Moralis = require("moralis/node"); // Node.js
const appId = "YOUR_MORALIS_APP_ID";
const serverUrl = "YOUR_MORALIS_SERVER_URL";
const masterKey = "YOUR_MORALIS_MASTER_KEY"; 

Moralis.start({ serverUrl, appId, masterKey });
```

## Implementing Cloud Function Validation

It’s important to make sure the parameters required for a Cloud Function are provided and are in the necessary format. you can specify a validator function or object which will be called prior to your Cloud Function.

Let’s take a look at the `averageStars` example. If you wanted to make sure that `request.params.movie` is provided, and that `averageStars` can only be called by logged-in users, you could add a validator object to the function.

```javascript
Moralis.Cloud.define("averageStars", async (request) => {
  const query = new Moralis.Query("Review");
  query.equalTo("movie", request.params.movie);
  const results = await query.find();
  let sum = 0;
  for (let i = 0; i < results.length; ++i) {
    sum += results[i].get("stars");
  }
  return sum / results.length;
},{
  fields : ['movie'],
  requireUser: true
});
```

If the rules specified in the validator object aren’t met, the Cloud Function won’t run. This means that you can confidently build your function, knowing that `request.params.movie` is defined, as well as `request.user`.

### More Advanced Validation

Often, not only is it important that `request.params.movie` is defined, but also that it's the correct data type. You can do this by providing an `Object` to the `fields` parameter in the "Validator."

```javascript
Moralis.Cloud.define("averageStars", async (request) => {
  const query = new Moralis.Query("Review");
  query.equalTo("movie", request.params.movie);
  const results = await query.find();
  let sum = 0;
  for (let i = 0; i < results.length; ++i) {
    sum += results[i].get("stars");
  }
  return sum / results.length;
},{
  fields : {
    movie : {
      required: true,
      type: String,
      options: val => {
        return val.length < 20;
      },
      error: "Movie must be less than 20 characters"
    }
  },
  requireUserKeys: {
    accType : {
      options: 'reviewer',
      error: 'Only reviewers can get average stars'
    }
  }
});
```

This function will only run if:

* `request.params.movie` is defined.
* `request.params.movie` is a String.
* `request.params.movie` is less than 20 characters.
* `request.user` is defined.
* `request.user.get('accType')` is defined.
* `request.user.get('accType')` is equal to ‘reviewer.’

However, the requested user could set ‘accType’ to 'reviewer' and then recall the function. Here, you could provide validation on a `Moralis.User` `beforeSave` trigger. `beforeSave` validators have a few additional options available, to help you make sure your data is secure.

```javascript
Moralis.Cloud.beforeSave(Moralis.User, () => {
  // any additional beforeSave logic here
}, {
    fields: {
      accType: {
        default: 'viewer',
        constant: true
      },
    },
});
```

This means that the field `accType` on `Moralis.User` will be ‘viewer’ on signup, and will be unchangeable, unless `masterKey` is provided.

The full range of built-in validation options are:

* `requireMaster`: Whether the function requires a `masterKey` to run.
* `requireUser`: Whether the function requires a `request.user` to run.
* `validateMasterKey`: Whether the validator should run on `masterKey` (defaults to false).
* `fields`: An `Array` or `Object` of fields that are required on the request.
* `requireUserKeys`: An `Array` of fields to be validated on `request.user`.

The full range of built-in validation options on `.fields` are:

* `type`: The type of the `request.params[field]` or `request.object.get(field)`.
* `default`: What the field should default to if it’s `null`.
* `required`: Whether the field is required.
* `options`: A singular option, an array of options, or custom function of allowed values for the field.
* `constant`: Whether the field is immutable.
* `error`: A custom error message if validation fails.

You can also pass a function to the Validator. This can help you apply reoccurring logic to your Cloud Code.

```javascript
const validationRules = request => {
  if (request.master) {
    return;
  }
  if (!request.user || request.user.id !== 'masterUser') {
    throw 'Unauthorized';
  }
}

Moralis.Cloud.define('adminFunction', request => {
// do admin code here, confident that request.user.id is masterUser, or masterKey is provided
},validationRules)

Moralis.Cloud.define('adminFunctionTwo', request => {
// do admin code here, confident that request.user.id is masterUser, or masterKey is provided
},validationRules)
```

#### SOME CONSIDERATIONS TO BE AWARE OF

* The validation function will run prior to your Cloud Code Functions. You can use `async` and promises here, but try to keep the validation as simple and fast as possible so your cloud requests resolve quickly.
* As previously mentioned, cloud validator objects will not validate if a master key is provided, unless `validateMasterKey:true` is set. However, if you set your validator to a function, the function will **always** run.

## Calling via REST API

Cloud Functions can be called directly using a simple GET request. Add your Moralis App Id and any Cloud Function arguments as query parameters to your Moralis server URL. Say you had the following Cloud Function:

```javascript
Moralis.Cloud.define("Hello", (request) => {
  return `Hello ${request.params.name}! Cloud functions are cool!`
});
```

Then the URL would look something like the following:

```
https://1a2b3c4c5d6f.moralis.io:2053/server/functions/Hello?_ApplicationId=1a2b3c4c5d6f1a2b3c4c5d6f&name=CryptoChad
```

The URL has the following structure:

1. Full Morlis server url.
2. `/functions/`.
3. Cloud Function Name.
4. `?_ApplicationId=yourMoralisAppId`.
5. (optional) Cloud Function param key/value pairs: `&param1=value&param2=value`.

## Web3

Web3 functions are available within Cloud Code including the ability to call contract methods. Moralis uses the [Web3.js](https://web3js.readthedocs.io) library.

```javascript
// get a web3 instance for a specific chain
const web3 = Moralis.web3ByChain("0x1"); // mainnet
```

Ask for a `web3` object by supplying the `chainId` for the blockchain you wish to connect to. The following is a list of the currently supported chains.

**Note:** the `web3` instance returned by `Moralis.web3ByChain()` cannot sign transactions. There is a way to sign a transaction using a private key, but this is NOT recommended for security reasons.

| Chain Name                    | ChainId   |
| ----------------------------- | --------- |
| Ethereum (Mainnet)            | `0x1`     |
| Ropsten (Ethereum Testnet)    | `0x3`     |
| Rinkeby                       | `0x4`     |
| Goerli (Ethereum Testnet)     | `0x5`     |
| Kovan                         | `0x2a`    |
| Binance Smart Chain (Mainnet) | `0x38`    |
| Binance Smart Chain (Testnet) | `0x61`    |
| Matic (Mainnet)               | `0x89`    |
| Mumbai (Matic Testnet)        | `0x13881` |
| Local Dev (Ganache, Hardhat)  | `0x539`   |

### Contracts

Once you have a `web3` instance, you can use it to make contract calls by constructing a contract instance with the ABI and contract address.

```javascript
const contract = new web3.eth.Contract(abi, address);
```

For convenience, Moralis bundles the [Openzepplin](https://github.com/OpenZeppelin/openzeppelin-contracts/tree/master/contracts/token) ABI for ERC20, ERC721, and ERC1155.

* `Moralis.Web3.abis.erc20`
* `Moralis.Web3.abis.erc721`
* `Moralis.Web3.abis.erc1155`

Bringing it all together...

```javascript
const web3 = Moralis.web3ByChain("0x38"); // BSC
const abi = Moralis.Web3.abis.erc20;
const address = "0x...."

// create contract instance
const contract = new web3.eth.Contract(abi, address);

// get contract name
const name = await contract.methods
  .name()
  .call()
  .catch(() => "");
```

For more details on the Web3.js contract interface, see the [`web3.eth.Contract`](https://web3js.readthedocs.io/en/v1.3.4/web3-eth-contract.html) section of the Web3.js docs.

The Web3 instance returned by `Moralis.web3ByChain()` cannot sign transactions. In the near future, it may be possible to do this with custom plugins. For now, if you need to make on-chain contract interactions, consider doing them on the frontend or create a NodeJS backend where you have access to [Truffle's HdWalletProvider](https://github.com/trufflesuite/truffle/tree/develop/packages/hdwallet-provider).

### Contract ABI

To find the ABI for a contract already published on Ethereum's Mainnet you can look on [Etherscan](https://etherscan.io) by searching for the contract address and looking in the "Contract" tab. There are similar block explorers for other chains where the ABI is published. For instance, you can go to [BscScan](https://www.bscscan.com) for Binance Smart Chain.

For your own contracts, the ABI can be found in the build directory after compiling the contract

* Truffle: `/truffle/build/contracts/myContract.json`.
* Hardhat: `/artifacts/contracts/myContract.sol/myContract.json`.
