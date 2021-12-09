---
description: >-
  Ensure you lock down your dapp before going to production. This article
  explains the best practices.
---

# ⚠ Permissions

## Fast development

Moralis is optimised for fast development. We want you to be able to achieve great results fast.&#x20;

The default settings are therefore very permissive and you need to ensure you lock your database down before going to production.

Below is a checklist of the most important steps to take before going to production. Read full [Security Docs](security.md).

## Production checklist

### Client Class Creation

By default, Moralis allows any client to create new Classes in the database and add new columns to existing classes.&#x20;

This is great when you are developing your dapp as you can alter the structure of the database very fast straight from the SDK. This increases developer productivity substantially. &#x20;

Of course, this kind of permission should be turned off when you go to production.

[Read this](https://docs.moralis.io/moralis-server/database/security#client-class-creation) to disable Client Class Creation.

### Class Permissions

When you create a new Class in the database - it has no permission so that you can quickly experiment with it by reading and writing data to it from the SDK without any configuration.&#x20;

This is explained when you create a new Class via the UI.

![Default Class Permission Warning](<../../.gitbook/assets/Screenshot 2021-11-29 at 12.24.30.png>)

This should be locked down before going to production.

The default system Classes such as `User` and `Session` are locked down by default.

But for all other Classes you need to set CLP and/or ACL policies to ensure no unauthorised users write and/or read the data.

Learn by watching the video below and reading [Security Docs](security.md).

{% embed url="https://www.youtube.com/watch?v=Yd4gFQ5ppmQ" %}
CLP and ACL policies explained.
{% endembed %}

## Restricting File Uploads

By default Moralis allows any authenticated user to upload [Files](../files/). It's a good idea to put in logic that restricts file uploads.

If your app has no use for files they should be completely disabled. You can disable all file uploads by including the following trigger in your cloud code.

\`\`\`\`&#x20;

```javascript
// Some codeMoralis.Cloud.beforeSaveFile((request) => {
  throw "Not Allowed";
 });Moralis.Cloud.beforeSaveFile((request) => {
  throw "Not Allowed";
 });
```

&#x20;

## Getting help

If you have any questions about database security or locking down your database feel free to ask in the [Forum](https://forum.moralis.io).
