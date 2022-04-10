---
description: >-
  Subscribing to Queries to Get Real-Time Alerts Whenever Data in the Query
  Result Set Changes.
---

# Advance Live Queries

Follow this guide for the standard Live Query implementation:

{% content-ref url="live-queries.md" %}
[live-queries.md](live-queries.md)
{% endcontent-ref %}

## Advanced Connection

We manage a global WebSocket connection for you in our [standard connection](live-queries.md#standard-api) which is suitable for most cases. However, in some cases you have multiple LiveQuery servers and want to connect to all of them, **a single WebSocket connection isn't enough**. We've exposed the `LiveQueryClient` for such scenarios.

## LiveQueryClient

A `LiveQueryClient` is a wrapper of a [standard WebSocket client](live-queries.md#standard-connection). We add several useful methods to help you connect/disconnect to LiveQueryServer and subscribe/unsubscribe a `MoralisQuery` easily.

### Initialize

```javascript
let Moralis = require('moralis');
let LiveQueryClient = Moralis.LiveQueryClient;
let client = new LiveQueryClient({
  applicationId: '',
  serverURL: '',
  javascriptKey: '',
  masterKey: ''
});
```

* `applicationId` (required): it's the `applicationId` of your Moralis Dapp.
* `serverURL` (required): it's the URL of your LiveQuery server.
* `javascriptKey`&#x20;
* `masterKey`&#x20;

{% hint style="info" %}
`javascriptKey` and `masterKey` are used for verifying the `LiveQueryClient` when it tries to connect to the LiveQuery server. If you set them, they should match your Moralis app. You can check the LiveQuery protocol [here](https://github.com/parse-community/parse-server/wiki/Moralis-LiveQuery-Protocol-Specification) for more details.
{% endhint %}

### Open

```javascript
client.open();
```

After you call this, the `LiveQueryClient` will try to send a connect request to the LiveQuery server.

### Subscribe

```javascript
let query = new Moralis.Query('Game');
let subscription = client.subscribe(query, sessionToken);
```

* `query` (required): it is the `MoralisQuery` you'll want to subscribe.
* `sessionToken`: If you provide the `sessionToken`, the LiveQuery server will only send updates to clients whose sessionToken is fit for the `MoralisObject`s ACL. You can check the LiveQuery protocol [here](https://github.com/parse-community/parse-server/wiki/Moralis-LiveQuery-Protocol-Specification) for more details.

The `subscription` you get is the same `subscription` you receive from our Standard API. You can check our [Standard API](live-queries.md) about how to use the `subscription` to get events.

### Unsubscribe

```javascript
client.unsubscribe(subscription);
```

*   `subscription` (required):  It's the subscription you want to unsubscribe from.

    After you call this, you won't get any events from the subscription object.

### Close

```javascript
client.close();
```

This function will close the WebSocket connection to this `LiveQueryClient`, cancel the auto-reconnect, and unsubscribe all subscriptions based on it.

## Event Handling

We expose three events to help you monitor the status of the `LiveQueryClient`.

### Open Event

```javascript
client.on('open', () => {
  console.log('connection opened');
});
```

When we establish the WebSocket connection to the LiveQuery server, you'll get this event.

### Close Event

```javascript
client.on('close', () => {
  console.log('connection closed');
});
```

When we lose the WebSocket connection to the LiveQuery server, you'll get this event.

### Error Event

```javascript
client.on('error', (error) => {
  console.log('connection error');
});
```

When a network error or LiveQuery server error happens, you'll get this event.

## Reconnect

Since the whole LiveQuery feature relies on the WebSocket connection to the LiveQuery server, we always try to maintain an open WebSocket connection.&#x20;

Thus, when the connection is lost to the LiveQuery server, we try to auto-reconnect. We do [exponential back off](https://en.wikipedia.org/wiki/Exponential\_backoff) under the hood.&#x20;

However, if the WebSocket connection is closed due to `Moralis.LiveQuery.close()` or `client.close()`, we'll cancel the auto-reconnect.

## SessionToken

We send `sessionToken` to the LiveQuery server when you subscribe to a `MoralisQuery`. For the standard API, we use the `sessionToken` of the current user by default. For the advanced API, you can use any `sessionToken` when you subscribe to a `MoralisQuery`. An important thing to be aware of is when you log out or the `sessionToken` you are using is invalid, you should unsubscribe from the subscription and subscribe to the `MoralisQuery` again. Otherwise, you may face a security issue since you'll get events that shouldn't be sent to you.
