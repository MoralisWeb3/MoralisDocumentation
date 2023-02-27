---
title: "Streams (Syncs)"
slug: "./streams"
description: "This guide will teach you how to set up streams for your self-hosted Moralis Server"
sidebar_position: 3
---

{% hint style="info" %}

## Overview

This guide will teach you how to **set up Streams** on your **self-hosted Moralis Server**.

{% endhint %}

{% hint style="info" %}

### [More Info](http://docs.moralis.io/streams-api)

Using **Streams**, you can **listen to real-time on-chain events** and automatically **synchronize incoming data** with your **database**.

{% endhint %}

**Streams** is what you need to **replace** the **old Moralis-hosted Syncs**:

![](images/streams-1.webp)

## Configuration

Open **`src/index.ts`** and make sure that `streamsSync` method gets **`config.STREAMS_WEBHOOK_URL`** as a `webhookUrl` parameter:

```javascript index.ts
//........

//Set up streamSync
app.use(
  streamsSync(parseServer, {
    apiKey: config.MORALIS_API_KEY,
    webhookUrl: config.STREAMS_WEBHOOK_URL,
  })
);

//.........
```

You can modify this parameter in your `.env` file by setting **`STREAMS_WEBHOOK_URL`** to a different `string` value. By default it's:

```shell .env
STREAMS_WEBHOOK_URL = '/streams-webhook'
```

{% hint style="info" %}

If you want to **modify** the ability to **listen to Streams** you can do so on your `.env` file by setting **`USE_STREAMS`** to `true` or `false`.

{% endhint %}

## Build and run

{% hint style="info" %}

We need to build and run the project to get the full **Streams Webhook URL** that we'll use later.

{% endhint %}

Build the project:

```bash npm2yarn
npm run build
```

And then run it locally:

```bash npm2yarn
npm run start
```

After starting your server you shoud see a similar message on your terminal. The message will contain the **Streams Webhook URL** (powered by [ngrok](https://ngrok.com/)) you will use to **create a Stream**:

```shell Terminal
Moralis Server is running on port 1337 and stream webhook url https://ba50-137-101-196-66.ngrok.io/streams-webhook
```

## Create a Stream

To create a Stream, head over to the [**following guide**](https://docs.moralis.io/docs/using-webui) or follow our [**YouTube tutorial**](https://youtu.be/o8MAFOFc7H0?t=426) from minute **7:05**.

You will need to provide the **Streams Webhook URL** you got in the previous step:

![](images/streams-2.webp)
