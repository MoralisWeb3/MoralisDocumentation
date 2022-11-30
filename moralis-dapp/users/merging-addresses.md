---
description: Link Multiple Addresses from Multiple Blockchains to the Same User Profile.
---

# Merging Addresses

It's possible to link multiple addresses from multiple chains to the same user profile!

This is done with the **`Moralis.link()`** function. Once an address is linked to the user, Moralis will pull all transactions, ERC20 tokens, and NFTs for those addresses as well. This way all user transactions can be gathered together in one place.

### Link Ethereum, BSC, Polygon addresses

Normally a user has more than one Ethereum address. You'll want to maintain the same user session even if the user changes to another active address, known as "linking". This will add the address to the current user, allowing the user to change their active address and still have the same user profile.

```javascript
Moralis.onAccountChanged( async (account) => {
  const confirmed = confirm("Link this address to your account?");
  if (confirmed) {
    await Moralis.link(account);
  }
});
```

{% hint style="warning" %}
Note: Calling `link()`on an address already associated with a user will throw an error.

You can see which addresses the user has already linked by querying the `user.attributes.accounts` array (will return undefined if the user has not yet linked or authenticated an address).
{% endhint %}

### Tutorial

{% embed url="https://youtu.be/onzTGQcE_a0" %}
How to Merge MetaMask Addresses? Web3 Programming Tutorial - Ivan on Tech
{% endembed %}
