---
description: >-
  This section describes the step-by-step guide on using the Auth API to start authenticating your user.
---

# ❓ Step-by-step Guide

## Prerequisites

Before we get started, we recommend you to have a minimum knowledge of these tools:

1. Docker (See [Installation Guide](https://docs.docker.com/get-docker/))
2. NestJS (To learn more, see [here](https://nestjs.com/))
3. ReactJS (To learn more, see [here](https://reactjs.org/))

## 1. Git Clone Boilerplate

To start a new Create React App project with TypeScript, you can run this to clone the boilerplate:

```
git clone https://github.com/MoralisWeb3/web3-auth-api-boilerplate.git
cd web3-auth-api-boilerplate
```

The boilerplate displays an example of authenticating users with the Auth API and consists of a backend and frontend part.

## 2. Install frontend package

The frontend application is used to:
 - To sign in/sign up web2 user
 - To sign in/sign up web3 user
 - To link existing web2 user with web3 wallet address
 - To view NFTs of web3 user

Make sure to install all the packages in frontend before proceeding to the next step.

Rename `.env.example` to `.env`

In `.env`, configure `REACT_APP_MORALIS_APP_ID` and `REACT_APP_MORALIS_SERVER_URL` (Create Dapp and Obtain key [here](https://admin.moralis.io/dapps)).

{% tabs %}
{% tab title="npm" %}

```
cd frontend
npm install
```

{% endtab %}

{% tab title="yarn" %}

```
cd frontend
yarn install
```

{% endtab %}
{% endtabs %}

## 3. Start frontend application

Start the application by running the command below:

{% tabs %}
{% tab title="npm" %}

```
npm run start
```

{% endtab %}

{% tab title="yarn" %}

```
yarn start
```

{% endtab %}
{% endtabs %}

## 4. Spin up docker image

Before you get started with backend, spin up the required docker images for NestJS application to connect to. Make sure all your docker image is up and running.

{% tabs %}
{% tab title="npm" %}

```
npm run dev:up
```

{% endtab %}

{% tab title="yarn" %}

```
yarn dev:up
```

{% endtab %}
{% endtabs %}

## 5. Install backend package

The backend application mainly used for:
 - To sign in/sign up web2 user
 - To sign in/sign up web3 user
 - To link existing web2 user with web3 wallet address
 - To generate authentication token to authenticate frontend user
 - To fetch wallet addresses NFTs

Rename `.env.example` to `.env`.

In `.env`, configure `MORALIS_WEB3_API_KEY` (Obtain the key [here](https://admin.moralis.io/web3apis))

Now, let's go back to the **root directory** of the repository and run the command below:

{% tabs %}
{% tab title="npm" %}

```
cd backend
npm install
```

{% endtab %}

{% tab title="yarn" %}

```
cd backend
yarn install
```

{% endtab %}
{% endtabs %}

## 6. Start backend Application

{% tabs %}
{% tab title="npm" %}

```
npm run start:dev
```

{% endtab %}

{% tab title="yarn" %}

```
yarn start:dev
```

{% endtab %}
{% endtabs %}

## 7. Now see the Magic happens!!!

Open the application via **http://localhost:3000** in the browser and you shall see the application running.