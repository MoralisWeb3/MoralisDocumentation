---
description: Link Multiple Addresses from Multiple Blockchains to the Same User Profile.
---

# Merging Addresses

It's possible to link multiple addresses from multiple chains to the same user profile! This is done with the `Moralis.link()` function. Once an address is linked to the user, Moralis will pull all transactions, ERC20 tokens, and NFTs for those addresses as well. This way all user transactions can be gathered together in one place.

### Ethereum, BSC, Polygon

Normally a user has more than one Ethereum address. You'll want to maintain the same user session even if the user changes to another active address, known as "linking". This will add the address to the current user, allowing the user to change their active address and still have the same user profile.

```javascript
Moralis.onAccountChanged( async (accounts) => {
  const confirmed = confirm("Link this address to your account?");
  if (confirmed) {
    await Moralis.link(accounts[0]);
  }
});
```

Note, calling `link()` to an address already associated with a user will throw an error. You can see which addresses the user has already linked by querying the `user.attributes.accounts` array (which will be `undefined` if the user has not yet linked or authenticated an address).

#### How to Merge MetaMask Addresses? Web3 Programming Tutorial - Ivan on Tech

{% embed url="https://youtu.be/onzTGQcE_a0" %}

### Linking Elrond Address

Elrond accounts can be linked to existing accounts using the following code. This will prompt the user to authenticate on the ledger device.

```javascript
await Moralis.ERD.link(address)
```
