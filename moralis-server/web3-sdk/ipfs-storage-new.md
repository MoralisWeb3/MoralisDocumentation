---
description: Upload multiple files and place them in a folder directory (ERC1155 Compliant)
---

# 🔥IPFS Storage (new)

{% embed url="https://www.youtube.com/watch?v=VglTdr0n5ZQ" %}
**Bulk Mint NFTs on OpenSea Using IPFS folders (ERC1155 Compliant)**
{% endembed %}

## 🔥 uploadFolder (new)

Uploads multiple files and place them in a folder directory. Returns path (asynchronous).

#### Options:

* `abi`(required): Array of JSON and Base64 Supported

```javascript
const options = {
    abi": [
    {
      "path": "moralis/logo.jpg",
      "content": "iVBORw0KGgoAAAANSUhEUgAAABgAAAAYCAYAAADgdz34AAAABHNCSVQICAgIfAhkiAAAAAlwSFlzAAAApgAAAKYB3X3"
    }
  ]
   };
  const pairAddress = await Moralis.Web3API.defi.getPairAddress(options);
```

#### Example result:

```javascript
[
  {
    "path": "https://ipfs.moralis.io/QmPQ3YJ3hgfsBzJ1U4MGyV2C1GhDy6MWCENr1qMdMpKVnY/moralis/logo.jpg"
  }
]
```
