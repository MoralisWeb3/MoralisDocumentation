---
description: Learn how to manage and authenticate Users in Moralis.
---

# 👥 Users

At the core of many apps, it is of great importance that users can access their information in a highly secure manner. We provide a specialized user class called **`Moralis.User`** that automatically handles much of the functionality required for user account management.

With this class, you'll be able to add user account functionality to your app.

{% hint style="success" %}
`Moralis.User` is a subclass of [`Moralis.Object`](../database/objects.md), and has all the same features:

1. Flexible schema
2. Automatic persistence
3. A key-value interface
   {% endhint %}

All the methods that are on [`Moralis.Object`](../database/objects.md) also exist in `Moralis.User`. The difference is that `Moralis.User` has some special additions specific to user accounts.

Authentication of users can be done in different ways. You can authenticate via an [email login](email-login/) or via a [crypto wallet](web3-login.md).

Let's start by learning how to login user via their crypto wallet - no matter which wallet or blockchain they use! 🤯

### Authenticate Users

{% content-ref url="web3-login.md" %}
[web3-login.md](web3-login.md)
{% endcontent-ref %}

{% content-ref url="email-login/" %}
[email-login](email-login/)
{% endcontent-ref %}

{% content-ref url="unity-login/" %}
[unity-login](unity-login/)
{% endcontent-ref %}

### Manage Users

{% content-ref url="merging-addresses.md" %}
[merging-addresses.md](merging-addresses.md)
{% endcontent-ref %}

{% content-ref url="current-user.md" %}
[current-user.md](current-user.md)
{% endcontent-ref %}

{% content-ref url="delete-user.md" %}
[delete-user.md](delete-user.md)
{% endcontent-ref %}

{% content-ref url="sessions.md" %}
[sessions.md](sessions.md)
{% endcontent-ref %}
