---
description: Intro About Users in Moralis.
---

# Intro

At the core of many apps, it is of great importance that users can access their information in a highly secure manner. We provide a specialized user class called `Moralis.User` that automatically handles much of the functionality required for user account management.

With this class, you'll be able to add user account functionality to your app.

`Moralis.User` is a subclass of [`Moralis.Object`](https://docs.moralis.io/objects), and has all the same features, such as flexible schema, automatic persistence, and a key-value interface. All the methods that are on `Moralis.Object` also exist in `Moralis.User`. The difference is that `Moralis.User` has some special additions specific to user accounts.

Authentication of users can be done in different ways. You can authenticate via an email login or let users log in via a crypto wallet.

Let's start by learning how to login user via their crypto wallet - no matter which wallet or blockchain they use! 🤯

{% content-ref url="crypto-login/" %}
[crypto-login](crypto-login/)
{% endcontent-ref %}

