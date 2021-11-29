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

## Request weights

All Moralis plans have generous limits on the amount of requests you can make per month. How many included requests you have depends on the plan you have, check the [pricing page](https://moralis.io/pricing) for more details.&#x20;

In most cases one request to the Speedy Nodes or the Web3 API counts as one request towards your monthly quota or allowed rate limit.

However, some Speedy Node methods and API requests are very computationally expensive and therefore count as several requests.&#x20;

By giving some heavy requests higher weight we ensure that you ONLY pay for what you use and not a cent more - period! This way you get cheap requests for most use-cases while we can protect our systems from abuse by weighing the computationally expensive endpoints.

See the tables below for details about Speedy Node methods and API Endpoints that are weighted.&#x20;

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
| /{address}/nft/{token\_address}                | 5 requests  |
| /{address}/nft/transfers                       | 5 requests  |
| /nft/{address}/transfers                       | 5 requests  |
| /nft/{address}/owners                          | 5 requests  |
| /nft/{address}                                 | 5 requests  |
| /nft/{address}/metadata                        | 5 requests  |
| /nft/transfers                                 | 5 requests  |
| /nft/search                                    | 5 requests  |
| uploadIPFSFolder                               | 25 requests |
