---
title: "Custom Cloud Code"
slug: "./custom-cloud-code"
description: "This guide will teach you how to set up your own Cloud Code."
sidebar_position: 2
---

{% hint style="info" %}

### Overview

This guide will teach you how to **set up your own [Cloud Code](https://docs.parseplatform.org/cloudcode/guide/)**.

{% endhint %}

**Custom Cloud Code** replaces the old **Moralis-hosted Cloud Functions**:

![](./images/cloud-code-1.webp)

## Configuration

{% hint style="info" %}

By default the cloud code is located in **`src/cloud/main.ts`**.

{% endhint %}

**Create a new file** inside `src/cloud/` folder and name it `cloud.ts`.

Now add the following to `src/cloud/main.ts` to import the file:

```typescript main.ts
//import cloud.ts file
import "./cloud";
```

## Adding custom cloud code

Add some **cloud functions** inside your newly created file `src/cloud/cloud.ts`:

```typescript cloud.ts
declare const Parse: any;

Parse.Cloud.define("Hello", () => {
  return `Hello! Cloud functions are cool!`;
});

Parse.Cloud.define("SayMyName", (request: any) => {
  return `Hello ${request.params.name}! Cloud functions are cool!`;
});
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

## Accessing cloud code from client

Now on a **Moralis v1 client**, you can call your **custom cloud functions** using the **Moralis v1 syntax**:

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

<Tabs>
  <TabItem value="javascript" label="Vanilla Javascript" default>

```typescript
const response = await Moralis.Cloud.run("Hello");
console.log(response);
```

  </TabItem>
  <TabItem value="react" label="React">

```typescript
const { fetch, data, error } = useMoralisCloudFunction("Hello");

<button onClick={() => fetch()}>Fetch<button>
```

  </TabItem>
</Tabs>

{% hint style="info" %}

If you don't know how to set up a **Moralis v1 client**, head over to [**_Client connection_**](../client-connection.md).

{% endhint %}

{% hint style="info" %}
You can also watch the following [**video tutorial**](custom-cloud-code.md#video-tutorial) which goes through the whole setup.
{% endhint %}

## Video tutorial

{% embed url="https://www.youtube.com/watch?v=eZeGEy33SgY" %}
{% endembed %}
