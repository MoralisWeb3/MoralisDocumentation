---
title: "Production Environment Setup"
slug: "production-environment-setup"
sidebar_position: 2
---

:::info overview
This guide will teach you how to upgrade the **database** and **rate-limiting** features from the [Local Environment Setup](/web3-data-api/self-hosting-moralis-server/local-environment-setup) to **hosted solutions** so you have a **production-ready server**.
:::

:::caution Important
The completion of [**Local Environment Setup**](/web3-data-api/self-hosting-moralis-server/local-environment-setup) is **required** to continue.
:::

## Use MongoDB Atlas

:::note
This option can be used for local development as well.
:::

### Sign Up

[Create a new account](https://account.mongodb.com/account/register). You'll be automatically logged in afterwards.

### Create a database

Just after signing up, MongoDB offers you to create and deploy a database. **Choose your plan** and then ***Create***:

![](/img/content/database-1.webp)

### Configuration

After database creation, follow the **Security Quickstart** to set up network security controls.

Choose ***Username and Password*** and then ***Create User***:

![](/img/content/database-2.webp)

:::note
We will need the **username** and the **password** later.
:::

Now choose your environment. In this tutorial we choose ***My Local Environment*** but choose ***Cloud Environment*** for a more advanced setup. 

Both options will automatically add your **current IP Address** to the *Access List*. Add any other desired IP Address here and then choose ***Finish and Close***:

![](/img/content/database-3.webp)

:::tip
You can configurate this further under [*Network Access*](https://cloud.mongodb.com/v2/63ef51b2ca3fd8321c7a3817#/security/network/accessList).
:::

### Connection

Find your database cluster under [*Database Deployments*](https://cloud.mongodb.com/v2/63ef51b2ca3fd8321c7a3817#/clusters) and choose ***Connect***:

![](/img/content/database-4.webp)

Choose ***Connect your application***:

![](/img/content/database-5.webp)

Copy the connection string:

![](/img/content/database-6.webp)

Now set the `DATABASE_URL` in your `.env` with that string:

```shell
DATABASE_URL: 'mongodb+srv://<username>:<password>@cluster0.vok8pet.mongodb.net/?retryWrites=true&w=majority'
```

:::caution remember
Make sure to replace `<username>` and `<password>` with the created user's credentials. Also make sure that the user has read and write privileges. By default it will, but all this can be configured under [*Database Access*](https://cloud.mongodb.com/v2/63ef51b2ca3fd8321c7a3817#/security/database/users).
:::


## Use Redis Enterprise Cloud

:::note
This option can be used for local development as well.
:::

### Sign Up

[Create a new account](https://redis.com/try-free/). You'll be automatically logged in afterwards.

### Create a subscription

Under *Subscriptions*, choose ***New subscription***:

![](/img/content/redis-1.webp)

Choose your plan, enter a *Subscription name* and choose ***Create subscription***:

![](/img/content/redis-2.webp)

### Create a database

With a subscription created, now choose ***New database***:

![](/img/content/redis-3.webp)

Enter a *Database name* and choose ***Activate database***. We left all the settings as default (recommended):

![](/img/content/redis-4.webp)

### Connection

With a database created, now choose ***Connect***:

![](/img/content/redis-5.webp)

Choose *RedisInsight Desktop* and ***copy the connection URL***:

![](/img/content/redis-6.webp)

Now set the `REDIS_CONNECTION_STRING` in your `.env` with that URL:

```shell
REDIS_CONNECTION_STRING = 'redis://<username>:<password>@redis-17170.c262.us-east-1-3.ec2.cloud.redislabs.com:17170'
```

:::caution remember
Make sure to replace `<username>` and `<password>` with the user's credentials.

A `default` user is created automatically when a database is created. You can see/edit the password under the [database's configuration](https://app.redislabs.com/#/databases).

However, if you want to create a **custom username and password** from scratch, go to [*Data Access Control*](https://app.redislabs.com/#/data-access-control/users).
:::