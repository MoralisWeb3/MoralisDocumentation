---
description: >-
  Subscribing to Queries to Get Real-Time Alerts Whenever Data in the Query
  Result Set Changes.
---

# Live Queries

## Standard API

We maintain a WebSocket connection to communicate with the Moralis LiveQuery server. When used server-side, we use the [`ws`](https://www.npmjs.com/package/ws) package and in the browser we use [`window.WebSocket`](https://developer.mozilla.org/en-US%7B%7B%20site.baseUrl%20%7D%7D/Web/API/WebSockets\_API). We think that in most cases it isn't necessary to deal with the WebSocket connection directly. Thus, we developed a simple API to let you focus on your own business logic.

## Create a Subscription

{% embed url="https://youtu.be/PZDATN_dKAM" %}

```javascript
let query = new Moralis.Query('Game');
let subscription = await query.subscribe();
```

The subscription you get is actually an event emitter. For more information on event emitter, check [here](https://nodejs.org/api/events.html). You'll get the LiveQuery events through this `subscription`. The first time you call subscribe, we'll try to open the WebSocket connection to the LiveQuery server for you.

## Event Handling

We define several types of events you'll get through a subscription object:

### Open Event

```javascript
subscription.on('open', () => {
 console.log('subscription opened');
});
```

When you call `query.subscribe()`, we send a subscribe request to the LiveQuery server. When we get the confirmation from the LiveQuery server, this event will be emitted.

When the client fails to maintain the WebSocket connection and gets disconnected to the LiveQuery server, we'll try to auto-reconnect the LiveQuery server. If we reconnect the LiveQuery server and successfully resubscribe the `MoralisQuery`, you'll also get this event.

### Create Event

```javascript
subscription.on('create', (object) => {
  console.log('object created');
});
```

When a new `MoralisObject` is created and it fulfills the `MoralisQuery` you subscribe, you'll get this event. The `object` is the `MoralisObject` which was created.

### Update Event

```javascript
subscription.on('update', (object) => {
  console.log('object updated');
});
```

When an existing `MoralisObject` fulfills the `MoralisQuery`, your subscribe is updated (The `MoralisObject` fulfills the `MoralisQuery` before and after changes), and you'll get this event. The `object` is the `MoralisObject` which was updated. Its content is the latest value of the `MoralisObject`.

### Enter Event

```javascript
subscription.on('enter', (object) => {
  console.log('object entered');
});
```

When an existing `MoralisObject`'s old value does not fulfill the `MoralisQuery` but its new value fulfills the `MoralisQuery`, you'll get this event. The `object` is the `MoralisObject` which enters the `MoralisQuery`. Its content is the latest value of the `MoralisObject`.

### Leave Event

```javascript
subscription.on('leave', (object) => {
  console.log('object left');
});
```

When an existing `MoralisObject`'s old value fulfills the `MoralisQuery` but its new value doesn't fulfill the `MoralisQuery`, you'll get this event. The `object` is the `MoralisObject` which leaves the `MoralisQuery`. Its content is the latest value of the `MoralisObject`.

### Delete Event

```javascript
subscription.on('delete', (object) => {
  console.log('object deleted');
});
```

When an existing `MoralisObject` which fulfills the `MoralisQuery` is deleted, you'll get this event. The `object` is the `MoralisObject` which is deleted.

### Close Event

```javascript
subscription.on('close', () => {
  console.log('subscription closed');
});
```

When the client loses its WebSocket connection to the LiveQuery server and we can't get any more events, you'll get this event.

## Unsubscribe

```javascript
subscription.unsubscribe();
```

If you would like to stop receiving events from a `MoralisQuery`, you can just unsubscribe the `subscription`. After that, you won't get any events from the `subscription` object.

## Close

```javascript
Moralis.LiveQuery.close();
```

When you are done using LiveQuery, you can call `Moralis.LiveQuery.close()`. This function will close the WebSocket connection to the LiveQuery server, cancel the auto-reconnect, and unsubscribe all subscriptions based on it. If you call `query.subscribe()` after this, we will create a new WebSocket connection to the LiveQuery server.

## Setup Server URL

```javascript
Moralis.liveQueryServerURL = 'ws://XXXX'
```

Most of the time you do not need to manually set this. If you have set up your `Moralis.serverURL`, we will try to extract the hostname and use `ws://hostname` as the default `liveQueryServerURL`. However, if you want to define your own `liveQueryServerURL` or use a different protocol such as `wss`, you should set it yourself.

## WebSocket Status

We expose three events to help you monitor the status of the WebSocket connection:

### Open Event

```javascript
Moralis.LiveQuery.on('open', () => {
  console.log('socket connection established');
});
```

When we establish the WebSocket connection to the LiveQuery server, you'll get this event.

### Close Event

```javascript
Moralis.LiveQuery.on('close', () => {
  console.log('socket connection closed');
});
```

When we lose the WebSocket connection to the LiveQuery server, you'll get this event.

### Error Event

```javascript
Moralis.LiveQuery.on('error', (error) => {
  console.log(error);
});
```

When a network error or LiveQuery server error happens, you'll get this event.

## Advanced API

In our standard API, we manage a global WebSocket connection for you, which is suitable for most cases. However, for some cases, for instance, when you have multiple LiveQuery servers and want to connect to all of them, a single WebSocket connection isn't enough. We've exposed the `LiveQueryClient` for these scenarios.

## LiveQueryClient

A `LiveQueryClient` is a wrapper of a standard WebSocket client. We add several useful methods to help you connect/disconnect to LiveQueryServer and subscribe/unsubscribe a `MoralisQuery` easily.

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

* `applicationId` is mandatory, it's the `applicationId` of your Moralis app.
* `serverURL` is mandatory, it's the URL of your LiveQuery server.
* `javascriptKey` and `masterKey` are optional, they are used for verifying the `LiveQueryClient` when it tries to connect to the LiveQuery server. If you set them, they should match your Moralis app. You can check the LiveQuery protocol [here](https://github.com/parse-community/parse-server/wiki/Moralis-LiveQuery-Protocol-Specification) for more details.

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

* `query` is mandatory, it is the `MoralisQuery` you'll want to subscribe.
*   `sessionToken` is optional, if you provide the `sessionToken`, when the LiveQuery server gets `MoralisObject`'s updates from moralis server, it'll try to check whether the `sessionToken` fulfills the `MoralisObject`'s ACL. The LiveQuery server will only send updates to clients whose sessionToken is fit for the `MoralisObject`s ACL. You can check the LiveQuery protocol [here](https://github.com/parse-community/parse-server/wiki/Moralis-LiveQuery-Protocol-Specification) for more details.

    The `subscription` you get is the same `subscription` you receive from our Standard API. You can check our Standard API about how to use the `subscription` to get events.

### Unsubscribe

```javascript
client.unsubscribe(subscription);
```

*   `subscription` is mandatory, it's the subscription you want to unsubscribe from.

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

Since the whole LiveQuery feature relies on the WebSocket connection to the LiveQuery server, we always try to maintain an open WebSocket connection. Thus, when we find out that we've lost connection to the LiveQuery server, we try to auto-reconnect. We do exponential back off under the hood. However, if the WebSocket connection is closed due to `Moralis.LiveQuery.close()` or `client.close()`, we'll cancel the auto-reconnect.

## SessionToken

We send `sessionToken` to the LiveQuery server when you subscribe to a `MoralisQuery`. For the standard API, we use the `sessionToken` of the current user by default. For the advanced API, you can use any `sessionToken` when you subscribe to a `MoralisQuery`. An important thing to be aware of is when you log out or the `sessionToken` you are using is invalid, you should unsubscribe from the subscription and subscribe to the `MoralisQuery` again. Otherwise, you may face a security issue since you'll get events that shouldn't be sent to you.
