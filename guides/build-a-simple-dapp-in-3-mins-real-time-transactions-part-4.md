---
description: >-
  Build Your First Moralis dApp! This Guide Series Covers the Basics and How to
  Get Started Fast. In Part Four, We Subscribe to Our Query to Get Real-Time
  Alerts.
---

# Build a Simple dApp in 3 Mins - Real-Time Transactions (Part 4)

## How to Get Real-Time Data

Not only can we get all past transactions, but we can also get notifications when new transactions happen!

### Just Subscribe to the Query

```javascript
const subscription = await query.subscribe();
```

### And Handle the Events

```javascript
subscription.on("create", function (data) {
  console.log("new transaction: ", data);
});
```

That's it!... we can modify our `getUserTransactions()` function as follows:

```javascript
async function getUserTransactions(user) {
  // create query
  const query = new Moralis.Query("EthTransactions");
  query.equalTo("from_address", user.get("ethAddress"));

  // subscribe to query updates ** add this**
  const subscription = await query.subscribe();
  handleNewTransaction(subscription);

  // run query
  const results = await query.find();
  console.log("user transactions:", results);
}

async function handleNewTransaction(subscription) {
  // log each new transaction
  subscription.on("create", function (data) {
    console.log("new transaction: ", data);
  });
}
```

Now, whenever the user makes a new transaction on the mainnet, it will print a message.
Note: on new nitro servers (the ones that have coreservices plugin), update instead of create should be used for subscription.

### Outstanding!

Real-time action! Can't stop now, we're almost to the finish line... keep going!

{% content-ref url="build-a-simple-dapp-in-3-mins-cloud-functions-part-5.md" %}
[build-a-simple-dapp-in-3-mins-cloud-functions-part-5.md](build-a-simple-dapp-in-3-mins-cloud-functions-part-5.md)
{% endcontent-ref %}
