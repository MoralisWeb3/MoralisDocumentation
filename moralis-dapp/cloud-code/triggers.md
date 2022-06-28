---
description: Run Code on the Server in Response to Server Events.
---

# Triggers

## Introduction

Triggers are a way to perform operations in response to particular server-side events such as when data is saved to a particular collection. These typically come in two flavors.

- Before X: trigger is called before X happens.
- After X: trigger is called after X happens.

Triggers operate against a specific collection like `EthTransactions`. For example, to update the status of an order object to "sold" when a particular NFT is transferred, you could define an `afterSave` trigger on `EthNFTTransfers`. The trigger would get called every time a new entry is added to the `EthNFTTransfers` collection, and you could check to see if it matches one of the open orders.

### Unconfirmed Transactions

Transactions on Testnet and Mainnet can take a while to be confirmed. When Moralis detects a new transaction (or event) in an unconfirmed state, these get put into the collection with `confirmed: false`. When the transaction gets confirmed, the status is updated to `confirmed: true`.

#### Consequences for Triggers

This means if you define an `afterSave` trigger on a collection then the trigger can get fired TWICE- once for `confirmed: false` and again for `confirmed: true`! This can happen for any collection with a "confirmed" property. Write your trigger callback function with this behavior in mind. If the trigger creates entries in other collections then this could result in duplicate entries, or double counting in calculations, etc.

The confirmed status can be checked by getting the object that fired the trigger using `request.object`.

```javascript
Moralis.Cloud.afterSave("EthTransactions", async function (request) {
  const confirmed = request.object.get("confirmed");
  if (confirmed) {
    // do something
  } else {
    // handle unconfirmed case
  }
});
```

#### Local Dev Chains

{% hint style="info" %}
Ganache and Hardhat only process new blocks when a transaction is made. This means if you mint a new NFT or make a token transfer these transactions could get "stuck" in a pending table. These will get moved out of pending after one confirmation.
{% endhint %}

### Tutorials

{% hint style="info" %}
Legacy UI is present in the video, some things might be different (you can no longer write Cloud code in the UI)
{% endhint %}

{% embed url="https://youtu.be/-IVGFwPILgc" %}

## Save Triggers <a href="#delete-triggers" id="delete-triggers"></a>

### beforeSave <a href="#beforesave" id="beforesave"></a>

#### IMPLEMENTING DATA VALIDATION <a href="#implementing-data-validation" id="implementing-data-validation"></a>

Another reason to run code in the cloud is to enforce a particular data format. For example, you might have both an Android and an iOS app, and you want to validate data for each of those. Rather than writing code once for each client environment, you can write it just once with cloud code.

Data validation can be done in the code of the trigger.

```javascript
Moralis.Cloud.beforeSave("Review", (request) => {
   throw "validation error";
}
});
```

If the function throws, then the `Review` object will not be saved and the client will get an error. If nothing is thrown, the object will be saved normally.

One useful tip is that even if your app has many different versions, the same version of cloud code applies to all of them. Thus, if you launch an application that doesn’t correctly check the validity of input data, you can still fix this problem by adding a validation with `beforeSave`.

#### MODIFYING OBJECTS ON SAVE <a href="#modifying-objects-on-save" id="modifying-objects-on-save"></a>

In some cases, you don’t want to throw out invalid data. You just want to tweak it a bit before saving it. `beforeSave` can handle this case, too. Any adjustment you make to request.object will be saved.

In our movie review example, we might want to ensure that comments aren’t too long. A single long comment might be tricky to display. We can use `beforeSave` to truncate the `comment` field to 140 characters:

```javascript
Moralis.Cloud.beforeSave("Review", (request) => {
  const comment = request.object.get("comment");
  if (comment.length > 140) {
    // Truncate and add a ...
    request.object.set("comment", comment.substring(0, 137) + "...");
  }
  // ...
  return request.object
});
```

#### PREDEFINED CLASSES <a href="#predefined-classes" id="predefined-classes"></a>

If you want to use `beforeSave` for a predefined class in the Moralis JavaScript SDK, you should not pass a string for the first argument. Instead, you should pass the class itself, for example:

```javascript
Moralis.Cloud.beforeSave(
  Moralis.User,
  async (request) => {
    // code here
  }
  // Validation Object or Validation Function
);
```

### afterSave <a href="#aftersave" id="aftersave"></a>

In some cases, you may want to perform some type of action, such as a push, after an object has been saved. You can do this by registering a handler with the `afterSave` method. For example, suppose you want to keep track of the number of comments on a blog post. You can do that by writing a function like this:

```javascript
const logger = Moralis.Cloud.getLogger();

Moralis.Cloud.afterSave("Comment", (request) => {
  const query = new Moralis.Query("Post");
  query
    .get(request.object.get("post").id)
    .then(function (post) {
      post.increment("comments");
      return post.save();
    })
    .catch(function (error) {
      logger.error("Got an error " + error.code + " : " + error.message);
    });
});
```

#### ASYNC BEHAVIOR <a href="#async-behavior" id="async-behavior"></a>

In the example above, the client will receive a successful response before the promise in the handler completes, regardless of how the promise resolves. For instance, the client will receive a successful response even if the handler throws an exception. Any errors that occurred while running the handler can be found in the cloud code log.

You can use an `afterSave` handler to perform lengthy operations after sending a response back to the client. In order to respond to the client before the `afterSave` handler completes, your handler may not return a promise and your `afterSave` handler may not use async/await.

#### PREDEFINED CLASSES <a href="#predefined-classes-1" id="predefined-classes-1"></a>

If you want to use `afterSave` for a predefined class in the Moralis JavaScript SDK, you should not pass a string for the first argument. Instead, you should pass the class itself, for example:

```javascript
Moralis.Cloud.afterSave(Moralis.User, async (request) => {
  // code here
});
```

### Context <a href="#context" id="context"></a>

When saving a `Moralis.Object`, you may pass a `context` dictionary that's accessible in the Cloud Code Save Triggers. More info in the [JavaScript Guide](https://docs.moralis.io/objects#cloud-code-context).

The context is also passed from a `beforeSave` handler to an `afterSave` handler. The following example sends emails to users who are being added to a Moralis.Role’s user relation asynchronously, so that the client receives a response before the emails complete sending:

```javascript
const beforeSave = function beforeSave(request) {
  const { object: role } = request;
  // Get users that will be added to the users relation.
  const usersOp = role.op("users");
  if (usersOp && usersOp.relationsToAdd.length > 0) {
    // add the users being added to the request context
    request.context = { buyers: usersOp.relationsToAdd };
  }
};

const afterSave = function afterSave(request) {
  const { object: role, context } = request;
  if (context && context.buyers) {
    const purchasedItem = getItemFromRole(role);
    const promises = context.buyers.map(emailBuyer.bind(null, purchasedItem));
    item.increment("orderCount", context.buyers.length);
    promises.push(item.save(null, { useMasterKey: true }));
    Promise.all(promises).catch(request.log.error.bind(request.log));
  }
};
```

## Delete Triggers <a href="#delete-triggers" id="delete-triggers"></a>

### beforeDelete <a href="#beforedelete" id="beforedelete"></a>

You can run custom cloud code before an object is deleted. You can do this with the `beforeDelete` method. For instance, this can be used to implement a restricted delete policy that is more sophisticated than what can be expressed through [ACLs](broken-reference). For example, suppose you have a photo album app, where many photos are associated with each album, and you want to prevent the user from deleting an album if it still has a photo in it. You can do that by writing a function like this:

```javascript
Moralis.Cloud.beforeDelete("Album", async (request) => {
  const query = new Moralis.Query("Photo");
  query.equalTo("album", request.object);
  const count = await query.count({ useMasterKey: true });
  if (count > 0) {
    throw "Can't delete album if it still has photos.";
  }
});
```

If the function throws, the `Album` object will not be deleted, and the client will get an error. Otherwise, the object will be deleted normally.

#### PREDEFINED CLASSES <a href="#predefined-classes-2" id="predefined-classes-2"></a>

If you want to use `beforeDelete` for a predefined class in the Moralis JavaScript SDK, you should not pass a string for the first argument. Instead, you should pass the class itself, for example:

```javascript
Moralis.Cloud.beforeDelete(Moralis.User, async (request) => {
  // code here
});
```

### afterDelete <a href="#afterdelete" id="afterdelete"></a>

In some cases, you may want to perform some type of action, such as a push, after an object has been deleted. You can do this by registering a handler with the `afterDelete` method. For example, suppose that after deleting a blog post, you also want to delete all associated comments. You can do that by writing a function like this:

```javascript
const logger = Moralis.Cloud.getLogger();

Moralis.Cloud.afterDelete("Post", (request) => {
  const query = new Moralis.Query("Comment");
  query.equalTo("post", request.object);
  query
    .find()
    .then(Moralis.Object.destroyAll)
    .catch((error) => {
      logger.error(
        "Error finding related comments " + error.code + ": " + error.message
      );
    });
});
```

The `afterDelete` handler can access the object that was deleted through `request.object`. This object is fully fetched, but cannot be refetched or resaved.

The client will receive a successful response to the delete request after the handler terminates, regardless of how the `afterDelete` terminates. For instance, the client will receive a successful response even if the handler throws an exception. Any errors that occurred while running the handler can be found in the cloud code log.

#### PREDEFINED CLASSES <a href="#predefined-classes-3" id="predefined-classes-3"></a>

If you want to use `afterDelete` for a predefined class in the Moralis JavaScript SDK, you should not pass a string for the first argument. Instead, you should pass the class itself, for example:

```javascript
Moralis.Cloud.afterDelete(Moralis.User, async (request) => {
  // code here
});
```

## File Triggers <a href="#file-triggers" id="file-triggers"></a>

### beforeSaveFile <a href="#beforesavefile" id="beforesavefile"></a>

With the `beforeSaveFile` method, you can run custom cloud code before any file is saved. Returning a new `Moralis.File` will save the new file instead of the one sent by the client.

#### EXAMPLES <a href="#examples" id="examples"></a>

```javascript
// Changing the file name
Moralis.Cloud.beforeSaveFile(async (request) => {
  const { file } = request;
  const fileData = await file.getData();
  const newFile = new Moralis.File("a-new-file-name.txt", { base64: fileData });
  return newFile;
});

// Returning an already saved file
Moralis.Cloud.beforeSaveFile((request) => {
  const { user } = request;
  const avatar = user.get("avatar"); // this is a Moralis.File that is already saved to the user object
  return avatar;
});

// Saving a different file from uri
Moralis.Cloud.beforeSaveFile((request) => {
  const newFile = new Moralis.File("some-file-name.txt", {
    uri: "www.somewhere.com/file.txt",
  });
  return newFile;
});
```

#### METADATA AND TAGS <a href="#metadata-and-tags" id="metadata-and-tags"></a>

Adding Metadata and Tags to your files allows you to add additional bits of data to the files that are stored within your storage solution (i.e AWS S3). The `beforeSaveFile` hook is a great place to set the metadata and/or tags on your files.

Note: not all storage adapters support metadata and tags. Check the documentation for the storage adapter you’re using for compatibility.

```javascript
// Adding metadata and tags
Moralis.Cloud.beforeSaveFile((request) => {
  const { file, user } = request;
  file.addMetadata("createdById", user.id);
  file.addTag("groupId", user.get("groupId"));
});
```

### afterSaveFile <a href="#aftersavefile" id="aftersavefile"></a>

The `afterSaveFile` method is a great way to keep track of all the files stored in your app. For example:

```javascript
Moralis.Cloud.afterSaveFile(async (request) => {
  const { file, fileSize, user } = request;
  const fileObject = new Moralis.Object("FileObject");
  fileObject.set("file", file);
  fileObject.set("fileSize", fileSize);
  fileObject.set("createdBy", user);
  const token = { sessionToken: user.getSessionToken() };
  await fileObject.save(null, token);
});
```

### beforeDeleteFile <a href="#beforedeletefile" id="beforedeletefile"></a>

You can run custom cloud code before any file gets deleted. For example, let's say you want to add logic that only allows files to be deleted by the user who created it. You could use a combination of the `afterSaveFile` and the `beforeDeleteFile` methods as follows:

```javascript
Moralis.Cloud.afterSaveFile(async (request) => {
  const { file, user } = request;
  const fileObject = new Moralis.Object('FileObject');
  fileObject.set('fileName', file.name());
  fileObject.set('createdBy', user);
  await fileObject.save(null, { useMasterKey: true );
});

Moralis.Cloud.beforeDeleteFile(async (request) => {
  const { file, user } = request;
  const query = new Moralis.Query('FileObject');
  query.equalTo('fileName', file.name());
  const fileObject = await query.first({ useMasterKey: true });
  if (fileObject.get('createdBy').id !== user.id) {
    throw 'You do not have permission to delete this file';
  }
});
```

### afterDeleteFile <a href="#afterdeletefile" id="afterdeletefile"></a>

In the above `beforeDeleteFile` example, the `FileObject` collection is used to keep track of saved files in your app. The `afterDeleteFile` trigger is a good place to clean up these objects once a file has been successfully deleted.

```javascript
Moralis.Cloud.afterDeleteFile(async (request) => {
  const { file } = request;
  const query = new Moralis.Query("FileObject");
  query.equalTo("fileName", file.name());
  const fileObject = await query.first({ useMasterKey: true });
  await fileObject.destroy({ useMasterKey: true });
});
```

## Find Triggers <a href="#find-triggers" id="find-triggers"></a>

### beforeFind <a href="#beforefind" id="beforefind"></a>

In some cases, you may want to transform an incoming query, adding an additional limit or increasing the default limit, adding extra includes, or restrict the results to a subset of keys. You can do so with the `beforeFind` trigger.

#### EXAMPLES <a href="#examples-1" id="examples-1"></a>

```javascript
// Properties available
Moralis.Cloud.beforeFind("MyObject", (req) => {
  let query = req.query; // the Moralis.Query
  let user = req.user; // the user
  let triggerName = req.triggerName; // beforeFind
  let isMaster = req.master; // if the query is run with masterKey
  let isCount = req.count; // if the query is a count operation
  let logger = req.log; // the logger
  let installationId = req.installationId; // The installationId
});

// Selecting keys
Moralis.Cloud.beforeFind("MyObject", (req) => {
  let query = req.query; // the Moralis.Query
  // Force the selection on some keys
  query.select(["key1", "key2"]);
});

// Asynchronous support
Moralis.Cloud.beforeFind("MyObject", (req) => {
  let query = req.query;
  return aPromise().then((results) => {
    // do something with the results
    query.containedIn("key", results);
  });
});

// Returning a different query
Moralis.Cloud.beforeFind("MyObject", (req) => {
  let query = req.query;
  let otherQuery = new Moralis.Query("MyObject");
  otherQuery.equalTo("key", "value");
  return Moralis.Query.or(query, otherQuery);
});

// Rejecting a query
Moralis.Cloud.beforeFind("MyObject", (req) => {
  // throw an error
  throw new Moralis.Error(101, "error");

  // rejecting promise
  return Promise.reject("error");
});

// Setting the read preference for a query
Moralis.Cloud.beforeFind("MyObject2", (req) => {
  req.readPreference = "SECONDARY_PREFERRED";
  req.subqueryReadPreference = "SECONDARY";
  req.includeReadPreference = "PRIMARY";
});
```

#### PREDEFINED CLASSES <a href="#predefined-classes-4" id="predefined-classes-4"></a>

If you want to use `beforeFind` for a predefined class in the Moralis JavaScript SDK, you should not pass a string for the first argument. Instead, you should pass the class itself, for example:

```javascript
Moralis.Cloud.beforeFind(Moralis.User, async (request) => {
  // code here
});
```

### afterFind <a href="#afterfind" id="afterfind"></a>

In some cases, you may want to manipulate the results of a query before they are sent to the client. You can do so with the `afterFind` trigger.

```javascript
Moralis.Cloud.afterFind("MyCustomClass", async (request) => {
  // code here
});
```

#### PREDEFINED CLASSES <a href="#predefined-classes-5" id="predefined-classes-5"></a>

If you want to use `afterFind` for a predefined class in the Moralis JavaScript SDK, you should not pass a string for the first argument. Instead, you should pass the class itself, for example:

```javascript
Moralis.Cloud.afterFind(Moralis.User, async (request) => {
  // code here
});
```

## Session Triggers <a href="#session-triggers" id="session-triggers"></a>

### beforeLogin <a href="#beforelogin" id="beforelogin"></a>

Sometimes you may want to run custom validation on a login request. The `beforeLogin` trigger can be used for blocking an account from logging in (for example, if they are banned), recording a login event for analytics, notifying a user by email if a login occurred at an unusual IP address and more.

```javascript
Moralis.Cloud.beforeLogin(async (request) => {
  const { object: user } = request;
  if (user.get("isBanned")) {
    throw new Error("Access denied, you have been banned.");
  }
});
```

#### SOME CONSIDERATIONS TO BE AWARE OF <a href="#some-considerations-to-be-aware-of-1" id="some-considerations-to-be-aware-of-1"></a>

- It waits for any promises to resolve.
- The user is not available on the request object - the user has not yet been provided a session until after `beforeLogin` is successfully completed.
- Like `afterSave` on `Moralis.User`, it will not save mutations to the user unless explicitly saved.

**THE TRIGGER WILL RUN…**

- On username & password logins.
- On `authProvider` logins.

**THE TRIGGER WON’T RUN…**

- On sign up.
- If the login credentials are incorrect.

### afterLogout <a href="#afterlogout" id="afterlogout"></a>

Sometimes you may want to run actions after a user logs out. For example, the `afterLogout` trigger can be used for clean-up actions after a user logs out. The triggers contain the session object that has been deleted on logout. From this session object, you can determine the user who logged out to perform user-specific tasks.

```javascript
Moralis.Cloud.afterLogout(async (request) => {
  const { object: session } = request;
  const user = session.get("user");
  user.set("isOnline", false);
  user.save(null, { useMasterKey: true });
});
```

#### SOME CONSIDERATIONS TO BE AWARE OF <a href="#some-considerations-to-be-aware-of-2" id="some-considerations-to-be-aware-of-2"></a>

- As with with `afterDelete` triggers, the `_Session` object that is contained in the request has already been deleted.

**THE TRIGGER WILL RUN…**

- When the user logs out and a `_Session` object was deleted.

#### THE TRIGGER WON’T RUN… <a href="#the-trigger-wont-run-1" id="the-trigger-wont-run-1"></a>

- If a user logs out and no `_Session` object was found to delete.
- If a `_Session` object is deleted without the user logging out by calling the logout method of an SDK.

## LiveQuery Triggers <a href="#livequery-triggers" id="livequery-triggers"></a>

### beforeConnect <a href="#beforeconnect" id="beforeconnect"></a>

You can run custom cloud code before a user attempts to connect to your LiveQuery server with the `beforeConnect` method. For instance, this can be used to only allow users that have logged in to connect to the LiveQuery server.

```javascript
Moralis.Cloud.beforeConnect((request) => {
  if (!request.user) {
    throw "Please login before you attempt to connect.";
  }
});
```

In most cases, the `connect` event is called the first time the client calls `subscribe`. If this is your use case, you can listen for errors using this event.

```javascript
const logger = Moralis.Cloud.getLogger();

Moralis.LiveQuery.on("error", (error) => {
  logger.info(error);
});
```

### beforeSubscribe <a href="#beforesubscribe" id="beforesubscribe"></a>

In some cases, you may want to transform the incoming subscription query. Examples include adding an additional limit, increasing the default limit, adding extra includes, or restricting the results to a subset of keys. You can do so with the `beforeSubscribe` trigger.

```javascript
Moralis.Cloud.beforeSubscribe("MyObject", (request) => {
  if (!request.user.get("Admin")) {
    throw new Moralis.Error(
      101,
      "You are not authorized to subscribe to MyObject."
    );
  }
  let query = request.query; // the Moralis.Query
  query.select("name", "year");
});
```

### afterLiveQueryEvent <a href="#afterlivequeryevent" id="afterlivequeryevent"></a>

In some cases, you may want to manipulate the results of a Live Query before they are sent to the client. You can do so with the `afterLiveQueryEvent` trigger.

#### EXAMPLES <a href="#examples-2" id="examples-2"></a>

```javascript
// Changing values on object and original
Moralis.Cloud.afterLiveQueryEvent("MyObject", (request) => {
  const object = request.object;
  object.set("name", "***");

  const original = request.original;
  original.set("name", "yolo");
});

// Prevent LiveQuery trigger unless 'foo' is modified
Moralis.Cloud.afterLiveQueryEvent("MyObject", (request) => {
  const object = request.object;
  const original = request.original;
  if (!original) {
    return;
  }
  if (object.get("foo") != original.get("foo")) {
    request.sendEvent = false;
  }
});
```

By default, MoralisLiveQuery does not perform queries that require additional database operations. This is to keep your Moralis Server as fast and efficient as possible. If you require this functionality, you can perform these in `afterLiveQueryEvent`.

```javascript
// Including an object on LiveQuery event, on update only.
Moralis.Cloud.afterLiveQueryEvent("MyObject", async (request) => {
  if (request.event != "update") {
    request.sendEvent = false;
    return;
  }
  const object = request.object;
  const pointer = object.get("child");
  await pointer.fetch();
});

// Extend matchesQuery functionality to LiveQuery
Moralis.Cloud.afterLiveQueryEvent("MyObject", async (request) => {
  if (request.event != "create") {
    return;
  }
  const query = request.object.relation("children").query();
  query.equalTo("foo", "bart");
  const first = await query.first();
  if (!first) {
    request.sendEvent = false;
  }
});
```

#### SOME CONSIDERATIONS TO BE AWARE OF <a href="#some-considerations-to-be-aware-of-3" id="some-considerations-to-be-aware-of-3"></a>

- Live Query events won’t trigger until the `afterLiveQueryEvent` trigger has been completed. Make sure any functions inside the trigger are efficient and restrictive to prevent bottlenecks.

### onLiveQueryEvent <a href="#onlivequeryevent" id="onlivequeryevent"></a>

Sometimes you may want to monitor Live Query Events to be used with a 3rd party such as "Datadog." The `onLiveQueryEvent` trigger can log events triggered, number of clients connected, number of subscriptions and errors.

```javascript
Moralis.Cloud.onLiveQueryEvent(
  ({
    event,
    client,
    sessionToken,
    useMasterKey,
    installationId,
    clients,
    subscriptions,
    error,
  }) => {
    if (event !== "ws_disconnect") {
      return;
    }
    // Do your magic
  }
);
```

### Events <a href="#events" id="events"></a>

- connect
- subscribe
- unsubscribe
- ws_connect
- ws_disconnect
- ws_disconnect_error

“connect” differs from “ws_connect”, the former means that the client completed the connect procedure as defined by Moralis Live Query protocol, where “ws_connect” just means that a new websocket was created.
