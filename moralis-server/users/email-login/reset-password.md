---
description: Reset a User Password.
---

# Reset Password

As you introduce passwords into a system, users will forget them. In such cases, our library provides a way to let them securely reset their password by sending an email with a reset link.

### Server Setup

In order to send the password reset email, the email provider first needs to be set up on your Moralis Server. You'll also need to create a template. See the [Sending Email](../../tools/sending-email.md) page:

{% content-ref url="../../tools/sending-email.md" %}
[sending-email.md](../../tools/sending-email.md)
{% endcontent-ref %}

### Password Reset Request

To kick off the password reset flow, ask the user for their email address, and call:

```javascript
Moralis.User.requestPasswordReset("email@example.com")
.then(() => {
  // Password reset request was sent successfully
}).catch((error) => {
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

Note that the messaging in this flow will reference your app by the name that you specified when you created this app on Moralis.
