---
description: Upload multiple files and place them in a folder directory (ERC1155 Compliant)
---

# 🏪 Web3API.storage (IPFS)

## 🔥 uploadFolder (new)

Uploads multiple files and place them in a folder directory. Returns path (asynchronous).

#### Options:

- `abi`(required): Array of JSON and Base64 Supported

{% tabs %}
{% tab title="JS" %}

```javascript
const options = {
  abi: [
    {
      path: "moralis/logo.jpg",
      content:
        "iVBORw0KGgoAAAANSUhEUgAAABgAAAAYCAYAAADgdz34AAAABHNCSVQICAgIfAhkiAAAAAlwSFlzAAAApgAAAKYB3X3",
    },
  ],
};
const path = await Moralis.Web3API.storage.uploadFolder(options);
```

{% endtab %}

{% tab title="React" %}

```javascript
import React from "react";
import { useMoralisWeb3Api } from "react-moralis";

const Web3Api = useMoralisWeb3Api();

const uploadFolder = async () => {
  const options = {
    abi: [
      {
        path: "moralis/logo.jpg",
        content:
          "iVBORw0KGgoAAAANSUhEUgAAABgAAAAYCAYAAADgdz34AAAABHNCSVQICAgIfAhkiAAAAAlwSFlzAAAApgAAAKYB3X3",
      },
    ],
  };
  const path = await Web3Api.storage.uploadFolder(options);
  console.log(path);
};
```

{% endtab %}

{% tab title="curl" %}

```bash
curl -X 'POST' \
  'https://deep-index.moralis.io/api/v2/ipfs/uploadFolder' \
  -H 'accept: application/json' \
  -H 'X-API-Key: MY-API-KEY' \
  -H 'Content-Type: application/json' \
  -d '[
  {
    "path": "moralis/logo.jpg",
    "content": "iVBORw0KGgoAAAANSUhEUgAAABgAAAAYCAYAAADgdz34AAAABHNCSVQICAgIfAhkiAAAAAlwSFlzAAAApgAAAKYB3X3"
  }
]'
```

{% endtab %}

{% tab title="Unity" %}

```csharp
using MoralisUnity;
using MoralisUnity.Web3Api.Models;
using System.Collections.Generic;
using UnityEngine;

public class Example
{
    public async void fetchPairReserves()
    {
        // Define file information.
        IpfsFileRequest req = new IpfsFileRequest()
        {
            Path = "moralis/logo.jpg",
            Content = "iVBORw0KGgoAAAANSUhEUgAAABgAAAAYCAYAAADgdz34AAAABHNCSVQICAgIfAhkiAAAAAlwSFlzAAAApgAAAKYB3X3"
        };
        // Multiple requests can be sent via a List so define the request list.
        List<IpfsFileRequest> reqs = new List<IpfsFileRequest>();
        // Add requests to request list.
        reqs.Add(req);
        List<IpfsFile> resp = await Moralis.Web3Api.Storage.UploadFolder(reqs);
        foreach (IpfsFile file in resp)
        {
            Debug.Log(file.ToJson());
        }
    }
}
```

{% endtab %}
{% endtabs %}

#### Example result:

```javascript
[
  {
    path: "https://ipfs.moralis.io/QmPQ3YJ3hgfsBzJ1U4MGyV2C1GhDy6MWCENr1qMdMpKVnY/moralis/logo.jpg",
  },
];
```

### Tutorial

{% hint style="info" %}
Legacy UI is present in this video, some things might be slightly different
{% endhint %}

{% embed url="https://www.youtube.com/watch?v=VglTdr0n5ZQ" %}
**Bulk Mint NFTs on OpenSea Using IPFS folders (ERC1155 Compliant)**
{% endembed %}
