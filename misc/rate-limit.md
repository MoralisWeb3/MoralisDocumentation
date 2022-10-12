---
description: Important note regarding rate limits when using Moralis Services.
---

# Billing & Rate Limits

It's important you study rate limits for your plan and the different services that you use.\
Please email hello@moralis.io if you have any questions!

## Abuse prevention

If you start getting Cloudflare errors such as Cloudflare error 1020 - it's most likely you are being flagged by our abuse prevention system and Cloudflare blocks your access on our behalf.

You may also be seeing other error messages.

Below are a few scenarios where you get temporarily banned.

1. If you are sending requests although your key is already rate-limited we may temporarily ban you. For example, let's say your plan allows you to do 30 requests per second. If you try to do 200 requests in the same second - you will most likely get temporarily banned.
2. Use one key per project. For example, if you create 100 free accounts and send requests using the keys from these accounts - all accounts tied to the same project will be banned.
3. If you think you are temporarily banned by mistake please email hello@moralis.io and we will help you fast.

### How to avoid getting banned?

1. Don't use more than 1 Moralis account.
2. Implement rate-limiting logic in your app so you don't try doing more requests than your plan allows.
3. Email us at hello@moralis.io if you have any questions.

## Is it allowed to create multiple accounts?

As we offer generous free plans we don't allow multiple account creation.

Only 1 free account is allowed per user - you have to upgrade to pro-plan if you need more limits that the free plan offers you.

If you build your dapp based on resources spread over several free accounts - you will be banned and your service will face unexpected downtime.

Please contact us at hello@moralis.io if you have any questions 🙌

## Automatic Sync Free Plan Limitation

On the free plan you are allowed to have 200 different syncs including all smart contract events and user syncs.

Also you can maximum sync 20,000 events across all events.

If you need more please email hello@moralis.io and we will give you an appropriate pro-plan.

## Request weights

All Moralis plans have generous limits on the amount of requests you can make per month. How many included requests you have depends on the plan you have, check the [pricing page](https://moralis.io/pricing) for more details.

In most cases one request to the Web3 API counts as one request towards your monthly quota or allowed rate limit.

However, some API requests are very computationally expensive and therefore count as several requests.

By giving some heavy requests higher weight we ensure that you ONLY pay for what you use and not a cent more - period! This way you get cheap requests for most use-cases while we can protect our systems from abuse by weighing the computationally expensive endpoints.

See the tables below for details about API Endpoints that are weighted.

### API Requests

| Path                                           | Weight      |
| ---------------------------------------------- | ----------- |
| /info/endpointWeights                          | 0 request   |
| /{address}                                     | 1 request   |
| /{address}/balance                             | 1 request   |
| /erc20/metadata                                | 1 request   |
| /erc20/metadata/symbols                        | 1 request   |
| /erc20/{address}/allowance                     | 1 request   |
| /resolve/{domain}                              | 1 request   |
| /{pair\_address}/reserves                      | 1 request   |
| /resolve/{address}/reverse                     | 1 request   |
| /web3/version                                  | 1 request   |
| /{address}/events                              | 2 requests  |
| /{address}/erc20/transfers                     | 2 requests  |
| /erc20/{address}/transfers                     | 2 requests  |
| /block/{block\_number\_or\_hash}/nft/transfers | 2 requests  |
| /nft/{address}/{token\_id}                     | 2 requests  |
| /nft/{address}/{token\_id}/transfers           | 2 requests  |
| /{address}/logs                                | 2 requests  |
| /{address}/function                            | 2 requests  |
| /{address}                                     | 2 requests  |
| /erc20/{address}/price                         | 3 requests  |
| /nft/{address}/trades                          | 4 requests  |
| /nft/{address}/lowestprice                     | 4 requests  |
| /{address}/erc20                               | 5 requests  |
| /block/{block\_number\_or\_hash}               | 5 requests  |
| /nft/search                                    | 5 requests  |
| /{address}/nft                                 | 5 requests  |
| /{address}/nft/transfers                       | 5 requests  |
| /{address}/nft/{token\_address}                | 5 requests  |
| /nft/{address}                                 | 5 requests  |
| /nft/{address}/transfers                       | 5 requests  |
| /nft/{address}/owners                          | 5 requests  |
| /nft/{address}/metadata                        | 5 requests  |
| /nft/{address}/sync                            | 5 requests  |
| /nft/{address}/{token\_id}/metadata/resync     | 5 requests  |
| /nft/transfers                                 | 5 requests  |
| /nft/{address}/{token\_id}/owners              | 20 requests |

Note: for exact rate limit values the endpoint `https://deep-index.moralis.io/api/v2/info/endpointWeights` can be used.

Note: `/nft/{address}/{token_id}/metadata/resync` has a billing cost of 5 and a rate limit cost of 25, meaning that you can call it only once per second with a free plan and twice a second with a Pro plan

example of output:

```
[
  {
    "endpoint": "getBlock",
    "path": "/block/{block_number_or_hash}",
    "price": 1
  },
  {
    "endpoint": "getContractEvents",
    "path": "/{address}/events",
    "price": 2
  },
  {
    "endpoint": "getTransactions",
    "path": "/{address}",
    "price": 1
  },
 ...
  {
    "endpoint": "endpointWeights",
    "path": "/info/endpointWeights",
    "price": 0
  }
]
```

## Why am I rate limited?

There are 2 different types of rate-limits you need to know about.

### Rate-limits when using `Moralis.Web3API.` in the SDK

#### Error 141 - Rate limit between your server and your clients

The first type of rate limit is protecting your Moralis Server from spam requests from your clients.

As you know - anyone can use the Moralis SDK and call the [Web3 API](../moralis-dapp/web3-sdk/) using your server.

Your server has built-in rate limits you can adjust that dictate how many requests different types of users can do before they are rate limited. You have full control over these rate limits and can adjust them with a few lines of code in your Cloud Code.

Read more [here](https://docs.moralis.io/moralis-dapp/web3-sdk/rate-limit).

If your clients go above the allowed rate-limit you set they will see the following error:

`Error 141: Too many requests to Web3API from this particular client, the clients needs to wait before sending more requests. This can be adjusted using Moralis.settings.setAPIRateLimit. Read More:` [`https://docs.moralis.io/moralis-dapp/web3-sdk/rate-limit`](https://docs.moralis.io/moralis-dapp/web3-sdk/rate-limit)`.`

_When this error can happen:_

Imagine a particular user using your website trying to make a large number of requests during the same minute. Your server will protect itself and reject the user. Only the particular user will be affected.

The way to fix this error is to adjust your settings like described above.

#### Error 141 - Rate limit between your Moralis server and Web3 API

When your users are calling the Web3 API from the SDK they may not be rate-limited by your server (situation described above) but they may get rate-limited because of your plan.

`Error 141: This Moralis Server is rate-limited because of the plan restrictions. See the details about the current rate and throttle limits: [RATE LIMIT INFO].`

_When this error can happen:_

You have many users not doing too many requests individually, but collectively they are doing more requests than your plan allows. For example - your plan allows 1000 request per minute. You have 100 users doing 15 requests per minute.

The way to fix this error is to upgrade your Moralis plan.

### Rate-limits when calling Web3 API using HTTP

#### Error 429 - Rate limit between your non-Moralis server and Web3 API

When you are calling Web3 API from your own non-Moralis backend you may get limited by the Web3 API.

In that case you will get `Error 429: Rate limit exceeded`.

When you call the API you can expect the response header in order to understand your rate limits.

![Response header example.](<../.gitbook/assets/headers_example.png>)

The most important values to look at are `x-rate-limit-limit` and `x-rate-limit-used`.

The first one tells you how many requests you are allowed to do per second and the second one how many requests you already did in current second.

Some heavy requests count as [several requests](https://docs.moralis.io/misc/rate-limit#request-weights).

In order to not get rate-limited pay attention to `x-rate-limit-used` to be lower than `x-rate-limit-limit`.

The way to fix this error is to upgrade your Moralis plan.

## Example of how to use cursor (nodejs)

```javascript
const Moralis = require("moralis/node");
const serverUrl = "https://server_domain:2053/server";
const appId = "app id";
const contractAddress = "contract address";
async function getAllOwners() {
  await Moralis.start({ serverUrl: serverUrl, appId: appId });
  let cursor = null;
  let owners = {};
  do {
    const response = await Moralis.Web3API.token.getNFTOwners({
      address: contractAddress,
      chain: "eth",
      limit: 500,
      cursor: cursor,
    });
    console.log(
      `Got page ${response.page} of ${Math.ceil(
        response.total / response.page_size
      )}, ${response.total} total`
    );
    for (const owner of response.result) {
      owners[owner.owner_of] = {
        amount: owner.amount,
        owner: owner.owner_of,
        tokenId: owner.token_id,
        tokenAddress: owner.token_address,
      };
    }
    cursor = response.cursor;
  } while (cursor != "" && cursor != null);

  console.log("owners:", owners, "total owners:", Object.keys(owners).length);
}

getAllOwners();
```

## Example of how to use cursor (python)

```python
import requests
import time


def get_nft_owners(offset, cursor):
    print("offset", offset)
    url = 'https://deep-index.moralis.io/api/v2/nft/<address_here>/owners?chain=polygon&format=decimal'
    if cursor:
      url = url + "&cursor=%s" % cursor

    print("api_url", url)
    headers = {
        "Content-Type": "application/json",
        "X-API-Key": "API_KEY_HERE"
    }
    statusResponse = requests.request("GET", url, headers=headers)
    data = statusResponse.json()
    print("HTTP headers:", statusResponse.headers)
    try:
        print("nr results", len(data['result']))
    except:
        print(repr(data))
        print("exiting")
        raise SystemExit

    cursor = data['cursor']
    print(data['page'], data['total'])
    return cursor


cursor = None
for j in range(0, 10):
    cursor = get_nft_owners(j*500, cursor)
    print()
    time.sleep(1.1)
```
