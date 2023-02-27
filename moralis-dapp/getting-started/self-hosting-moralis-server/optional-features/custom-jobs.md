---
title: "Custom Jobs"
slug: "./custom-jobs"
description: "This guide will teach you how to add custom jobs to your self-hosted Moralis Server"
sidebar_position: 3
---

{% hint style="info" %}

### Overview

This guide will teach you how to **add custom jobs** to your **self-hosted Moralis server**.

{% endhint %}

{% hint style="warning" %}

### Important

The completion of the [**Parse Dashboard**](parse-dashboard.md) and [**Custom Cloud Code**](custom-cloud-code.md) guides is **required** to set up custom jobs.

{% endhint %}

## Installation

Install the required package:

```bash npm2yarn
npm install parse-server-jobs-scheduler
```

## Configuration

Inside your `src/cloud/` folder, create a new file called `jobs.ts` and add the following code:

```typescript jobs.ts
declare const Parse: any;

import Scheduler from "parse-server-jobs-scheduler";
const scheduler = new Scheduler();

// Recreates all crons when the server is launched
scheduler.recreateScheduleForAllJobs();

// Recreates schedule when a job schedule has changed
Parse.Cloud.afterSave("_JobSchedule", async (request: any) => {
  scheduler.recreateSchedule(request.object.id);
});

// Destroy schedule for removed job
Parse.Cloud.afterDelete("_JobSchedule", async (request: any) => {
  scheduler.destroySchedule(request.object.id);
});
```

Import the new file inside `src/cloud/main.ts` by adding:

```typescript main.ts
//import jobs.ts file
import "./jobs";
```

## Creating a job

Create a job inside `src/cloud/cloud.ts`:

```typescript cloud.ts
declare const Parse: any;

Parse.Cloud.define("Hello", () => {
  return `Hello! Cloud functions are cool!`;
});

Parse.Cloud.define("SayMyName", (request: any) => {
  return `Hello ${request.params.name}! Cloud functions are cool!`;
});

// Creating a new job
Parse.Cloud.job("newJob", () => {
  console.log("The code inside newJob has beed executed");
  return "test";
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

## Schedule a job

Head over to <http://localhost:1337/dashboard/> and choose **_Jobs --> Schedule a job_**:

![](./images/jobs-1.webp)

Write a description and choose `newJob` on the **_Cloud job_** dropdown:

![](./images/jobs-2.webp)

Configure when you want the job to run and choose **_Schedule_**:

![](./images/jobs-3.webp)

{% hint style="success" %}

If _Start immediately_ was selected your job will start right away and repeat based on the selected time interval.

{% endhint %}

![](./images/jobs-4.webp)
