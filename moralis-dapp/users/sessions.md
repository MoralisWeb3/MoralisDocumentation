---
description: Managing Sessions.
---

# Sessions

Sessions represent an instance of a user logged into a device. Sessions are automatically created when users log in or sign up. They are automatically deleted when users log out. There is one distinct `Session` object for each user-installation pair; if a user issues a login request from a device they're already logged into, that user's previous `Session` object for that Installation is automatically deleted. `Session` objects are stored on Moralis in the Session class, and you can view them on the Moralis Dashboard Data Browser. We provide a set of APIs to manage `Session` objects in your app.

`Session` is a subclass of a Moralis `Object`, so you can query, update, and delete sessions in the same way that you manipulate normal objects on Moralis. Because Moralis Server automatically creates sessions when you log in or sign up users, you should not manually create `Session` objects unless you're building a "Moralis for IoT" app (e.g. Arduino or Embedded C). Deleting a `Session` will log the user out of the device that's currently using this session's token.

Unlike other Moralis objects, the `Session` class does not have cloud code triggers. So you cannot register a `beforeSave` or `afterSave` handler for the Session class.

## `Session` Properties

The `Session` object has these special fields:

* `sessionToken` (readonly): String token for authentication on Moralis API requests. In the response of `Session` queries, only your current `Session` object will contain a session token.
* `user`: (readonly) Pointer to the `User` object that this session is for.
* `createdWith` (readonly): Information about how this session was created (e.g. `{ "action": "login", "authProvider": "password"}`).
  * `action` could have values: `login`, `signup`, `create`, or `upgrade`. The `create` action is when the developer manually creates the session by saving a `Session` object.  The `upgrade` action is when the user is upgraded to a revocable session from a legacy session token.
  * `authProvider` could have values: `password`, `anonymous`, `facebook`, or `twitter`.
* `expiresAt` (readonly): Approximate UTC date when this `Session` object will be automatically deleted. You can configure session expiration settings (either 1-year inactivity expiration or no expiration) in your app's Moralis Dashboard settings page.
*   `installationId` (can be set only once): String referring to the `Installation` where the session is logged in from. For Moralis SDKs, this field will be automatically set when users log in or sign up.

    All special fields except `installationId` can only be set automatically by Moralis Server. You can add custom fields onto `Session` objects, but please keep in mind that any logged-in device (with session token) can read other sessions that belong to the same user (unless you disable Class-Level Permissions, see below).

## Handling Invalid Session Token Error

With revocable sessions, your current session token could become invalid if its corresponding `Session` object is deleted from your Moralis Server. This could happen if you implement a Session Manager UI that lets users log out of other devices, or if you manually delete the session via Cloud Code, REST API, or Data Browser. Sessions could also be deleted due to automatic expiration (if configured in app settings). When a device's session token no longer corresponds to a `Session` object on your Moralis Server, all API requests from that device will fail with “Error 209: invalid session token”.

To handle this error, we recommend writing a global utility function that is called by all of your Moralis request error callbacks. You can then handle the "invalid session token" error in this global function. You should prompt the user to log in again so that they can obtain a new session token. This code could look like this:

```javascript
function handleMoralisError(err) {
  switch (err.code) {
    case Moralis.Error.INVALID_SESSION_TOKEN:
      Moralis.User.logOut();
      // If web browser, render a log in screen
      // If Express.js, redirect the user to the log in route
      break;

    // Other Moralis API errors that you want to explicitly handle
  }
}

// For each API request, call the global error handler
query.find().then(function() {
  // do stuff
}, function(err) {
  handleMoralisError(err);
});
```

## `Session`  Security

`Session` objects can only be accessed by the user specified in the user field. All `Session` objects have an ACL that is read and write by that user only. You cannot change this ACL. This means querying for sessions will only return objects that match the currently logged-in user.

When you log in a user via a `User` login method, Moralis will automatically create a new unrestricted `Session` object in your Moralis Server. Same for signups and Facebook/Twitter logins.

You can configure Class-Level Permissions (CLPs) for the `Session` class just like other classes on Moralis. CLPs restrict reading/writing of sessions via the `Session` API, but do not restrict Moralis Server's automatic session creation/deletion when users log in, sign up, and log out. We recommend that you disable all CLPs not needed by your app. Here are some common use cases for `Session` CLPs:

* **Find**, **Delete** — Useful for building a UI screen that allows users to see their active session on all devices, and log out of sessions on other devices. If your app does not have this feature, you should disable these permissions.
* **Create** — Useful for apps that provide user sessions for other devices from the phone app. You should disable this permission when building apps for mobile and web. For IoT apps, you should check whether your IoT device actually needs to access user-specific data. If not, then your IoT device does not need a user session, and you should disable this permission.
* **Get**, **Update**, **Add Field** — Unless you need these operations, you should disable these permissions.
