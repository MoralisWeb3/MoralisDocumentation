---
description: Managing Roles.
---

# Roles

As your app grows in scope and user-base, you may find yourself needing more coarse-grained control over access to pieces of your data than user-linked ACLs can provide. To address this requirement, Moralis supports a form of "[Role-based Access Control](https://en.wikipedia.org/wiki/Role-based_access_control)." Roles provide a logical way of grouping users with common access privileges to your Moralis data. Roles are named objects that contain users and other roles. Any permission granted to a role is implicitly granted to its users as well as to the users of any roles that it contains.

For example, in your application with curated content, you may have a number of users that are considered “Moderators” and can modify and delete content created by other users. You may also have a set of users that are “Administrators” and are allowed all of the same privileges as Moderators, but can also modify the global settings for the application. By adding users to these roles, you can ensure that new users can be made Moderators or Administrators, without having to manually grant permission to every resource for each user.

We provide a specialized class called `Moralis.Role` that represents these role objects in your client code. `Moralis.Role` is a subclass of `Moralis.Object`, and has all of the same features, such as a flexible schema, automatic persistence, and a key-value interface. All the methods that are on `Moralis.Object` also exist on `Moralis.Role`. The difference is that `Moralis.Role` has some additions specific to the management of roles.

### `Moralis.Role` Properties <a href="parserole-properties" id="parserole-properties"></a>

`Moralis.Role` has several properties that set it apart from `Moralis.Object`:

* **name**: The name for the role. This value is required, and can only be set once as a role is being created. The name must consist of alphanumeric characters, spaces, -, or \_. This name will be used to identify the Role without needing its objectId.
* **users**: A [relation](objects.md#relational-data) to the set of users that will inherit permissions granted to the containing role.
* **roles**: A [relation](objects.md#relational-data) to the set of roles whose users and roles will inherit permissions granted to the containing role.

### Security for Role Objects <a href="security-for-role-objects" id="security-for-role-objects"></a>

The `Moralis.Role` uses the same security scheme (ACLs) as all other objects on Moralis, except that it requires an ACL to be set explicitly. Generally, only users with greatly elevated privileges (e.g. a master user or Administrator) should be able to create or modify a Role, so you should define its ACLs accordingly. Remember, if you give write-access to a `Moralis.Role` for a user, that user can add other users to the role, or even delete the role altogether.

To create a new `Moralis.Role`, you would write:

```javascript
// By specifying no write privileges for the ACL, we can ensure the role cannot be altered.
const roleACL = new Moralis.ACL();
roleACL.setPublicReadAccess(true);
const role = new Moralis.Role("Administrator", roleACL);
role.save();
```

You can add users and roles that should inherit your new role’s permissions through the “users” and “roles” relations on `Moralis.Role`:

```javascript
const role = new Moralis.Role(roleName, roleACL);
role.getUsers().add(usersToAddToRole);
role.getRoles().add(rolesToAddToRole);
role.save();
```

Take great care when assigning ACLs to your roles so that they can only be modified by those who should have permission to modify them.

### Role-Based Security for Other Objects <a href="role-based-security-for-other-objects" id="role-based-security-for-other-objects"></a>

Now that you have created a set of roles for use in your application, you can use them with ACLs to define the privileges that their users will receive. Each `Moralis.Object` can specify a `Moralis.ACL`, which provides an access control list that indicates which users and roles should be granted read or write access to the object.

Giving a role read or write permission to an object is straightforward. You could use the `Moralis.Role`:

```javascript
const moderators = /* Query for some Moralis.Role */;
const wallPost = new Moralis.Object("WallPost");
const postACL = new Moralis.ACL();
postACL.setRoleWriteAccess(moderators, true);
wallPost.setACL(postACL);
wallPost.save();
```

You can avoid querying for a role by specifying its name for the ACL:

```javascript
const wallPost = new Moralis.Object("WallPost");
const postACL = new Moralis.ACL();
postACL.setRoleWriteAccess("Moderators", true);
wallPost.setACL(postACL);
wallPost.save();
```

### Role Hierarchy <a href="role-hierarchy" id="role-hierarchy"></a>

As described above, one role can contain another, establishing a parent-child relationship between the two roles. The consequence of this relationship is that any permission granted to the parent role is implicitly granted to all of its child roles.

These types of relationships are commonly found in applications with user-managed content, such as forums. Some small subset of users are Administrators, with the highest level of access to tweaking the application’s settings, creating new forums, setting global messages, and so on. Another set of users are Moderators, who are responsible for ensuring that the content created by users remains appropriate. Any user with Administrator privileges should also be granted the permissions of any Moderator. To establish this relationship, you would make your Administrators role a child role of Moderators, like this:

```javascript
const administrators = /* Your "Administrators" role */;
const moderators = /* Your "Moderators" role */;
moderators.getRoles().add(administrators);
moderators.save();
```
