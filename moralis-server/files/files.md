---
description: Managing Files in Moralis Server.
---

# Files

## Creating a Moralis.File

{% embed url="https://youtu.be/I_wxIshq4WA" %}

`Moralis.File` lets you store application files in the cloud that would otherwise be too large or cumbersome to fit into a regular `Moralis.Object`. The most common use case is storing images, but you can also use it for documents, videos, music, and any other binary data.

Getting started with `Moralis.File` is easy. There are a couple of ways to create a file. The first is with a base64-encoded String:

{% tabs %}
{% tab title="JS" %}

```javascript
const base64 = "V29ya2luZyBhdCBQYXJzZSBpcyBncmVhdCE=";
const file = new Moralis.File("myfile.txt", { base64: base64 });
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
            onSuccess: (result) => console.log(result),
            onError: (error) => console.log(error),
        }
    );
};
```

{% endtab %}
{% endtabs %}

Alternatively, you can create a file from an array of byte values:

```javascript
const bytes = [ 0xBE, 0xEF, 0xCA, 0xFE ];
const file = new Moralis.File("myfile.txt", bytes);
```

Moralis will auto-detect the type of file you are uploading based on the file extension, but you can specify the `Content-Type` with a third parameter:

```javascript
const file = new Moralis.File("myfile.zzz", fileData, "image/png");
```

### In the Browser

In a browser, you’ll want to use an HTML form with a file upload control. To do this, create a file input tag that allows the user to pick a file from their local drive to upload:

```
<input type="file" id="profilePhotoFileUpload">
```

Then, in a click handler or other function, get a reference to that file:

> note this is using JQuery, for JS, (see vid)[https://youtu.be/I_wxIshq4WA?t=93] or getElementById 

{% tabs %}
{% tab title="JS" %}

```javascript
const fileUploadControl = $("#profilePhotoFileUpload")[0];
if (fileUploadControl.files.length > 0) {
  const file = fileUploadControl.files[0];
  const name = "photo.jpg";

  const moralisFile = new Moralis.File(name, file);
}
```

{% endtab %}
{% tab title="React" %}

```javascript
import { useState } from "react";
import { useMoralisFile } from "react-moralis";

function App() {
    const [fileTarget, setFileTarget] = useState("");
    const { saveFile } = useMoralisFile();

    const uploadFile = () => {
        saveFile("photo.jpg", fileTarget, {
            type: "image/png",
            onSuccess: (result) => console.log(result),
            onError: (error) => console.log(error),
        });
    };

    const fileInput = (e) => setFileTarget(e.target.files[0]);

    return (
        <div>
            <input type="file" onChange={fileInput} />
            <button onClick={uploadFile}>Call The Code</button>
        </div>
    );
}

export default App;
```

{% endtab %}
{% endtabs %}

Notice in this example that we give the file a name of `photo.jpg`. There are two things to note here:

* You don’t need to worry about filename collisions. Each upload gets a unique identifier so there’s no problem with uploading multiple files named `photo.jpg`.
* It’s important that you give a name to the file that has a file extension. This lets Moralis figure out the file type and handle it accordingly. So, if you’re storing PNG images, make sure your filename ends with `.png`.

Next, you’ll want to save the file to the cloud. As with `Moralis.Object`, there are many variants of the `save` method you can use depending on what sort of callback and error handling suits you.

```javascript
moralisFile.save().then(function() {
  // The file has been saved to Moralis.
}, function(error) {
  // The file either could not be read, or could not be saved to Moralis.
});
```

### In NodeJs

In NodeJs you can fetch images or other files and store them as a `Moralis.File`:

```javascript
const Moralis = require('moralis/node');
const request = require('request-promise');


const options = {
  uri: 'https://bit.ly/2zD8fgm',
  resolveWithFullResponse: true,
  encoding: null, // <-- this is important for binary data like images.
};

request(options)
  .then((response) => {
    const data = Array.from(Buffer.from(response.body, 'binary'));
    const contentType = response.headers['content-type'];
    const file = new Moralis.File('logo', data, contentType);
    return file.save();
  })
  .then((file => console.log(file.url())))
  .catch(console.error);
```

### In Objects

Finally, after the save completes, you can associate a `Moralis.File` with a `Moralis.Object` just like any other piece of data:

```javascript
const jobApplication = new Moralis.Object("JobApplication");
jobApplication.set("applicantName", "Joe Smith");
jobApplication.set("applicantResumeFile", moralisFile);
jobApplication.save();
```

{% hint style="info" %}
If you are saving Files in your cloud code you must provide the MasterKey when saving the object. Example:

```javascript
jobApplication.save(null, { useMasterKey: true });
```
{% endhint %}



## Retrieving File Contents

How to best retrieve the file contents back depends on the context of your application. Because of cross-domain request issues, it’s best if you can make the browser do the work for you. Typically, that means rendering the file’s URL into the DOM. Here we render an uploaded profile photo on a page with jQuery:

```javascript
const profilePhoto = profile.get("photoFile");
$("profileImg")[0].src = profilePhoto.url();
```

If you want to process a file’s data in cloud code, you can retrieve the file using our HTTP networking libraries:

```javascript
Moralis.Cloud.httpRequest({ url: profilePhoto.url() }).then(function(response) {
  // The file contents are in response.buffer.
});
```

## Deleting Files

You can delete files that are referenced by objects using the `destroy` method. The master key is required to delete a file:

```javascript
const profilePhoto = profile.get("photoFile");
await profilePhoto.destroy({ useMasterKey: true });
```

## Adding Metadata and Tags

Adding metadata and tags to your files allows you to add additional bits of data to the files that are stored within your storage solution (i.e AWS S3).

Note: not all storage adapters support metadata and tags. Check the documentation for the storage adapter you’re using for compatibility.

```javascript
// Init with metadata and tags
const metadata = { createdById: 'some-user-id' };
const tags = { groupId: 'some-group-id' };
const file = new Moralis.File('myfile.zzz', fileData, 'image/png', metadata, tags);

// Add metadata and tags
const file = new Moralis.File('myfile.zzz', fileData, 'image/png');
file.addMetadata('createdById', 'some-user-id');
file.addTag('groupId', 'some-group-id');
```
