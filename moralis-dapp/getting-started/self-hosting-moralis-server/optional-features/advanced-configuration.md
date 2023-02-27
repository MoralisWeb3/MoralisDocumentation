---
title: "Advanced configuration"
slug: "./advanced-configuration"
description: "This tutorial teaches you how to configure your self-hosted Moralis Server."
sidebar_position: 4
---

{% hint style="info" %}

### Overview

This guide teaches you how to make **advanced adjustments** to your **self-hosted Moralis Server**.

{% endhint %}

## Authentication

The server is using the [Auth Api](https://docs.moralis.io/reference/auth-api-overview) to handle authentication. In order to set this up correctly, you can change the values in `src/auth/authService.ts`. Here you can configure the variables for `requestMessage` to generate the message with the correct info from your dapp.

## Rate limiting

By default, rate limiting is implemented using Redis. You can customise this behaviour by setting these values in your `.env`

- `RATE_LIMIT_TTL`: Rate limit window in seconds
- `RATE_LIMIT_AUTHENTICATED`: Rate limit requests per window for authenticated users
- `RATE_LIMIT_ANONYMOUS`: Rate limit requests per window for anonymous users

Alternatively, you can replace the `handleRateLimit` function to a custom implementation in `src/rateLimit.ts`

## Generate api proxy endpoints

The EvmApi and SolApi can be accessed in the frontend via cloud functions (this is what happens when you call `Moralis.Web3Api.<method>` in the JS SDK v1).

These cloud functions are generated when you run `yarn gen:cloud`.

Note that with the current implementation the generated cloud functions might show you some Typescript errors. This can happen when we make updates to the api that are not reflected yet in the NodeJs SDK. These can be ignored or fixed manually, please let us know when this happens. We are working on a better experience to keep these definitions in sync.

## More configuration

You can configure the server more however you want, see the [Parse Server documentation](https://docs.parseplatform.org/parse-server/guide/) for more info.
