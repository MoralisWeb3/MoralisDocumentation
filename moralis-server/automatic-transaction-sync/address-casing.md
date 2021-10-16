---
description: Important note about how Moralis treats address casing.
---

# Address Casing

When syncing addresses from transactions or smart contracts, Moralis will normalize them to lowercase. This makes it easier to create queries based on address as it doesn't require creating a checksum address. Just be sure when comparing an address that comes from outside sources (like MetaMask or Etherscan), to convert it to lowercase first.

```javascript
// this is a checksum address (note the mixed cases)
let address = '0xEDe998b7BdE2467732b748613a1Aab4e5528dE15';

// In order to make queries based on the address you can convert it to lowercase
address = address.toLowerCase();

// Now we can use the address in our queries
const query = new Moralis.Query("EthTokenBalance");
query.equalTo("owner_of", address);
```
