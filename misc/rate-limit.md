---
description: Important note regarding rate limits when using Moralis Services.
---

# Billing & Rate Limits

It's important you study rate limits for your plan and the different services that you use.\
Please email hello@moralis.io if you have any questions!

## Abuse prevention

As Moralis offers a free tier we have systems in place in order to prevent abuse of our systems.

Below are a few scenarios where your IP may get temporarily banned.

1. If you are sending requests although your key is already rate-limited we may temporarily ban your IP. For example, let's say you are on the free plan and you are allowed to do 3,000 requests per minute using your key. If you try to do 100,000 requests in the same minute using the same key - you will most likely get temporarily IP-banned.
2. You are allowed to use several keys on the same IP for testing when you are way below your rate limits but it's not recommended for production as our systems may flag it as abuse. For example, if you create 100 free accounts and send requests using the keys from these accounts from the same IP - it's going to be temporarily disabled.
3. If you think you are temporarily banned by mistake please email hello@moralis.io and we will help you fast.

### How to avoid an IP ban?

1. Ideally don't use more than 1 key on the same IP.
2. Implement rate-limiting logic in your app so you don't try doing more requests than your plan allows.
3. Email us at hello@moralis.io if you have any questions.

## Request weights

All Moralis plans have generous limits on the amount of requests you can make per month. How many included requests you have depends on the plan you have, check the [pricing page](https://moralis.io/pricing) for more details.

In most cases one request to the Speedy Nodes or the Web3 API counts as one request towards your monthly quota or allowed rate limit.

However, some Speedy Node methods and API requests are very computationally expensive and therefore count as several requests.

By giving some heavy requests higher weight we ensure that you ONLY pay for what you use and not a cent more - period! This way you get cheap requests for most use-cases while we can protect our systems from abuse by weighing the computationally expensive endpoints.

See the tables below for details about Speedy Node methods and API Endpoints that are weighted.

### Speedy Node Requests

| Method                         | Cost         |
| ------------------------------ | ------------ |
| All methods not on this list   | 1 request    |
| eth\_estimateGas               | 10 requests  |
| eth\_sendRawTransaction        | 30 requests  |
| debug\_traceTransaction        | 30 requests  |
| trace\_replayTransaction       | 200 requests |
| trace\_replayBlockTransactions | 200 requests |

### API Requests

| Endpoint                                       | Cost        |
| ---------------------------------------------- | ----------- |
| All endpoints not on this list                 | 1 request   |
| /{address}/events                              | 2 requests  |
| /{address}/erc20/transfers                     | 2 requests  |
| /block/{block\_number\_or\_hash}/nft/transfers | 2 requests  |
| /nft/{address}/{token\_id}                     | 2 requests  |
| /nft/{address}/{token\_id}/owners              | 2 requests  |
| /nft/{address}/{token\_id}/transfers           | 2 requests  |
| /{address}/logs                                | 2 requests  |
| /transaction/{transaction\_hash}               | 2 requests  |
| /{address}/function                            | 2 requests  |
| /erc20/{address}/price                         | 3 requests  |
| /nft/{address}/lowestprice                     | 4 requests  |
| /nft/{address}/trades                          | 4 requests  |
| /{address}/erc20                               | 5 requests  |
| /{address}/nft/{token\_address}                | 5 requests  |
| /{address}/nft/transfers                       | 5 requests  |
| /nft/{address}/transfers                       | 5 requests  |
| /nft/{address}/owners                          | 5 requests  |
| /nft/{address}                                 | 5 requests  |
| /nft/{address}/metadata                        | 5 requests  |
| /nft/transfers                                 | 5 requests  |
| /nft/search                                    | 5 requests  |
| uploadIPFSFolder                               | 25 requests |

## Why am I rate limited?

There are 2 different types of rate-limits you need to know about.

### Rate-limits when using `Moralis.Web3API.` in the SDK

#### Error 141 - Rate limit between your server and your clients

The first type of rate limit is protecting your Moralis Server from spam requests from your clients.

As you know - anyone can use the Moralis SDK and call the [Web3 API](../moralis-server/web3-sdk/) using your server.

Your server has built-in rate limits you can adjust that dictate how many requests different types of users can do before they are rate limited. You have full control over these rate limits and can adjust them with a few lines of code in your Cloud Code.

Read more [here](https://docs.moralis.io/moralis-server/web3-sdk/rate-limit).

If your clients go above the allowed rate-limit you set they will see the following error:

`Error 141: Too many requests to Web3API from this particular client, the clients needs to wait before sending more requests. This can be adjusted using Moralis.settings.setAPIRateLimit. Read More:` [`https://docs.moralis.io/moralis-server/web3-sdk/rate-limit`](https://docs.moralis.io/moralis-server/web3-sdk/rate-limit)`.`

_When this error can happen:_

Imagine a particular user using your website trying to make a large number of requests during the same minute. Your server will protect itself and reject the user. Only the particular user will be affected.

The way to fix this error is to adjust your settings like described above.

#### Error 141 - Rate limit between your Moralis server and Web3 API

When your users are calling the Web3 API from the SDK they may not be rate-limited by your server (situation described above) but they may get rate-limited because of your plan.

`Error 141: This Moralis Server is rate-limited because of the plan restrictions. See the details about the current rate and throttle limits: [RATE LIMIT INFO].`

_When this error can happen:_

You have many users not doing too many requests individually, but collectively they are doing more requests than your plan allows. For example - your plan allows 1000 request per minute. You have 100 users doing 15 requests per minute.&#x20;

The way to fix this error is to upgrade your Moralis plan.

### Rate-limits when calling Web3 API using HTTP

#### Error 429 - Rate limit between your non-Moralis server and Web3 API

When you are calling Web3 API from your own non-Moralis backend you may get limited by the Web3 API.

In that case you will get `Error 429: Rate limit exceeded`.

When you call the API you can expect the response header in order to understand your rate limits.

![Response header example.](<../.gitbook/assets/Screenshot 2021-12-15 at 15.38.46.png>)

The most important values to look at are `x-rate-limit-limit` and `x-rate-limit-throttle-limit`.

The first one tells you how many requests you are allowed to do per minute and the second one how many you can do per second.

Some heavy requests count as [several requests](https://docs.moralis.io/misc/rate-limit#request-weights).

In order to not get rate-limited pay attention to `x-rate-limit-used` and `x-rate-throttle-used`.

The way to fix this error is to upgrade your Moralis plan.

_(If you are using NFT endpoints with offset - please_ [_read this_](https://forum.moralis.io/t/nft-endpoints-temporary-offset-rate-limit/5867/16?u=ivan) _as they have temporarily different special weights)._
