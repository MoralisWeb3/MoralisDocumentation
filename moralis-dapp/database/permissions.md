---
description: Ensure you lock down your Dapp before going to production.
---

# Permissions

## Fast development

Moralis is optimised for fast development. We want you to be able to achieve great results fast.

The default settings are therefore very permissive and you need to ensure you lock your database down before going to production.

{% hint style="success" %}
Below is a checklist of the most important steps to take before going to production. Read full [Security Docs](security.md).
{% endhint %}

## Production checklist

### 1. Client Class Creation

By default, Moralis allows any client to create new Classes in the database and add new columns to existing classes.

This is great when you are developing your Dapp as you can alter the structure of the database very fast straight from the SDK. This increases developer productivity substantially.

Of course, this kind of permission should be turned off when you go to production.

{% hint style="info" %}
[Read this](security.md#client-class-creation) to disable Client Class Creation.
{% endhint %}

### 2. Class Permissions

When you create a new Class in the database - it has no permission so that you can quickly experiment with it by reading and writing data to it from the SDK without any configuration.

This is explained when you create a new Class via the UI.

![Default Class Permission Warning](../../.gitbook/assets/Moralis\_dashboard\_create\_class.png)

This should be locked down before going to production. The default system Classes such as `User` and `Session` are locked down by default.

But for all other Classes, you need to set CLP and/or ACL policies to ensure no unauthorised users write and/or read the data.

{% hint style="info" %}
Learn by watching the [video](permissions.md#tutorial) below and reading [Security Docs](security.md).
{% endhint %}

### 3. Restricting File Uploads

By default Moralis allows any authenticated user to upload [**Files**](../files/). It's a good idea to put in logic that restricts file uploads.

If your app has no use for files they should be completely disabled. You can disable all file uploads by including the following trigger in your cloud code.

```javascript
Moralis.Cloud.beforeSaveFile((request) => {
  throw "Not Allowed";
});
```

You can do any custom logic you want in order to determine whether a file should be allowed to save or not. For example, you can analyze the `request` object in order to see which user is trying to save the file. Read more here: [**File Triggers**](../cloud-code/triggers.md#file-triggers).

## Tutorial&#x20;

{% hint style="info" %}
Legacy UI is present in the video, some things might be different
{% endhint %}

{% embed url="https://www.youtube.com/watch?v=Yd4gFQ5ppmQ" %}
CLP and ACL policies explained.
{% endembed %}

## Getting help

If you have any questions about database security or locking down your database feel free to ask in the [Forum](https://forum.moralis.io).
