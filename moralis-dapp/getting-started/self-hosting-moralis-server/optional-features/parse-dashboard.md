---
title: "Parse Dashboard"
slug: "./parse-dashboard"
description: "This guide will teach you how to set up your own Parse Dashboard."
sidebar_position: 1
---

{% hint style="info" %}

This guide will teach you how to **set up your own [Parse Dashboard](https://github.com/parse-community/parse-dashboard)**.

{% endhint %}

The data stored in your MongoDB will be automatically synced and shown here:

![](/img/content/dashboard-1.webp)

With [MongoDB](/web3-data-api/self-hosting-moralis-server/production-environment-setup#use-mongodb-atlas) and Parse Dashboard set up, you'll have an almost exact experience to the **Moralis-hosted-server's database**:

![](/img/content/dashboard-2.webp)

## Installation

```bash npm2yarn
npm install parse-dashboard
```

## Configuration

Inside your `src` folder create a new file named `parseDashboard.ts`. Fill it with the following code:

```javascript src/parseDashboard.ts
//@ts-nocheck
import ParseDashboard from "parse-dashboard";
import config from "./config";

export const parseDashboard = new ParseDashboard(
  {
    apps: [
      {
        appName: "Moralis Server",
        serverURL: config.SERVER_URL,
        appId: config.APPLICATION_ID,
        masterKey: config.MASTER_KEY,
      },
    ],
  },
  { allowInsecureHTTP: true }
);
```

Now inside `index.ts`, import the file you just created:

```javascript src/index.ts
// @ts-ignore
import ParseServer from "parse-server";
import Moralis from "moralis";
import config from "./config";
import cors from "cors";
import express from "express";
import http from "http";
import ngrok from "ngrok";
import { parseServer } from "./parseServer";
import { streamsSync } from "@moralisweb3/parse-server";

// Import parseDashboard.ts //
import { parseDashboard } from "./parseDashboard";

//.....
```

Then create a new route to access your dashboard:

```javascript src/index.ts
//......

app.use(
  streamsSync(parseServer, {
    apiKey: config.MORALIS_API_KEY,
    webhookUrl: "/streams",
  })
);
app.use(`/server`, parseServer.app);

// Add the new route //
app.use(`/dashboard`, parseDashboard);

//......
```

## Build and run

Build the project:

```bash npm2yarn
npm run build
```

And then run it locally:

```bash npm2yarn
npm run start
```

## Accessing

Head over to **<http://localhost:1337/dashboard/>** to access your **self-hosted dashboard**:

![](/img/content/dashboard-3.webp)

![](/img/content/dashboard-4.webp)

## Securing access

{% hint style="info" %}

You can protect your dashboard from being accessed by the public by adding `username`and `password`.

{% endhint %}

Replace all the code in `parseDashboard.ts` for the following:

```javascript src/parseDashboard.ts
//@ts-nocheck
import ParseDashboard from "parse-dashboard";
import config from "./config";

export const parseDashboard = new ParseDashboard(
  {
    apps: [
      {
        appName: "Moralis Server",
        serverURL: config.SERVER_URL,
        appId: config.APPLICATION_ID,
        masterKey: config.MASTER_KEY,
      },
    ],
    users: [
      {
        user: "username123",
        pass: "password123",
      },
    ],
  },
  { allowInsecureHTTP: true }
);
```

Where `user` will be your username and `pass` will be your chosen password.

To test this you have to **rebuild** and **restart** your local server:

```bash npm2yarn
npm run build
```

```bash npm2yarn
npm run start
```

{% hint style="success" %}

`username` and `password` will now be required when accessing **<http://localhost:1337/dashboard/>**.

{% endhint %}

![](/img/content/6f65c0b-image.webp)
