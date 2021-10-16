---
description: Technical Details on Data Storage.
---

# Data

## Data <a href="data" id="data"></a>

We’ve designed the Moralis SDKs so that you typically don’t need to worry about how data is saved while using the client SDKs. Simply add data to the Moralis `Object`, and it’ll be saved correctly.

Nevertheless, there are some cases where it’s useful to be aware of how data is stored on the Moralis platform.

### Data Storage <a href="data-storage" id="data-storage"></a>

Internally, Moralis stores data as JSON, so any datatype that can be converted to JSON can be stored on Moralis. Refer to the [Data Types in Objects](objects.md#data-types) section of this guide to see platform-specific examples.

Keys including the characters `$` or `.`, along with the key `__type` key, are reserved for the framework to handle additional types, so don’t use those yourself. Key names must contain only numbers, letters, and underscore, and must start with a letter. Values can be anything that can be JSON-encoded.

### Data Type Lock-In <a href="data-type-lock-in" id="data-type-lock-in"></a>

When a class is initially created, it doesn’t have an inherent schema defined. This means that the first object could have any type and multiple fields that you'd like.

However, after a field has been set at least once, that field is locked into the particular type that was saved. For example, if a `User` object is saved with field `name` of type `String`, that field will be restricted to the `String` type only (the server will return an error if you try to save anything else).

One special case is that any field can be set to `null`, no matter what type it is.

### The Data Browser <a href="the-data-browser" id="the-data-browser"></a>

The Data Browser is the web UI where you can update and create objects in each of your apps. Here, you can see the raw JSON values that are saved that represent each object in your class.

When using the interface, keep in mind the following:

* The `objectId`, `createdAt`, `updatedAt` fields cannot be edited (these are set automatically).
* The value “(empty)” denotes that the field has not been set for that particular object (this is different than `null`).
* You can remove a field’s value by hitting your "Delete" key while the value is selected.

The Data Browser is also a great place to test the Cloud Code validations contained in your Cloud Code functions (such as `beforeSave`). These are run whenever a value is changed or an object is deleted from the Data Browser, just as they would be if the value was changed or deleted from your client code.
