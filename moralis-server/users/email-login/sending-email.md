---
description: Sending Emails from Moralis Server.
---

# Setup Email

## Server Setup

{% embed url="https://youtu.be/SY30AUb8144" %}

Moralis supports sending emails to users. Click the "View Details" button on your server instance, then the "Email Configuration" tab. You'll need to sign up for a [SendGrid](https://sendgrid.com) account and provide the following:

![](<../../../.gitbook/assets/image (86).png>)

* **API Key**
* **From Email: T**his will appear as the "from" address on emails received by users (must be authorized by SendGrid as a single sender or domain).
* **Sendgrid Verification Email Template ID**: The template to use for the verification email.
* **Sendgrid Password Reset Template ID**: The template to use for the password reset email.

### Email Template

To enable sending a verification email, or a password reset link, some additional setup is required. Both of these operations require a [SendGrid Dynamic Template](https://sendgrid.com/solutions/email-api/dynamic-email-templates/).

![Press the "Create a Dynamic Template" button.](<../../../.gitbook/assets/image (88).png>)

![Design the look of the email. Your template must include a \{{ link \}} tag. When done press "Save".](<../../../.gitbook/assets/image (89).png>)

![After saving, it will look something like this. Copy the "Template ID" you see here into your Moralis Server Email Configuration above.](<../../../.gitbook/assets/image (87).png>)

Repeat the steps above to create templates for both email verification and password resets.

### Dynamic Template Data

When creating dynamic templates, the following parameters are sent to the template:

\{{ link \}} -> (confirmation link or reset password link)

\{{ email \}} -> the email of the user

## Sending

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

{% embed url="https://youtu.be/Q6IQRe4yUrM" %}
