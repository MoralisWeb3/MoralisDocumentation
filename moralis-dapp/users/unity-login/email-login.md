---
description: Authenticate via Email and Password.
---

# 📧 Email Authentication

Moralis allows you to authenticate your users using email and passwords.

<!-- These profile details can be later linked with [web3 wallets](../web3-login.md). -->

### Sign Up with Username

It's also possible to <mark style="color:purple;">**authenticate without a wallet**</mark> via username and password. This makes use of the built-in `MoralisUser` class.

<!-- This class extends [`Moralis.Object`](../../database/objects.md) with some extra attributes: -->

- **`username`**: the username for the user (required)
- **`password`**: the password for the user (required on signup)
- **`email`**: the email address for the user (optional)
<!--
  {% hint style="success" %}
  Use **`Moralis.User.signUp(username, password)`**to create a new user
  {% endhint %}
-->

```csharp
// This can be placed in a function or anywhere really

MoralisUser user = Moralis.Create<MoralisUser>();
user.username = "myname";
user.password = "mypass";
user.email = "myemail@gmail.com"; // optional
try {
    // this signs the user up and doesnt log them in
    await user.SignUpAsync();
    // Hooray! Let them login the app now.
}catch (Exception error) {
    // Show the error message somewhere and let the user try again.
    Debug.log("{0} Exception caught:", error);
}

```

or you can use a simpler method below

```csharp
try {
    // this signs the user up and doesnt log them in
    await Moralis.SignUpAsync("myname", "mypass");
    // Hooray! Let them login the app now.
}catch (Exception error) {
    // Show the error message somewhere and let the user try again.
    Debug.log("{0} Exception caught:", error);
}

```

{% hint style="info" %}
Note that we used the<mark style="color:purple;">**`SignUpAsync`**</mark>method, not the<mark style="color:purple;">**`save`**</mark>method. New **`MoralisUser`**'s created with a username should always be created using the<mark style="color:purple;">**`SignUpAsync`**</mark>method.<!-- Subsequent updates to a user can be done by calling<mark style="color:purple;">**`i will probably put a method here later on`**</mark> -->
{% endhint %}

### Users in Database

This call will asynchronously create a new user in your [Moralis Database](../../database/). Before it does this, it also

1. Checks to make sure that both the username and email are unique.
2. It securely hashes the password in the cloud using **`bcrypt`**.

{% hint style="warning" %}
We never store passwords in plaintext, nor will we ever transmit passwords back to the client in plaintext.
{% endhint %}

### Handle Sign Up Errors

If a signup isn’t successful, you should read the error object that is returned however, in most cases, this happens because the username or email is already being used by another user. You should clearly communicate this to your users, and ask them to try a different username.

You are free to use an email address as the username and if so, simply ask your users to enter their email into the username property — `MoralisUser` will work as normal. We’ll go over how this is handled in the reset password section.

### Log In With Username

After signing up, you can allow users to login through the \*\*`logIn`\*\*method

```csharp
var user = await Moralis.LogInAsync("myname", "mypass");
// Do stuff after successful login.
```

<!--
By default, the SDK uses the GET HTTP method. If you would like to override this and use a POST HTTP method instead, you may pass an optional boolean property in the options argument with the key **`usePost`**.

```javascript
const user = await Moralis.User.logIn("myname", "mypass", { usePost: true });
// Do stuff after successful login.
```
-->

### Get MoralisUser

After loging in, you can retrieve the current user

```csharp
 MoralisUser user = await Moralis.GetUserAsync();
```

### Logout current MoralisUser

Logs the current user out of Moralis

{% hint style="info" %}
This logs the user out of Moralis, but in the case of a wallet login, you may need to clear wallet connect cached provider and discoonect wallet connect to reset the connection
{% endhint %}

```csharp
 await Moralis.LogOutAsync();
```

### Verify Emails

{% hint style="info" %}
To use this feature, first [Setup Email Service](sending-email.md)
{% endhint %}

Enabling email verification in an application’s settings allows the application to reserve part of its experience for users with confirmed email addresses.

Email verification adds the **`emailVerified`** key to the `MoralisUser` object. When a `MoralisUser`’s `email` is set or modified, `emailVerified` is set to `false`. `Moralis`then emails the user a link which will set `emailVerified` to `true`.

There are three **`emailVerified`** states to consider:

1. **`true`** - The user <mark style="color:green;">**confirmed**</mark> his or her email address by clicking on the link Moralis emailed them. `MoralisUser`s can never have a `true` value when the user account is first created.
2. **`false`** - The user <mark style="color:red;">**did not confirm**</mark> his/her email address by clicking the link Moralis emailed them. If `emailVerified` is `false`, consider calling `fetch` on the `MoralisUser`.
3. **`undefined (missing)`**- This `MoralisUser` was created when <mark style="color:red;">**email verification was not set up**</mark> or `MoralisUser` <mark style="color:red;">**does not have an email**</mark> when signing up.

![User class in Moralis Database](<../../../.gitbook/assets/Screenshot 2022-03-15 at 1.33.58 PM.png>)

<!--
### Reset Password

{% hint style="info" %}
To use this feature, first [Setup Email Service](sending-email.md)
{% endhint %}

As you introduce passwords into a system, users will forget them. In such cases, our library provides a way to let them securely reset their password by sending an email with a reset link.

To kick off the password reset flow, ask the user for their email address, and call:

```javascript
Moralis.User.requestPasswordReset("email@example.com")
  .then(() => {
    // Password reset request was sent successfully
  })
  .catch((error) => {
    // Show the error message somewhere
    alert("Error: " + error.code + " " + error.message);
  });
```

This will attempt to match the given email with the user’s email or username field, and will send them a password reset email. By doing this, you can opt to have users use their email as their username, or you can collect it separately and store it in the email field.

The flow for password reset is as follows:

1. User requests that their password be reset by typing in their email.
2. Moralis sends an email to their address, with a special password reset link.
3. User clicks on the reset link and is directed to a special Moralis page that will allow them to type in a new password.
4. User types in a new password. Their password has now been reset to a value they specify.

{% hint style="info" %}
Note that the messaging in this flow will reference your app by the name that you specified when you created this app on Moralis.
{% endhint %}
-->

### Tutorial

You can connect your Moralis app with [**Sendgrid**](https://sendgrid.com) email service in order to send verification emails. The video below shows how to:

- Setting up email service (Sendgrid) with Moralis
- Signing up users with username and password
- Sending custom welcome emails upon creating new profiles
- Verifying emails for users
- Reset passwords for signed-up users

{% embed url="https://www.youtube.com/watch?v=PByFsb6t4Vo&ab_channel=MoralisWeb3" %}
Moralis User Email Verification using Sendrid
{% endembed %}
