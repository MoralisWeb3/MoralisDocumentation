---
description: The Current User Object to Access the Logged-In User.
---

# Current User

It would be bothersome if the user had to log in every time they open your app. You can avoid this by using the cached current **`Moralis.User`** object.

{% hint style="info" %}
Note that this functionality is disabled by default on Node.js environments (such as React Native) to discourage stateful usages on server-side configurations.

To bypass this behaviour in this particular use case, call once `Moralis.User.enableUnsafeCurrentUser()` right before using any cached-user-related functionalities.
{% endhint %}

### Managing Current User

Whenever you use any signup or login methods, the user is cached in localStorage, or in any storage you configured via the **`Moralis.setAsyncStorage`** method. You can treat this cache as a session, and automatically assume the user is logged in:

```javascript
const currentUser = Moralis.User.current();
if (currentUser) {
    // do stuff with the user
} else {
    // show the signup or login page
}
```

When using a platform with an async storage system, you should call `currentAsync()` instead.

```javascript
Moralis.User.currentAsync().then(function(user) {
    // do stuff with your user
});
```

You can clear the current user by logging them out:

```javascript
Moralis.User.logOut().then(() => {
  const currentUser = Moralis.User.current();  // this will now be null
});
```

### Setting the Current User

If you’ve created your own authentication routines, or otherwise logged in as a user on the server-side, you can now pass the session token to the client and use the **`become`** method. This method will ensure the session token is valid before setting the current user.

```javascript
Moralis.User.become("session-token-here").then(function (user) {
  // The current user is now set to user.
}, function (error) {
  // The token could not be validated.
});
```

### Security For User Objects

{% hint style="success" %}
The \*\*`Moralis.User`**class is **<mark style="color:green;">**secured**</mark>:closed\_lock\_with\_key:**by default. Data stored in a**`Moralis.User`\*\*can only be read or modified by that user.
{% endhint %}

A [Cloud Function](../cloud-code/cloud-functions.md#using-the-master-key-in-cloud-code) can be used to bypass this restriction by using the `useMasterKey` option.

Specifically, you are not able to invoke any of the `save` or `delete` methods unless the `Moralis.User` was obtained using an authenticated method, like `logIn` or `signUp`. This ensures that only the user can alter their own data.

The following illustrates this security policy:

```javascript
const user = await Moralis.User.logIn("my_username", "my_password");
user.set("username", "my_new_username");
await user.save();
// This succeeds, since the user was authenticated on the device

// Get the user from a non-authenticated method
const query = new Moralis.Query(Moralis.User);
const userAgain = await query.get(user.objectId);
userAgain.set("username", "another_username");
await userAgain.save().catch(error => {
  // This will error, since the Moralis.User is not authenticated
});
```

The **`Moralis.User`** obtained from **`Moralis.User.current()`** will always be authenticated.

If you need to check if a **`Moralis.User`** is authenticated, you can invoke the `authenticated` method. You do not need to check `authenticated` with **`Moralis.User`** objects that are obtained via an authenticated method.

### Encrypt Current User

You may often want to be more careful with user information stored in the browser and if this is the case, you can encrypt the current user object:

```javascript
Moralis.enableEncryptedUser();
Moralis.secret = 'my Secrey Key';
```

{% hint style="info" %}
* Note: This function <mark style="color:red;">**will not work**</mark> if **`Moralis.secret`** is not set.
* Also, note that this <mark style="color:green;">**only works in the browser**</mark>.
{% endhint %}

Now the record in local storage looks like a random string and can only be read using **`Moralis.User.current()`**. You can check if this feature is enabled with the function **`Moralis.isEncryptedUserEnabled()`**.
