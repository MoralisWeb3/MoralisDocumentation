---
title: "Deployment"
slug: "deployment"
sidebar_position: 6
---

{% hint style="info" %}

### Overview

This guide will teach you how to **deploy your server to a hosting service of your choice**.

{% endhint %}

{% hint style="warning" %}

### Important

The completion of [**Production Environment Setup**](https://v1docs.moralis.io/moralis-dapp/getting-started/self-hosting-moralis-server/production-environment-setup) is **required** to continue.

{% endhint %}

## Heroku

Login or sign-up to Heroku and configure your app:

Create a new app from the top menu and set your app name and preferred region where the server is hosted:

![](/img/content/41607d4-Screenshot_2022-09-08_at_02.42.11.webp)

![](/img/content/d4bb8cc-Screenshot_2022-09-08_at_02.42.38.webp)

Deploy your by connecting via Github or using the Heroku CLI. This will import your code to heroku and will automatically rebuild your app when changes are pushed to your repo.

![](/img/content/00b9508-Screenshot_2022-09-08_at_02.43.11.webp)

As a final step you need to set your environment variables.

Navigate to "Settings" and reveal the keys. Here you can paste all the variables from your `.env` file. Make sure that these variables are production-ready (no references to localhost, and different database that used in local development)

![](/img/content/914ac14-Screenshot_2022-09-08_at_02.44.26.webp)

![](/img/content/6f65c9c-Screenshot_2022-09-08_at_02.44.49.webp)

{% hint style="info" %}

Make sure to update `SERVER_URL` in the config vars to the deployed url on Heroku. This looks something like `moralis-demo.herokuapp.com/server`. Also make sure to set this same serverUrl in your frontend.

{% endhint %}

{% hint style="success" %}

You are now **self-hosting** your own **Moralis Server**!

{% endhint %}

## AWS Elastic Beanstalk

### Prerequisites

- Create an [AWS account](https://portal.aws.amazon.com/gp/aws/developer/registration/index.html?refid=349e66be-cf8d-4106-ae2c-54262fc45524).
- Create an [IAM user](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users_create.html).

### One-click deployment

{% hint style="warning" %}

Be aware that this process **could resort to expenses** as **Elastic Beanstalk** is not part of [AWS Free Tier](https://aws.amazon.com/es/free/).

{% endhint %}

[**Click here to deploy automatically**](https://console.aws.amazon.com/elasticbeanstalk/home?region=us-east-1#/newApplication?applicationName=demo-parse-server-migration&platform=Node.js&tierName=WebServer&environmentType=SingleInstance&sourceBundleUrl=https://moralis-s3-bucket.s3.eu-west-1.amazonaws.com/parse-server-migration.zip) and follow the **instructions below**:

Scroll down leaving all the other settings as default and choose _**Upload your code**_:

![](/img/content/31905da-image.webp)

_**Public S3 URL**_ should be automatically selected and filled. Make sure it is so and choose _**Review and launch**_:

![](/img/content/3c4f4f6-image.webp)

Scroll down leaving all the settings as default and choose _**Create app**_:

![](/img/content/a2a6af9-image.webp)

After a couple minutes the application is created but the environment is **not ready** because you need to **set your environment variables**:

![](/img/content/2d9a5c2-image.webp)

In the left navigation pane, go to **_Configuration_** and at the row where we find _Environment properties_, choose _**Edit**_:

![](/img/content/a2e78d0-image.webp)

Paste the environment variables retrieved on the [previous steps](https://docs.moralis.io/docs/run-parse-server-locally#setup-your-project) to the corresponding fields (marked in green) and choose _**Apply**_:

![](/img/content/b8e3def-image.webp)

{% hint style="info" %}

This [video](https://youtu.be/9GtysZs-FrA?t=147) also shows you how to get the **environment variables**.

{% endhint %}

Elastic Beanstalk will update your environment and after a couple of minutes it will be ready:

![](/img/content/00d25ea-image.webp)

In the left navigation pane, choose **_Go to environment_** to test it:

![](/img/content/42e04c4-image.webp)

Add **`/server`** to the URL and you'll be accessing your **_Moralis Parse Server_ hosted on AWS Elastic Beanstalk**:

![](/img/content/d4ef788-image.webp)

{% hint style="success" %}

You are now **self-hosting** your own **Moralis Server**!

{% endhint %}

## Railway

### One-click deployment

[**Click here to deploy automatically.**](https://railway.app/new/template/1c87QZ)

This will also create a MongoDB and Redis instance for you automatically, all managed by Railway. If you are using your own MongoDB/Redis instances, you can follow [Manual Deployment](https://docs.moralis.io/docs/deploy-to-production#manual-deployment-1), or use this template and just delete the MongoDB and Redis services later.

1. After your server has deployed, assign it a domain (see Step 4 of Manual Deployment).
2. Copy your MongoDB and Parse connection URL (from the "Connect" tab from Mongo).
3. Click on the "Variables" tab in your Parse Server and update:

- `SERVER_URL` with the domain from Step 1 e.g. [`https://***.up.railway.app/server`](https://***.up.railway.app/server)
- `DATABASE_URI` with your MongoDB connection URL e.g. [`mongodb://mongo:***.railway.app:****`](mongodb://mongo:***.railway.app:****/parse)
- `REDIS_CONNECTION_STRING` with your Redis connection URL e.g. [`redis://***@containers-us-west-157.railway.app:****`](redis://*@containers-us-west-157.railway.app:****)

Your server will re-deploy after these changes.

### Manual Deployment

1. Go to [Railway](https://railway.app/), click "Start a New Project" and choose "Deploy from GitHub repo". Connect your GitHub account.
2. Give permission for Railway to access your self-hosted server repository and click "Deploy Now":

![](/img/content/50e28a0-Railway_2.webp)

3. In your project's deployment page, click on the "Variables" tab and then "Raw Editor". Paste in your environment variables and click "Update Variables":

![](/img/content/43d1e9d-Railway_env_a.webp)

![](/img/content/009db76-Railway_-_env.webp)

4. Click on the "Settings" tab and under "Domains", click "Generate Domain". You can choose a different Railway domain or use your own custom domain:

![](/img/content/de66219-Railway_3.webp)

5. Copy this domain and update your `SERVER_URL` environment variable. Your project will re-deploy:

![](/img/content/2bdc4b2-Railway_4.webp)

6. After your re-deploy is successful, open your Railway project URL in your browser to test:

![](/img/content/63abd77-Railway_5.webp)

{% hint style="success" %}

You are now **self-hosting** your own **Moralis Server**!

{% endhint %}

## Google App Engine

### Prerequisites

- [Create a Google account](https://support.google.com/accounts/answer/27441?hl=en)
- [Create a Google App Engine project](https://console.cloud.google.com/projectcreate?previousPage=%2Fprojectselector2%2Fappengine%2Fcreate%3Flang%3Dpython%26;st%3Dtrue%26hl%3Des-419&organizationId=1095921968737&hl=es-419)
- [Install gcloud CLI](https://cloud.google.com/sdk/docs/install?hl=es-419)
- [Initialize gcloud CLI](https://cloud.google.com/sdk/docs/initializing?hl=es-419)

### Deployment

{% hint style="warning" %}

### Be aware!

If your [Google Cloud free trial](https://cloud.google.com/free) has ended, the following process **could resort to expenses**.

{% endhint %}

Replace the environment variables values in **`app.yaml`** with your own:

![](/img/content/161ccae-image.webp)

Open the terminal, make sure you're in the root folder and run:

```
gcloud app deploy
```

Choose your region:

![](/img/content/d8a49da-image.webp)

Type **`Y`** to continue:

![](/img/content/99306a1-image.webp)

After a couple minutes the application is deployed:

![](/img/content/87f1739-image.webp)

Now run the following command to see it on the browser:

```
gcloud app browse
```

Add **`/server`** to the URL and you'll be accessing your **_Moralis Parse Server_ hosted on Google App Engine**:

![](/img/content/7cbef58-image.webp)

{% hint style="success" %}

You are now **self-hosting** your own **Moralis Server**!

{% endhint%}
