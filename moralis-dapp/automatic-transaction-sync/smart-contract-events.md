---
description: >-
  Syncing historical smart contract events and getting notification when new
  events happen in real-time! Moralis puts the event data into your database in
  real time. You just have to provide the topic.
---

# Smart Contract Events

## Why do we need to sync and watch smart contract events?

Smart contracts emit so-called `events` when something meaningful happens within the smart contract that the smart contract wants to communicate to dapps and other smart contracts. The developers of the smart contract decide when these events are emitted. For example - when somebody sends an ERC20 token - the token contract will emit a `Transfer`event containing all of the data about the transfer.

When you are building dapps it's very important for you to be able to listen to these events in real time. For example, if you are building an NFT Marketplace, your dapp needs to know when somebody creates a new auction or when somebody bids on an item - the smart contract will communicate these events by emitting events.

Luckily, Moralis Server has this functionality built in.

**You can listen to smart contract events in real-time and also get all historic occurences of a specific event.**

{% hint style="info" %}
Legacy UI is present in this video, some things might be slightly different
{% endhint %}

{% embed url="https://www.youtube.com/watch?v=LMqqxkuo7b0" %}
A tutorial demonstrating how to sync and watch smart contract event with Moralis.
{% endembed %}

## The Graph vs Moralis For Smart Contract Indexing

{% hint style="info" %}
Legacy UI is present in this video, some things might be slightly different
{% endhint %}

{% embed url="https://www.youtube.com/watch?v=zrtcXd5cSe4" %}
Indexing Smart Contracts with The Graph vs Moralis
{% endembed %}

## Webhooks, SMS, Emails, Custom Code Based on Smart Contract Events

{% hint style="info" %}
Legacy UI is present in this video, some things might be slightly different
{% endhint %}

{% embed url="https://www.youtube.com/watch?v=PEILxU53-Zs" %}
This video explains how you can setup SMS, Email, Webhooks and run custom code when a smart contract even triggers.
{% endembed %}

## Sync and Watch Contract Events

### Defining

You can get all historical events from a specific smart contract topic and listen to new events in real-time. It requires the following information:

- \_**description**: \_a short description to identify this sync job.
- \_**topic**: \_The topic you will listen to, this could either be a definition or sha3:
  - `bet(address,uint256,bool)`
  - `0x20d932006281d267f137cd3921b707c2097e1f041b1291181cc3d0e86f449ebb`
- _**abi**_: If you provide the abi for this event, Moralis will automatically parse all the fields and populate the schema accordingly.
- _**address**_: The address you will listen to for this event.
- _**tableName**_: The name of the subclass that will be created in your Moralis database.

if you do not provide the \_\*\*abi \*\*\_then the following schema will be used:

```javascript
{
    "objectId": String,
    "block_hash": String,
    "topic0": String,
    "topic1": String,
    "block_timestamp": Date,
    "ACL": ACL,
    "data": String,
    "updatedAt": Date,
    "transaction_hash": String,
    "transaction_index": Number,
    "address": String,
    "log_index": Number,
    "createdAt": Date,
    "block_number": Number
}
```

This is an example of a valid _**abi:**_

```javascript
{
  "anonymous": false,
  "inputs": [
    {
      "indexed": true,
      "internalType": "address",
      "name": "user",
      "type": "address"
    },
    {
      "indexed": true,
      "internalType": "uint256",
      "name": "bet",
      "type": "uint256"
    },
    {
      "indexed": true,
      "internalType": "bool",
      "name": "win",
      "type": "bool"
    },
    {
      "indexed": false,
      "internalType": "uint8",
      "name": "side",
      "type": "uint8"
    }
  ],
  "name": "bet",
  "type": "event"
}
```

{% hint style="info" %}
If the abi provided contains an "id" input, it will then be parsed to "uid" due to field "id" being reserved in the Moralis.Object
{% endhint %}

### Table Name field limits

Table names are not allowed to contain nuneric characters (0 to 9)

### Name field limits

Name fields that start with and `_` don't work for now\_, \_like for example `"name": "_side"` will not work, and you'll have to change it to `"name": "side"` in order to make it work.

### Historical Event Limit

If a sync job is created that would result in retrieving 500k or more historical events, then the `"Sync_historical (Optional)`option will be disabled and no historical data will be saved. It is possible to contact support to upgrade your account to enable saving it anyway but think hard about whether it's actually necessary before doing so. It's possible to handle the events in real-time without saving the data to the database.

### Editing

To edit a sync job, click on _View Details_ on the server where the plugin is installed and go to the Sync tab. There you will see all your sync jobs. Click the \_Edit \_button.

![](<../../.gitbook/assets/image (97).png>)

Since making any alterations is potentially a breaking change to the event schema, saving the changes requires providing a new `tableName`. Boo-urns! Yes, this is inconvenient, but how plugins work will be changing soon anyway when the custom plugin platform is released and this will then be reexamined (so at least there's that to look forward to!).

If a new table name is not provided you'll see the following error message:

![This error message will also appear if you accidently name it the same as an existing plugin.](<../../.gitbook/assets/image (78).png>)

## Event Filters

Imagine you only want to get events in your database based on some conditions.

Moralis gives you an easy way to define filters based on ABI variables.

For example the filter below will only sync event instances where the variable `price` is equal to `80000000000000000`:

`{"eq": ["price", "80000000000000000"]}`

Below are some more filter examples. You can set conditions for any variable in the event.

If `sender` = `0x0` AND `receiver` = `0x0`

```
{
  "and": [
    { "eq": ["sender", "0x0"] },
    { "eq": ["receiver", "0x0"] }
  ]
}
```

If `sender` = `0x0` OR `receiver` = `0x0`

```
{
  "or": [
    { "eq": ["sender", "0x0"] },
    { "eq": ["receiver", "0x0"] }
  ]
}
```

If (`sender` = `0x0` AND `amount` = `1000` ) OR (`receiver` = `0x0` AND `amount` = `100` )

```
{
  "or": [
    {
      "and": [
       { "eq": ["sender", "0x0"] },
       { "eq": ["amount", "1000"]}
      ]
    },
    {
      "and": [
       { "eq": ["receiver", "0x0"] },
       { "eq": ["amount", "1000"]}
      ]
    }
  ]
}
```

If an event has wei denominated values you can use `<name>_decimals` fields to run comparisons (like greater than/less than):

```
{
  "and": [
    { "eq": ["sender", "0x0"] },
    { "gt": ["amount_decimals", "1000"]}
  ]
}
```

## Watching from Code

### Watch new smart contract event

Moralis Server has a special cloud function called `watchContractEvent(options)`. You can call it using the master key.

_Note: limit parameter is available only for Nitro servers (those one that have coreservices plugin). If limit parameter is not provided then the default value is 500000._

_Note: at the moment the events created via code won't be seen in the admin UI, you can only see them in the database, we are working on connecting the admin UI properly_

```javascript
// code example of creating a sync event from cloud code
let options = {
  chainId: "0x1",
  address: "0x5c69bee701ef814a2b6a3edd4b1652cb9cc5aa6f",
  topic: "PairCreated(address, address, address, uint256)",
  abi: {
    anonymous: false,
    inputs: [
      {
        indexed: true,
        internalType: "address",
        name: "token0",
        type: "address",
      },
      {
        indexed: true,
        internalType: "address",
        name: "token1",
        type: "address",
      },
      {
        indexed: false,
        internalType: "address",
        name: "pair",
        type: "address",
      },
      {
        indexed: false,
        internalType: "uint256",
        name: "test",
        type: "uint256",
      },
    ],
    name: "PairCreated",
    type: "event",
  },
  limit: 500000,
  tableName: "UniPairCreated",
  sync_historical: false,
};

Moralis.Cloud.run("watchContractEvent", options, { useMasterKey: true });
```

### Unwatch existing smart contract event

```javascript
// unwatch event that has TABLE_NAME as table in the database
let options = { tableName: "TABLE_NAME" };
Moralis.Cloud.run("unwatchContractEvent", options, { useMasterKey: true });
```

### Important Note

These features to watch/unwatch from code are still beta.

[Join our Discord](https://moralis.io/mage) to discuss the development!
