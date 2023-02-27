---
title: "Production Environment Setup"
slug: "production-environment-setup"
sidebar_position: 2
---

{% hint style="info" %}

### Overview

This guide will teach you how to upgrade the **database** and **rate-limiting** features from the [Local Environment Setup](/web3-data-api/self-hosting-moralis-server/local-environment-setup) to **hosted solutions** so you have a **production-ready server**.

{% endhint %}

{% hint style="warning" %}

### Important

The completion of [**Local Environment Setup**](/web3-data-api/self-hosting-moralis-server/local-environment-setup) is **required** to continue.

{% endhint%}

## Use MongoDB Atlas

{% hint style="info" %}

This option can be used for local development as well.

{% endhint %}

### Sign Up

[Create a new account](https://account.mongodb.com/account/register). You'll be automatically logged in afterwards.

### Create a database

Just after signing up, MongoDB offers you to create and deploy a database. **Choose your plan** and then **_Create_**:

![](/img/content/database-1.webp)

### Configuration

After database creation, follow the **Security Quickstart** to set up network security controls.

Choose **_Username and Password_** and then **_Create User_**:

![](/img/content/database-2.webp)

{% hint style="info" %}

We will need the **username** and the **password** later.

{% endhint %}

Now choose your environment. In this tutorial we choose **_My Local Environment_** but choose **_Cloud Environment_** for a more advanced setup.

Both options will automatically add your **current IP Address** to the _Access List_. Add any other desired IP Address here and then choose **_Finish and Close_**:

![](/img/content/database-3.webp)

{% hint style="info" %}

You can configurate this further under [_Network Access_](https://cloud.mongodb.com/v2/63ef51b2ca3fd8321c7a3817#/security/network/accessList).

{% endhint %}

### Connection

Find your database cluster under [_Database Deployments_](https://cloud.mongodb.com/v2/63ef51b2ca3fd8321c7a3817#/clusters) and choose **_Connect_**:

![](/img/content/database-4.webp)

Choose **_Connect your application_**:

![](/img/content/database-5.webp)

Copy the connection string:

![](/img/content/database-6.webp)

Now set the `DATABASE_URL` in your `.env` with that string:

```shell
DATABASE_URL: 'mongodb+srv://<username>:<password>@cluster0.vok8pet.mongodb.net/?retryWrites=true&w=majority'
```

{% hint style="warning" %}

### Remember

Make sure to replace `<username>` and `<password>` with the created user's credentials. Also make sure that the user has read and write privileges. By default it will, but all this can be configured under [_Database Access_](https://cloud.mongodb.com/v2/63ef51b2ca3fd8321c7a3817#/security/database/users).

{% endhint %}

## Use Redis Enterprise Cloud

{% hint style="info" %}

This option can be used for local development as well.

{% endhint %}

### Sign Up

[Create a new account](https://redis.com/try-free/). You'll be automatically logged in afterwards.

### Create a subscription

Under _Subscriptions_, choose **_New subscription_**:

![](/img/content/redis-1.webp)

Choose your plan, enter a _Subscription name_ and choose **_Create subscription_**:

![](/img/content/redis-2.webp)

### Create a database

With a subscription created, now choose **_New database_**:

![](/img/content/redis-3.webp)

Enter a _Database name_ and choose **_Activate database_**. We left all the settings as default (recommended):

![](/img/content/redis-4.webp)

### Connection

With a database created, now choose **_Connect_**:

![](/img/content/redis-5.webp)

Choose _RedisInsight Desktop_ and **_copy the connection URL_**:

![](/img/content/redis-6.webp)

Now set the `REDIS_CONNECTION_STRING` in your `.env` with that URL:

```shell
REDIS_CONNECTION_STRING = 'redis://<username>:<password>@redis-17170.c262.us-east-1-3.ec2.cloud.redislabs.com:17170'
```

{% hint style="warning" %}

### Remember

Make sure to replace `<username>` and `<password>` with the user's credentials.

A `default` user is created automatically when a database is created. You can see/edit the password under the [database's configuration](https://app.redislabs.com/#/databases).

However, if you want to create a **custom username and password** from scratch, go to [_Data Access Control_](https://app.redislabs.com/#/data-access-control/users).

{% endhint %}
