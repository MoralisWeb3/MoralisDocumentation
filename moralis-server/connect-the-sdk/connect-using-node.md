---
description: >-
  Once you have your Moralis Server launched it's time to connect to it via the
  Moralis SDK. This guide will show you how you can do it in just a few easy
  steps.
---

## Adding Moralis to Node.js


### Installing Moralis SDK

Run the following command to install Moralis SDK

```
npm install moralis
```

### SDK Initialization

Create a folder `moralis-node-app` and a file `index.ts` inside it.

You need to initialize Moralis SDK with the following syntax in node.js:

```javascript
  /* Moralis init code */
const serverUrl = "YOUR-SERVER-URL";
const appId = "YOUR-APP-ID";
const masterKey = "YOUR-MASTER-KEY";

await Moralis.start({ serverUrl, appId, masterKey });

```
with `masterKey` you can directly access the moralis dashbaord without the need for authentication.


### DB query

#### Saving data

To save object with data copy paste the following code:

```javascript

    await Moralis.start({ serverUrl, appId, masterKey, moralisSecret })
    
    const Monster = Moralis.Object.extend("Monster");
    const monster = new Monster();

    monster.set("strength", 1024);
    monster.set("ownerName", "Aegon");
    monster.set("canFly", true);

    await monster.save()
```

Run the following command in your terminal:

```
npx ts-node index.ts
```

Go to your moralis dashboard and you will see the data saved in the database:

![](<images/node1.png>)


#### Query

```javascript

    const Monster = Moralis.Object.extend("Monster");
    const query = new Moralis.Query("Monster");
    
    const results = await query.find();
    console.log(results);
```

Run:

```
npx ts-node start.ts
```

In your console you will see:

![](<images/node2.png>)

For more info on DB Queries click [here](https://docs.moralis.io/moralis-server/database/queries)

### Live Query

Subscribing to Queries to Get Real-Time Alerts Whenever Data in the Query Result Set Changes.

add the following code in your file:

```javascript
    const Monster = Moralis.Object.extend("Monster");
    const query = new Moralis.Query(Monster);

    let subscription = await query.subscribe();
    console.log(subscription);

```
Run:

```
npx ts-node start.ts
```

In your console you will see:

![](<images/node3.png>)

For more info on Live Queries click [here](https://docs.moralis.io/moralis-server/database/live-queries)


### Web3API use


```javascript
const serverUrl = "YOUR-SERVER-URL";
const appId = "YOUR-APP-ID";
const moralisSecret = "YOUR MORALIS SECRET";

await Moralis.start({ serverUrl, appId, moralisSecret });

    //calling `getTokenPrice({address:"tokenAddress", chain:"chainID"})` from web3API
    const price = await Moralis.Web3API.token.getTokenPrice(
    {address: "0xe9e7cea3dedca5984780bafc599bd69add087d56", chain: "bsc"})
    console.log(price);
}

```

with `moralisSecret` all API calls go directly to the API instead of passing through the Moralis Server.

To get `moralisSecret` you need to go to account settings as shown in image below

![](<images/moralisSecret1.png>)

then API and copy your `moralisSecret` key

![](<images/moralisSecret2.png>)

Run:

```
npx ts-node start.ts
```

You will see the following result:

![](<images/result1.png>)

### Enable Moralis with Private key

### `Moralis.Transfer`






###

###
