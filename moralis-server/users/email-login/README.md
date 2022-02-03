---
description: Authenticate via Email and Password.
---

# Web2 Authentication

Moralis allows you to authenticate your users using email and password and then connect wallets to their user profiles.

{% embed url="https://www.youtube.com/watch?v=aYi1_moT4kI" %}

### Sign Up with Username

It's also possible to authenticate without a wallet via username and password. This makes use of the built-in `Moralis.User` class. This class extends `Moralis.Object` with some extra attributes:

* **username**: the username for the user (required)
* **password**: the password for the user (required on signup)
* **email**: the email address for the user (optional)

Use `Moralis.User.signUp(username, password)` to create a new user:

```javascript
const user = new Moralis.User();
user.set("username", "my name");
user.set("password", "my pass");
user.set("email", "email@example.com");

// other fields can be set just like with Moralis.Object
user.set("phone", "415-392-0202");
try {
  await user.signUp();
  // Hooray! Let them use the app now.
} catch (error) {
  // Show the error message somewhere and let the user try again.
  alert("Error: " + error.code + " " + error.message);
}
```

This call will asynchronously create a new user in your Moralis app. Before it does this, it also checks to make sure that both the username and email are unique. Also, it securely hashes the password in the cloud using `bcrypt`. We never store passwords in plaintext, nor will we ever transmit passwords back to the client in plaintext.

Note that we used the `signUp` method, not the `save` method. New `Moralis.User`'s created with a username should always be created using the `signUp` method. Subsequent updates to a user can be done by calling `save`.

If a signup isn’t successful, you should read the error object that is returned however, in most cases, this happens because the username or email is already being used by another user. You should clearly communicate this to your users, and ask them to try a different username.

You are free to use an email address as the username and if so, simply ask your users to enter their email into the username property — `Moralis.User` will work as normal. We’ll go over how this is handled in the reset password section.

### Verifying User Email

You can connect your Moralis app with Sendgrid email service in order to send verification emails. The video below shows you how that works.

{% embed url="https://www.youtube.com/watch?v=PByFsb6t4Vo&ab_channel=MoralisWeb3" %}
Moralis User Email Verification using Sendrid
{% endembed %}



### Log In With Username

Of course, after you allow users to sign up, you need to let them log in to their account in the future. To do this, you can use the class method `logIn`.

```javascript
const user = await Moralis.User.logIn("myname", "mypass");
// Do stuff after successful login.
```

By default, the SDK uses the GET HTTP method. If you would like to override this and use a POST HTTP method instead, you may pass an optional boolean property in the options argument with the key `usePost`.

```javascript
const user = await Moralis.User.logIn("myname", "mypass", { usePost: true });
// Do stuff after successful login.
```

### Verifying Emails

Enabling email verification in an application’s settings allows the application to reserve part of its experience for users with confirmed email addresses. Email verification adds the `emailVerified` key to the `Moralis.User` object. When a `Moralis.User`’s `email` is set or modified, `emailVerified` is set to `false`. `Moralis`then emails the user a link which will set `emailVerified` to `true`.

There are three `emailVerified` states to consider:

1. `true` - the user confirmed his or her email address by clicking on the link `Moralis`emailed them. `Moralis.Users` can never have a `true` value when the user account is first created.
2. `false` - at the time the `Moralis.User` object was last refreshed, the user had not confirmed his or her email address. If `emailVerified` is `false`, consider calling `fetch` on the `Moralis.User`.
3. `missing`- the `Moralis.User` was created when email verification was off or the `Moralis.User` does not have an `email`.
