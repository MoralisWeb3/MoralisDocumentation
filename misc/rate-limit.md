---
description: Important note regarding rate limits when using Moralis Services.
---

# Billing & Rate Limits

It's important you study rate limits for your plan and the different services that you use.\
Please email hello@moralis.io if you have any questions!

## Abuse prevention&#x20;

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

## Request weights

All Moralis plans have generous limits on the amount of requests you can make per month. How many included requests you have depends on the plan you have, check the [pricing page](https://moralis.io/pricing) for more details.

In most cases one request to the Speedy Nodes or the Web3 API counts as one request towards your monthly quota or allowed rate limit.

However, some Speedy Node methods and API requests are very computationally expensive and therefore count as several requests.

By giving some heavy requests higher weight we ensure that you ONLY pay for what you use and not a cent more - period! This way you get cheap requests for most use-cases while we can protect our systems from abuse by weighing the computationally expensive endpoints.

See the tables below for details about Speedy Node methods and API Endpoints that are weighted.

### Speedy Node Requests

{% hint style="info" %}
Moralis Speedy Nodes have a requests limit of 50 requests per batch.
{% endhint %}

| Method                                   | Cost         |
| ---------------------------------------- | ------------ |
| All methods not on this list             | 1 request    |
| eth\_uninstallFilter                     | 1 request    |
| eth\_accounts                            | 1 request    |
| eth\_blockNumber                         | 1 request    |
| eth\_chainId                             | 1 request    |
| eth\_protocolVersion                     | 1 request    |
| eth\_syncing                             | 1 request    |
| net\_listening                           | 1 request    |
| net\_version                             | 1 request    |
| eth\_subscribe                           | 1 request    |
| eth\_unsubscribe                         | 1 request    |
| eth\_feeHistory                          | 1 request    |
| eth\_maxPriorityFeePerGas                | 1 request    |
| eth\_getTransactionReceipt               | 3 requests   |
| eth\_getUncleByBlockHashAndIndex         | 3 requests   |
| eth\_getUncleByBlockNumberAndIndex       | 3 requests   |
| eth\_getTransactionByBlockHashAndIndex   | 3 requests   |
| eth\_getTransactionByBlockNumberAndIndex | 3 requests   |
| eth\_getUncleCountByBlockHash            | 3 requests   |
| eth\_getUncleCountByBlockNumber          | 3 requests   |
| web3\_clientVersion                      | 3 requests   |
| web3\_sha3                               | 3 requests   |
| eth\_getBlockByNumber                    | 3 requests   |
| eth\_getStorageAt                        | 3 requests   |
| eth\_getTransactionByHash                | 3 requests   |
| trace\_get                               | 3 requests   |
| eth\_gasPrice                            | 3 requests   |
| eth\_getBalance                          | 3 requests   |
| eth\_getCode                             | 3 requests   |
| eth\_getFilterChanges                    | 3 requests   |
| eth\_newBlockFilter                      | 3 requests   |
| eth\_newFilter                           | 3 requests   |
| eth\_newPendingTransactionFilter         | 3 requests   |
| eth\_getBlockTransactionCountByHash      | 3 requests   |
| eth\_getBlockTransactionCountByNumber    | 3 requests   |
| eth\_getProof                            | 3 requests   |
| eth\_getBlockByHash                      | 3 requests   |
| trace\_block                             | 3 requests   |
| parity\_getBlockReceipts                 | 3 requests   |
| eth\_getTransactionCount                 | 3 requests   |
| eth\_call                                | 3 requests   |
| trace\_transaction                       | 3 requests   |
| eth\_getFilterLogs                       | 8 requests   |
| eth\_getLogs                             | 8 requests   |
| trace\_call                              | 8 requests   |
| trace\_callMany                          | 8 requests   |
| trace\_rawTransaction                    | 8 requests   |
| trace\_filter                            | 8 requests   |
| eth\_estimateGas                         | 9 requests   |
| debug\_traceTransaction                  | 31 requests  |
| eth\_sendRawTransaction                  | 35 requests  |
| trace\_replayTransaction                 | 398 requests |
| trace\_replayBlockTransactions           | 398 requests |

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

![Response header example.](<../.gitbook/assets/Screenshot 2021-12-15 at 15.38.46.png>)

The most important values to look at are `x-rate-limit-limit` and `x-rate-limit-throttle-limit`.

The first one tells you how many requests you are allowed to do per minute and the second one how many you can do per second.

Some heavy requests count as [several requests](https://docs.moralis.io/misc/rate-limit#request-weights).

In order to not get rate-limited pay attention to `x-rate-limit-used` and `x-rate-throttle-used`.

The way to fix this error is to upgrade your Moralis plan.

_(If you are using NFT endpoints with offset - please_ [_read this_](https://forum.moralis.io/t/nft-endpoints-temporary-offset-rate-limit/5867/16?u=ivan) _as they have temporarily different special weights)._

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
