NFT metadata refresh strategy.
The metadata contains information such as a name, a description, an image and the attributes.
They don't often change but it could be the case that for a reveal event, the contract owner decides to change the metadata of his collection.
We have an endpoint `/nft/{address}/{token_id}/metadata/resync` that allows a user to refresh an individual NFT but this might not be suitable for a collection.
We have a way that would allow us to refresh the metadata of requested NFTs. 
Here is the F.A.Q that could help you understand it.

- **What is your NFT metadata refresh strategy ?**
- This is our way to ensure that the metadata of your Favourite NFT are up to date as much as possible.

- **How doe it work?**
- When someone request the data on some specific NFTs, based on some criteria we will try to refresh the NFTs returned to the users.
Let's say we return the 25 NFTs transferred by a wallet, if applicable we will try to automatically update the metadata of those 25 NFTs. Shortly after the request, they will have their token_uri and metadata updated.

- **What are the criteria for refreshing an NFT ?**
- Glad you asked. If the NFT metadata point to an IPFS url, they can be updated once every 10 minutes. If they point to a non IPFS url, then they will have to be whitelisted. Those collections can then be refreshed once every 30 minutes.

- **What endpoints will trigger an automatique refresh ?**
- At the moment this is the list of the of endpoints that will trigger the refresh.
  - /nft/{address}/{token_id}
  - /nft/{address}/{token_id}/owners
  - /nft/{address}/{token_id}/transfers
  - /{address}/nft
  - /nft/{address}
  - /nft/{address}/transfers
  - /{address}/nft/transfers

- **What is the list of whitelisted non IPFS NFTs ?**
- We will update this FAQ and provide the link to the whitelisted collections shortly.

- **Is there a way to add my favourite non IPFS NFT collection to this list ?**
- Yes, you can submit your request [here](https://forum.moralis.io/c/moralis/non-ipfs-collection-refresh-whitelisting/24).

- **How long does it take to see the change after the request?**
- The time can vary and it will depends on the numbers of NFTs to be refreshed but usually you can see the change in less than 1 minute after the NFT being requested.

- **What are the chains supported ?**
- All our EVM compatible chains are supported.
