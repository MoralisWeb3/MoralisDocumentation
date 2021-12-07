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

{% embed url="https://www.youtube.com/watch?v=LMqqxkuo7b0" %}
A tutorial demonstrating how to sync and watch smart contract event with Moralis.
{% endembed %}

## Webhooks, SMS, Emails, Custom Code Based on Smart Contract Events

{% embed url="https://www.youtube.com/watch?v=PEILxU53-Zs" %}
This video explains how you can setup SMS, Email, Webhooks and run custom code when a smart contract even triggers.
{% endembed %}

## Sync and Watch Contract Events

### Defining

You can get all historical events from a specific smart contract topic and listen to new events in real-time. It requires the following information:

* \_**description**: \_a short description to identify this sync job.
* \_**topic**: \_The topic you will listen to, this could either be a definition or sha3:
  * `bet(address,uint256,bool)`
  * `0x20d932006281d267f137cd3921b707c2097e1f041b1291181cc3d0e86f449ebb`
* _**abi**_: If you provide the abi for this event, Moralis will automatically parse all the fields and populate the schema accordingly.
* _**address**_: The address you will listen to for this event.
* _**tableName**_: The name of the subclass that will be created in your Moralis database.

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

## Manual Event Handling

If you start a sync job that would result in retrieving 500,000 or more historical events then the `Sync_historical` option will be disabled and no historical data will be fetched. The contract event (or watched address transaction) will still be monitored by Moralis, but it requires a trigger to tell Moralis what to do with the data. Until a `beforeConsume` trigger is defined, the default behavior will be to skip the event and log a message to the dashboard.

![A skipped events are logged to the dashboard.](<../../.gitbook/assets/image (79).png>)

### beforeConsume Trigger

When historical data is not synced automatically a `beforeConsume` trigger needs to be created on the `tableName` given in the plugin definition.

```javascript
// always save event
Moralis.Cloud.beforeConsume(eventTableName, (event) => {
  return true;
});

// only save event if transaction is confirmed
Moralis.Cloud.beforeConsume(eventTableName, (event) => {
  return event && event.confirmed;
});
```

The callback should:

* Return `Boolean`.
  * When `true` the event will be saved, otherwise, it will be skipped.
* Be synchronous.
  * If a `Promise` is returned, it will NOT be awaited, and since a promise is a truthy value the event data will be saved to the database.
  * Because the callback is synchronous it should be as fast as possible. Ideally, the callback will make its decision based entirely on the input data with no database lookups or other outside calls (a "pure" function). If the callback is slow it could negatively affect the performance of your entire server.
* Have an input parameter. This will contain the properties of the event data, not a Moralis object.
