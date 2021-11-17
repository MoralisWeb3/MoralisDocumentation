---
description: Important note regarding rate limits when using Moralis Services.
---

# Billing & Rate Limits

It's important you study rate limits for your plan and the different services that you use.\
Please email hello@moralis.io if you have any questions!&#x20;

## Abuse prevention

As Moralis offers a free tier we have systems in place in order to prevent abuse of our systems.

Below are a few scenarios where your IP may get temporarily banned.&#x20;

1. If you are sending requests although your key is already rate-limited we may temporarily ban your IP. For example, let's say you are on the free plan and you are allowed to do 3,000 requests per minute using your key. If you try to do 100,000 requests in the same minute using the same key - you will most likely get temporarily IP-banned.
2. You are allowed to use several keys on the same IP for testing when you are way below your rate limits but it's not recommended for production as our systems may flag it as abuse. For example, if you create 100 free accounts and send requests using the keys from these accounts from the same IP - it's going to be temporarily disabled.
3. If you think you are temporarily banned by mistake please email hello@moralis.io and we will help you fast.

### How to avoid an IP ban?

1. Ideally don't use more than 1 key on the same IP.
2. Implement rate-limiting logic in your app so you don't try doing more requests than your plan allows.
3. Email us at hello@moralis.io if you have any questions.

## Request pricing

All Moralis plans have generous limits on the amount of requests you can make per month. How many included requests you have depends on the plan you have, check the [pricing page](https://moralis.io/pricing) for more details.&#x20;

It's easy to keep track of how many requests you've made, because one request to the Speedy Nodes or the Web3 API counts as one request towards your monthly quota. However, in order to prevent attacks and abuse of our APIs some of the more computationally heavy methods have a different cost where one call to the method counts as multiple requests towards your monthly quota. See the tables below for details

### Speedy Node Requests

