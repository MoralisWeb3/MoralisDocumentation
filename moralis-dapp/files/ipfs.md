---
description: Moralis Has Native Support for IPFS!
---

# IPFS

{% embed url="https://youtu.be/c9SalynPw-g" %}

## Saving Files

[IPFS ](https://ipfs.io)is supported out of the box when using Moralis.

You can upload files with the _saveIPFS()_ method (max file size 1 GB).

```javascript
// Save file input to IPFS
const data = fileInput.files[0];
const file = new Moralis.File(data.name, data);
await file.saveIPFS();

//console.log(file.ipfs(), file.hash())

// Save file reference to Moralis
const jobApplication = new Moralis.Object("Applications");
jobApplication.set("name", "Satoshi");
jobApplication.set("resume", file);
await jobApplication.save();

// Retrieve file
const query = new Moralis.Query("Applications");
query.equalTo("name", "Satoshi");
query.find().then(function ([application]) {
  const ipfs = application.get("resume").ipfs();
  const hash = application.get("resume").hash();
  console.log("IPFS url", ipfs);
  console.log("IPFS hash", hash);
});
```

The data is automatically pinned.

## Saving Objects

You can also upload JSON objects directly from JavaScript, by saving the base64 string, Moralis will automatically create the buffer from the base64 provided:

```javascript
const object = {
  key: "value",
};
const file = new Moralis.File("file.json", {
  base64: btoa(JSON.stringify(object)),
});
await file.saveIPFS();
```

By uploading base64, you could also upload other base64 encoded files such as images.

{% tabs %}
{% tab title="JS" %}
```javascript
const image = "data:image/png;base64,iVBORw0KGgoAAA....";
const file = new Moralis.File("image.png", { base64: image });
await file.saveIPFS();
```
{% endtab %}

{% tab title="React" %}
```javascript
const { saveFile } = useMoralisFile();

const uploadFile = () => {
  const base64 = "V29ya2luZyBhdCBQYXJzZSBpcyBncmVhdCE=";
  saveFile(
    "myfile.txt",
    { base64 },
    {
      type: "base64",
      saveIPFS: true,
      onSuccess: (result) => console.log(result.ipfs()),
      onError: (error) => console.log(error),
    }
  );
};
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
Legacy UI is present in the video, some things might be different
{% endhint %}

{% embed url="https://youtu.be/jPa0a7-6uUw" %}

## Getting Files via the Public Gateway

An IPFS file can be retrieved with a `GET` request to a public gateway. The URL for the Moralis gateway is:

- `https://gateway.moralisipfs.com/ipfs/`.

For example, [https://gateway.moralisipfs.com/ipfs/QmUfpsyqc4hwozotRo4woyi5fJqvfcej5GcFvKiWoY6xr6](https://gateway.moralisipfs.com/ipfs/QmUfpsyqc4hwozotRo4woyi5fJqvfcej5GcFvKiWoY6xr6). A function to fetch a JSON document in IPFS from the browser could be written as the following:

```javascript
async function fetchIPFSDoc(ipfsHash) {
  const url = `https://gateway.moralisipfs.com/ipfs/${ipfsHash}`;
  const response = await fetch(url);
  return await response.json();
}
```

### Public Gateway Use-Cases

Moralis public IPFS gateway is for:

1. Displaying content on websites that was uploaded via Moralis

Moralis public IPFS gateway is not for:

1. Running scripts downloading content from IPFS. The Moralis public gateway has a JS Challenge blocking scripts for abuse protection. If your use-case requires you to run script downloading a lot of IPFS data we recommend [Infura](https://infura.io/product/ipfs) or [Pinata](https://www.pinata.cloud).
2. Accessing files on IPFS that weren't uploaded via Moralis.
