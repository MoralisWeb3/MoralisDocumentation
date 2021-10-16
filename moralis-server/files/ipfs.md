---
description: Moralis Has Native Support for IPFS!
---

# IPFS

{% embed url="https://youtu.be/c9SalynPw-g" %}

## Configuring IPFS

⚠️ This is no longer required. IPFS is enabled by default. There used to be a tab for this in "View Details" on the server instance but this has been removed.

## Saving Files

[IPFS ](https://ipfs.io)is supported out of the box when using Moralis.

You can upload files with the _saveIPFS()_ method (max file size 64 MB).

```javascript
// Save file input to IPFS
const data = fileInput.files[0]
const file = new Moralis.File(data.name, data)
await file.saveIPFS();

//console.log(file.ipfs(), file.hash())

// Save file reference to Moralis
const jobApplication = new Moralis.Object('Applications')
jobApplication.set('name', 'Satoshi')
jobApplication.set('resume', file)
await jobApplication.save()

// Retrieve file
const query = new Moralis.Query('Applications')
query.equalTo('name', 'Satoshi')
query.find().then(function ([application]) {
   const ipfs = application.get('resume').ipfs()
   const hash = application.get('resume').hash()
   console.log('IPFS url', ipfs)
   console.log('IPFS hash', hash)
})
```

The data is automatically pinned.

## Saving Objects

You can also upload JSON objects directly from JavaScript, by saving the base64 string, Moralis will automatically create the buffer from the base64 provided:

```javascript
const object = {
    "key" : "value"
  }
const file = new Moralis.File("file.json", {base64 : btoa(JSON.stringify(object))});
await file.saveIPFS();
```

By uploading base64, you could also upload other base64 encoded files such as images.

```javascript
const image = "data:image/png;base64,iVBORw0KGgoAAA...."
const file = new Moralis.File("image.png", {base64 : image });
await file.saveIPFS();
```

{% embed url="https://youtu.be/jPa0a7-6uUw" %}

## Getting Files

An IPFS file can be retrieved with a `GET` request to a public gateway. The URL for the Moralis gateway is:

* `https://ipfs.moralis.io:2053/ipfs/`.

For example, [https://ipfs.moralis.io:2053/ipfs/QmUfpsyqc4hwozotRo4woyi5fJqvfcej5GcFvKiWoY6xr6](https://ipfs.moralis.io:2053/ipfs/QmUfpsyqc4hwozotRo4woyi5fJqvfcej5GcFvKiWoY6xr6). A function to fetch a JSON document in IPFS from the browser could be written as the following:

```javascript
async function fetchIPFSDoc(ipfsHash) {
  const url = `https://ipfs.moralis.io:2053/ipfs/${ipfsHash}`;
  const response = await fetch(url);
  return response.json();
}
```
