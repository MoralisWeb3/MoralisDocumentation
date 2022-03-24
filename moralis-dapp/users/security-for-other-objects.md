---
description: Security Principles.
---

# Security For Other Objects

The same security model that applies to the `Moralis.User` can be applied to other objects. For any object, you can specify which users are allowed to read and/or modify the object. To support this type of security, each object has an [access control list](http://en.wikipedia.org/wiki/Access_control_list), implemented by the `Moralis.ACL` class.

The simplest way to use a `Moralis.ACL` is to specify that an object may only be read or written by a single user. This is done by initializing a Moralis.ACL with a `Moralis.User`: `new Moralis.ACL(user)` generates a `Moralis.ACL` that limits access to that user. An object's ACL is updated when the object is saved, like any other property. Thus, to create a private note that can only be accessed by the current user:

```javascript
const Note = Moralis.Object.extend("Note");
const privateNote = new Note();
privateNote.set("content", "This note is private!");
privateNote.setACL(new Moralis.ACL(Moralis.User.current()));
privateNote.save();
```

This note will then only be accessible to the current user, although it will be accessible to any device where that user is signed in. This functionality is useful for applications where you want to enable access to user data across multiple devices, like a personal to-do list.

Permissions can also be granted on a per-user basis. You can add permissions individually to a `Moralis.ACL` using `setReadAccess` and `setWriteAccess`. For example, let's say you have a message that will be sent to a group of several users, where each individual user has the right to read and delete that message:

```javascript
const Message = Moralis.Object.extend("Message");
const groupMessage = new Message();
const groupACL = new Moralis.ACL();

// userList is an array with the users we are sending this message to.
for (let i = 0; i < userList.length; i++) {
  groupACL.setReadAccess(userList[i], true);
  groupACL.setWriteAccess(userList[i], true);
}

groupMessage.setACL(groupACL);
groupMessage.save();
```

You can also grant permissions to all users at once using `setPublicReadAccess` and `setPublicWriteAccess`. This allows patterns like posting comments on a message board. For example, to create a post that can only be edited by its author, but can be read by anyone:

```javascript
const publicPost = new Post();
const postACL = new Moralis.ACL(Moralis.User.current());
postACL.setPublicReadAccess(true);
publicPost.setACL(postACL);
publicPost.save();
```

Operations that are forbidden, such as deleting an object that you do not have write access to, result in a `Moralis.Error.OBJECT_NOT_FOUND` error code. For security purposes, this prevents clients from distinguishing which object IDs exist but are secured, versus which object IDs do not exist at all.
