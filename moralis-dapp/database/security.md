---
description: >-
  As Your App Development Progresses, You Will Want to Use Moralis' Security
  Features to Safeguard Data Appropriately. This Document Explains the Ways in
  Which You Can Secure Your Apps.
---

# Security

If your app is compromised, it’s not only you as the developer who suffers, but potentially the users of your app as well. Continue reading for our suggestions for sensible defaults and precautions to take before releasing your app into the wild.

### Client vs. Server <a href="#client-vs-server" id="client-vs-server"></a>

When an app first connects to Moralis, it identifies itself with an Application ID. This key is shipped as a part of your app, and anyone can decompile your app or proxy network traffic from their device to find it. This exploit is even easier with JavaScript — one can simply “view source” in the browser and immediately find your client key.

This is why Moralis has many other security features to help you secure your data.&#x20;

The <mark style="color:green;">**client key**</mark> is given out to your users, so anything that can be done with just the client key is doable by the general public, even malicious hackers.

The <mark style="color:green;">**master key**</mark>, on the other hand, is definitely a security mechanism. Using the master key allows you to bypass all of your app's security mechanisms, such as [class-level permissions](security.md#class-level-permissions) and [ACLs](security.md#access-control-lists). Having the master key is like having root access to your app’s servers, and you should guard your master key with the same zeal with which you would guard your production machines’ root password.

The overall philosophy is to limit the power of your clients (using client keys) and to perform any sensitive actions requiring the master key in Cloud Code. You’ll learn how to best wield this power in the section titled [Implementing Business Logic in Cloud Code](security.md#implementing-business-logic-in-cloud-code).

{% hint style="info" %}
**Final Note**: It is recommended to set up HTTPS and SSL in your server, to avoid man-in-the-middle attacks, but Moralis works fine as well with non-HTTPS connections.
{% endhint %}

### Class-Level Permissions <a href="#class-level-permissions" id="class-level-permissions"></a>

The second level of security is at the schema and data level. Enforcing security measures at this level will restrict how and when client applications can access and create data on Moralis. When you first begin developing your Moralis application, all of the defaults are set so that you can be a more productive developer. For example:

* A client application can create new classes on Moralis.
* A client application can add fields to classes.
* A client application can modify or query for objects on Moralis.

{% hint style="success" %}
You can configure any of these permissions to apply to everyone, no one, or to specific users or roles in your app.&#x20;
{% endhint %}

Roles are groups that contain users or other roles, which you can assign to an object to restrict its use. Any permission granted to a role is also granted to any of its children, whether they are users or other roles, enabling you to create an access hierarchy for your apps.

Once you are confident that you have the right classes and relationships between classes in your app, you should begin to lock it down by doing the following:

**Almost every class that you create should have these permissions tweaked to some degree.** For classes where every object has the same permissions, class-level settings will be most effective. For example, one common use case entails having a class of static data that can be read by anyone but written by no one.

#### CONFIGURING CLASS-LEVEL PERMISSIONS <a href="#configuring-class-level-permissions" id="configuring-class-level-permissions"></a>

Moralis lets you specify what operations are allowed per class. This lets you restrict the ways in which clients can access or modify your classes. To change these settings, go to the "Data Browser," select a class, and click the “Security” button.

You can configure the client’s ability to perform each of the following operations for the selected class:

* **Read**:
  * **Get**: With Get permission, users can fetch objects in this table if they know their objectIds.
  * **Find**: Anyone with Find permission can query all of the objects in the table, even if they don’t know their objectIds. Any table with public Find permission will be completely readable by the public unless you put an ACL on each object.
* **Write**:
  * **Update**: Anyone with Update permission can modify the fields of any object in the table that doesn’t have an ACL. For publicly readable data, such as game levels or assets, you should disable this permission.
  * **Create**: Like Update, anyone with Create permission can create new objects of a class. As with the Update permission, you’ll probably want to turn this off for publicly readable data.
  * **Delete**: With this permission, people can delete any object in the table that doesn’t have an ACL. All they need is its objectId.
* **Add fields**: Moralis classes have schemas that are inferred when objects are created. While you’re developing your app, this is great, because you can add a new field to your object without having to make any changes on the back-end. But once you ship your app, it’s very rare to need to add new fields to your classes automatically. You should pretty much always turn off this permission for all of your classes when you submit your app to the public.

{% hint style="info" %}
For each of the above actions, you can grant permission to all users (which is the default), or lock permissions down to a list of roles and users.&#x20;
{% endhint %}

For example, a class that should be available to all users would be set to read-only by only enabling get and find. A logging class could be set to write-only by only allowing creates. You could enable moderation of user-generated content by providing an update and delete access to a particular set of users or roles.

### Object-Level Access Control <a href="#object-level-access-control" id="object-level-access-control"></a>

Once you’ve locked down your schema and class-level permissions, it’s time to think about how data is accessed by your users. Object-level access control enables one user’s data to be kept separate from another’s because sometimes different objects in a class need to be accessible by different people. For example, a user’s private personal data should be accessible only to them.

Moralis also supports the notion of anonymous users for those apps that want to store and protect user-specific data without requiring explicit login.

When a user logs into an app, they initiate a session with Moralis. Through this session, they can add and modify their own data but are prevented from modifying other users’ data.

#### ACCESS CONTROL LISTS <a href="#access-control-lists" id="access-control-lists"></a>

The easiest way to control who can access which data is through access control lists, commonly known as ACLs. The idea behind an ACL is that each object has a list of users and roles along with what permissions that user or role has. A user needs read permissions (or must belong to a role that has read permissions) in order to retrieve an object’s data, and a user needs write permissions (or must belong to a role that has write permissions) in order to update or delete that object.

Once you have a User, you can start using ACLs. Remember, Users can be created through traditional username/password sign up, through a third-party login system like Facebook or Twitter, or even by using Moralis’ [automatic anonymous user](https://docs.parseplatform.org/ios/guide/#anonymous-users) functionality. To set an ACL on the current user’s data to not be publicly readable, all you have to do is:

```
var user = Moralis.User.current();
user.setACL(new Moralis.ACL(user));
```

Most apps should do this. If you store any sensitive user data, such as email addresses or phone numbers, you need to set an ACL like this so that the user’s private information isn’t visible to other users. If an object doesn’t have an ACL, it’s readable and writeable by everyone. By default Moralis sets the ACL on new User objects so that only the user can read or write their own data. (If you as the developer need to update other `_User` objects, remember that your master key can provide the power to do this.)

If you want the user to have some data that is public and some that are private, it’s best to have two separate objects. You can add a pointer to the private data from the public one.

You can also set different permissions for different columns in a Class. See the [video](security.md#undefined) for practical demonstrations.

Of course, you can set different read and write permissions on an object. For example, this is how you would create an ACL for a public post by a user, where anyone can read it:

```
var acl = new Moralis.ACL();
acl.setPublicReadAccess(true);
acl.setWriteAccess(Moralis.User.current().id, true);
```

Sometimes it’s inconvenient to manage permissions on a per-user basis, and you want to have groups of users who get treated the same (like a set of admins with special powers). Roles are a special kind of object that lets you create a group of users that can all be assigned to the ACL. The best thing about roles is that you can add and remove users from a role without having to update every single object that is restricted to that role. To create an object that is writeable only by admins:

```
var acl = new Moralis.ACL();
acl.setPublicReadAccess(true);
acl.setRoleWriteAccess("admins", true);
```

Of course, this snippet assumes you’ve already created a role named “admins”. This is often reasonable when you have a small set of special roles set up while developing your app. Roles can also be created and updated on the fly — for example, adding new friends to a “friendOf\_\_\_” role after each connection is made.

All of this is just the beginning. Applications can enforce all sorts of complex access patterns through ACLs and class-level permissions. For example:

* For private data, read and write access can be restricted to the owner.
* For a post on a message board, the author and members of the “Moderators” role can have “write” access, and the general public can have “read” access.
* For logging data that will only be accessed by the developer through the REST API using the master key, the ACL can deny all permissions.
* Data created by a privileged group of users or the developer, like a global message of the day, can have public read access but restrict write access to an “Administrators” role.
* A message sent from one user to another can give “read” and “write” access just to those users.

For the curious, here’s the format for an ACL that restricts read and write permissions to the owner (whose `objectId` is identified by `"aSaMpLeUsErId"`) and enables other users to read the object:

```
{
    "*": { "read":true },
    "aSaMpLeUsErId": { "read" :true, "write": true }
}
```

And here’s another example of the format of an ACL that uses a Role:

```
{
    "role:RoleName": { "read": true },
    "aSaMpLeUsErId": { "read": true, "write": true }
}
```

#### POINTER PERMISSIONS <a href="#pointer-permissions" id="pointer-permissions"></a>

Pointer permissions are a special type of class-level permission that creates a virtual ACL on every object in a class, based on users stored in pointer fields on those objects. For example, given a class with an `owner` field, setting a read pointer permission on `owner` will make each object in the class only readable by the user in that object’s `owner` field. For a class with a `sender` and a `reciever` field, a read pointer permission on the `receiver` field and a read and write pointer permission on the `sender` field will make each object in the class readable by the user in the `sender` and `receiver` field, and writable only by the user in the `sender` field.

Given that objects often already have pointers to the user(s) that should have permissions on the object, pointer permissions provide a simple and fast solution for securing your app using data that is already there, that doesn’t require writing any client code or cloud code.

Pointer permissions are like virtual ACLs. They don’t appear in the ACL column, but if you are familiar with how ACLs work, you can think of them like ACLs. In the above example with the `sender` and `receiver`, each object will act as if it has an ACL of:

```
📋{
    "<SENDER_USER_ID>": {
        "read": true,
        "write": true
    },
    "<RECEIVER_USER_ID>": {
        "read": true
    }
}
```

Note that this ACL is not actually created on each object. Any existing ACLs will not be modified when you add or remove pointer permissions, and any user attempting to interact with an object can only interact with the object if both the virtual ACL created by the pointer permissions, and the real ACL already on the object allow the interaction. For this reason, it can sometimes be confusing to combine pointer permissions and ACLs, so we recommend using pointer permissions for classes that don’t have many ACLs set. Fortunately, it’s easy to remove pointer permissions if you later decide to use Cloud Code or ACLs to secure your app.

#### REQUIRES AUTHENTICATION PERMISSION <a href="#requires-authentication-permission-requires-parse-server---230" id="requires-authentication-permission-requires-parse-server---230"></a>

The CLP `requiresAuthentication` prevents any non-authenticated user from performing the action protected by the CLP.

For example, if you want to allow your **authenticated users** to `find` and `get` `Announcement`’s from your application and your **admin role** to have all privileged, you would set the CLP:

```
📋// POST https://my-moralis-dapp.com/schemas/Announcement
// Note: You need to use PUT http method if the class already exists
// Set the X-Parse-Application-Id and X-Parse-Master-Key header
// body:
{
  "classLevelPermissions":
  {
    "find": {
      "requiresAuthentication": true,
      "role:admin": true
    },
    "get": {
      "requiresAuthentication": true,
      "role:admin": true
    },
    "create": { "role:admin": true },
    "update": { "role:admin": true },
    "delete": { "role:admin": true }
  }
}
```

**Effects**:

* Non-authenticated users won’t be able to do anything.
* Authenticated users (any user with a valid sessionToken) will be able to read all the objects in that class.
* Users belonging to the admin role will be able to perform all operations.

{% hint style="warning" %}
**Warning:** Note that this is in no way securing your content, if you allow anyone to login into your server, every client will still be able to query this object.
{% endhint %}

#### CLP AND ACL INTERACTION <a href="#clp-and-acl-interaction" id="clp-and-acl-interaction"></a>

Class-Level Permissions (CLPs) and Access Control Lists (ACLs) are both powerful tools for securing your app, but they don’t always interact exactly how you might expect. They actually represent two separate layers of security that each request has to pass through to return the correct information or make the intended change. These layers, one at the class level, and one at the object level are shown below. A request must pass through BOTH layers of checks in order to be authorized. Note that despite acting similarly to ACLs, [Pointer Permissions](objects.md#relational-data) are a type of class level permission, so a request must pass the pointer permission check in order to pass the CLP check.

![CLP vs ACL Diagram](https://docs.parseplatform.org/assets/images/clp\_vs\_acl\_diagram.png)

As you can see, whether a user is authorized to make a request can become complicated when you use both CLPs and ACLs. Let’s look at an example to get a better sense of how CLPs and ACLs can interact. Say we have a `Photo` class, with an object, `photoObject`. There are 2 users in our app, `user1` and `user2`. Now let's say we set a Get CLP on the `Photo` class, disabling public Get, but allowing `user1` to perform Get. Now let’s also set an ACL on `photoObject` to allow Read - which includes GET - for only `user2`.

You may expect that this will allow both `user1` and `user2` to get `photoObject`, but because the CLP layer of authentication and the ACL layer are both in effect at all times, it actually makes it so _neither_ `user1` nor `user2` can Get `photoObject`. If `user1` tries to Get `photoObject`, it will get through the CLP layer of authentication, but will then be rejected because it does not pass the ACL layer. In the same way, if `user2` tries to Get `photoObject`, it will also be rejected at the CLP layer of authentication.

Now let's look at an example that uses Pointer Permissions. Say we have a `Post` class, with an object, `myPost`. There are two users in our app, `poster`, and `viewer`. Let's say we add Pointer Permission that gives anyone in the `Creator` field of the `Post` class read and write access to the object, and for the `myPost` object, `poster` is the user in that field. There is also an ACL on the object that gives read access to `viewer`. You may expect that this will allow `poster` to read and edit `myPost`, and `viewer` to read it, but `viewer` will be rejected by the Pointer Permission, and `poster` will be rejected by the ACL, so again, neither user will be able to access the object.

Because of the complex interaction between CLPs, Pointer Permissions, and ACLs, we recommend being careful when using them together. It can often be useful to use CLPs only to disable all permissions for a certain request type and then use Pointer Permissions or ACLs for other request types. For example, you may want to disable Delete for a `Photo` class, but then put a Pointer Permission on `Photo` so the user who created it can edit it, just not delete it. Because of the especially complex way that Pointer Permissions and ACLs interact, we usually recommend only using one of those two types of security mechanisms.

#### SECURITY EDGE CASES <a href="#security-edge-cases" id="security-edge-cases"></a>

There are some special classes in Moralis that don’t follow all of the same security rules as every other class. Not all classes follow [Class-Level Permissions (CLPs)](security.md#class-level-permissions) or [Access Control Lists (ACLs)](security.md#access-control-lists) exactly how they are defined, and here are those exceptions documented.&#x20;

Here, “normal behaviour” refers to CLPs and ACLs working normally, while any other special behaviours are described in the footnotes.

|           | `_User`                    | `_Installation`               |
| --------- | -------------------------- | ----------------------------- |
| Get       | normal behavior \[1, 2, 3] | ignores CLP, but not ACL      |
| Find      | normal behavior \[3]       | master key only \[6]          |
| Create    | normal behavior \[4]       | ignores CLP                   |
| Update    | normal behavior \[5]       | ignores CLP, but not ACL \[7] |
| Delete    | normal behavior \[5]       | master key only \[7]          |
| Add Field | normal behavior            | normal behavior               |

1. Logging in, or `/server/login` in the REST API, does not respect the Get CLP on the user class. Login works just based on username and password, and cannot be disabled using CLPs.
2. Retrieving the current user, or becoming a User based on a session token, which is both `/server/users/me` in the REST API, do not respect the Get CLP on the user class.
3. Read ACLs do not apply to the logged-in user. For example, if all users have ACLs with Read disabled, then doing a find query over users will still return the logged-in user. However, if the Find CLP is disabled, then trying to perform a find on users will still return an error.
4. Create CLPs also apply to signing up. So disabling Create CLPs on the user class also disables people from signing up without the master key.
5. Users can only Update and Delete themselves. Public CLPs for Update and Delete may still apply. For example, if you disable public Update for the user class, then users cannot edit themselves. But no matter what the write ACL on a user is, that user can still Update or Delete itself, and no other user can Update or Delete that user. As always, however, using the master key allows users to update other users, independent of CLPs or ACLs.
6. Get requests on installations follows ACLs normally. Find requests without the master key is not allowed unless you supply the `installationId` as a constraint.

### Data Integrity in Cloud Code <a href="#data-integrity-in-cloud-code" id="data-integrity-in-cloud-code"></a>

For most apps, care around keys, class-level permissions, and object-level ACLs are all you need to keep your app and your users’ data safe. Sometimes, though, you’ll run into an edge case where they aren’t quite enough. For everything else, there’s [Cloud Code](../cloud-code/cloud-functions.md#implementing-cloud-function-validation).

Cloud Code allows you to upload JavaScript to Moralis' servers, where we will run it for you. Unlike client code running on users’ devices that may have been tampered with, Cloud Code is guaranteed to be the code that you’ve written, so it can be trusted with more responsibility.

One particularly common use case for Cloud Code is preventing invalid data from being stored. For this sort of situation, it’s particularly important that a malicious client cannot bypass the validation logic.

To create validation functions, Cloud Code allows you to implement a `beforeSave` trigger for your class. These triggers are run whenever an object is saved, and allow you to modify the object or completely reject a save. For example, this is how you create a [Cloud Code beforeSave trigger](../cloud-code/triggers.md#beforesave) to make sure every user has an email address set:

```
📋Moralis.Cloud.beforeSave('_User', request => {
  const user = request.object;
  if (!user.get("email")) {
    throw "Every user must have an email address.";
  }
});
```

Validations can lock down your app so that only certain values are acceptable. You can also use `afterSave` validations to normalize your data (e.g. formatting all phone numbers or currency identically). You get to retain most of the productivity benefits of accessing Moralis data directly from your client applications, but you can also enforce certain invariants for your data on the fly.

**Common scenarios that warrant validation include:**

* Making sure phone numbers have the right format.
* Sanitizing data so that its format is normalized.
* Making sure that an email address looks like a real email address.
* Requiring that every user specifies an age within a particular range.
* Not letting users directly change a calculated field.
* Not letting users delete specific objects unless certain conditions are met.

### Implementing Business Logic in Cloud Code <a href="#implementing-business-logic-in-cloud-code" id="implementing-business-logic-in-cloud-code"></a>

While validation often makes sense in Cloud Code, there are likely certain actions that are particularly sensitive and should be guarded as carefully as possible. In these cases, you can remove permissions or the logic from clients entirely and instead funnel all such operations to Cloud Code functions.

When a Cloud Code function is called, it can use the optional `{useMasterKey:true}` parameter to gain the ability to modify user data. With the master key, your Cloud Code function can override any ACLs and write data. This means that it’ll bypass all the security mechanisms you’ve put in place in the previous sections.

Say you want to allow a user to “like” a `Post` object without giving them full write permissions on the object. You can do this by having the client call a Cloud Code function instead of modifying the Post itself:

The master key should be used carefully. Setting `useMasterKey` to `true` only in the individual API function calls that need that security override:

```
📋Moralis.Cloud.define("like", async request => {
  var post = new Moralis.Object("Post");
  post.id = request.params.postId;
  post.increment("likes");
  await post.save(null, { useMasterKey: true })
});
```

One very common use case for Cloud Code is sending push notifications to particular users. In general, clients can’t be trusted to send push notifications directly, because they could modify the alert text, or push to people they shouldn’t be able to. Your apps settings will allow you to set whether “client push” is enabled or not; we recommend that you make sure it’s disabled. Instead, you should write Cloud Code functions that validate the data to be pushed and sent before sending a push.

### Client Class Creation

By default Moralis allows any SDK user to modify the database by creating new Classes and changing the structure of existing classes.

This is great during the development stage but should be turned off when you go to production so that your database is protected from spam (in case someone uses SDK to fill your database with new Classes or adds a lot of columns to existing columns).

This can be done in server settings.

![Client Class Creation needs to be disabled when you go to production.](<../../.gitbook/assets/Screenshot 2021-11-02 at 19.37.00.png>)

### Summary <a href="#parse-security-summary" id="parse-security-summary"></a>

Moralis provides a number of ways for you to secure data in your app. As you build your app and evaluate the kinds of data you will be storing, you can make the decision about which implementation to choose.

Most classes in your app will fall into one of a couple of easy-to-secure categories. For fully public data, you can use class-level permissions to lock down the table to put it publicly readable and writeable by no one. For fully private data, you can use ACLs to make sure that only the user who owns the data can read it. But occasionally, you’ll run into situations where you don’t want data that’s fully public or fully private. For example, you may have a social app, where you have data for a user that should be readable only to friends whom they’ve approved. For this, you’ll need a combination of the techniques discussed in this guide to enable exactly the sharing rules you desire.

We hope that you’ll use these tools to do everything you can to keep your app’s data and your users’ data secure. Together, we can make the web a safer place.

### Tutorial

{% hint style="info" %}
Legacy UI might be present in the videos, some things might be different
{% endhint %}

{% embed url="https://www.youtube.com/watch?v=Yd4gFQ5ppmQ" %}
CLP, ACL and protected fields practically demonstrated.
{% endembed %}

{% embed url="https://youtu.be/xK0OJxp_l3g" %}
Objects and Security
{% endembed %}
