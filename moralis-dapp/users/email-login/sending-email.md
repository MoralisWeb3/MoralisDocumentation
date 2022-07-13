---
description: Send Emails from your Dapp.
---

# Setup Email

### Setup email service in Dapp

**Moralis can send emails on your behalf.** We can achieve this by integrating [SendGrid](https://sendgrid.com) into the Dapp.

You would need an email service to do the following:

1. &#x20;Send Welcome emails upon user sign up.
2. Send password reset emails upon user password resetting&#x20;
3. Send verification emails for new users

### 1. Configure Email details&#x20;

Click the "View Details" button on your server instance, then the "Email Configuration" tab. You'll need to sign up for a [SendGrid](https://sendgrid.com) account and provide the following:

* **API Key**
* **From Email: T**his will appear as the "from" address on emails received by users (must be authorized by SendGrid as a single sender or domain).
* **Sendgrid Verification Email Template ID**: The template to use for the verification email.
* **Sendgrid Password Reset Template ID**: The template to use for the password reset email.

![Moralis email configuration](<../../../.gitbook/assets/SendGrid1.png>)

### 2. Apply Email Template

To enable sending a verification email, or a password reset link, some additional setup is required. Both of these operations require a [SendGrid Dynamic Template](https://sendgrid.com/solutions/email-api/dynamic-email-templates/).

![Press the "Create a Dynamic Template" button.](<../../../.gitbook/assets/Screenshot 2022-03-15 at 10.34.06 PM.png>)

![Design the look of the email. Your template must include a \{{ link \}} tag. When done press "Save".](<../../../.gitbook/assets/Screenshot 2022-03-15 at 10.40.16 PM.png>)

![After saving, it will look something like this. Copy the "Template ID" you see here into your Moralis Server Email Configuration above.](<../../../.gitbook/assets/Screenshot 2022-03-15 at 10.43.07 PM.png>)

Repeat the steps above to create templates for both email verification and password resets.

### 3. Create Dynamic Template Data

When creating dynamic templates, the following parameters are sent to the template:

`{{ link }}` -> (confirmation link or reset password link)

`{{ email }}` -> the email of the user

### 4. Send Email

Sending an email must be done on the server-side via cloud code as it requires the `MasterKey`. This is to help prevent the domain from being blacklisted for spam by bad actors.

```javascript
// in Cloud Code
Moralis.Cloud.define("sendEmailToUser", function (request) {
  Moralis.Cloud.sendEmail({
    to: request.user.get("email"),
    subject: "Fundamentals",
    html: "Pampamentally it does make sense https://youtu.be/xXrkgWDcd7c"
  });
});
```

### Tutorial

{% hint style="info" %}
Legacy UI is present in this video, some things might be slightly different
{% endhint %}

{% embed url="https://youtu.be/Q6IQRe4yUrM" %}

{% embed url="https://www.youtube.com/watch?v=PByFsb6t4Vo&ab_channel=MoralisWeb3" %}
Moralis User Email Verification using Sendrid and ReactJs
{% endembed %}
