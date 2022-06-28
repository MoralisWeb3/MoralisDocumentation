---
description: Storing Data in a Moralis Server.
---

# Objects

## Moralis.Object

Storing data on Moralis is built around `Moralis.Object`. Each `Moralis.Object` contains key-value pairs of JSON-compatible data. This data is schemaless, which means that you don’t need to specify ahead of time what keys exist on each `Moralis.Object`. You simply set whatever key-value pairs you want, and our back-end will store them.

For example, let’s say you’re building an NFT game where the characters are monsters. A single `Moralis.Object` could contain:

_`strength: 1024, ownerName: "Aegon", canFly: true`_

Keys must be alphanumeric strings. Values can be strings, numbers, booleans, or even arrays and dictionaries - anything that can be JSON-encoded.

Each `Moralis.Object` is an instance of a specific subclass with a class name that you can use to distinguish different sorts of data. For example, we could call the object a `LegendaryMonster`.

{% hint style="info" %}
We recommend that you NameYourClassesLikeThis and nameYourKeysLikeThis, just to keep your code looking pretty.
{% endhint %}

### <mark style="color:green;">Moralis.Object.extend()</mark>

To create a new subclass, use the `Moralis.Object.extend` method. Any `Moralis.Query` will return instances of the new class for any `Moralis.Object` with the same classname. If you’re familiar with `Backbone.Model`, then you already know how to use `Moralis.Object`. It’s designed to be created and modified in the same ways.

```javascript
// Simple syntax to create a new subclass of Moralis.Object.
const LegendaryMonster = Moralis.Object.extend("Monster");

// Create a new instance of that class.
const monster = new LegendaryMonster();

// Alternatively, you can use the typical Backbone syntax.
const LegendaryMonster = Moralis.Object.extend({
  className: "Monster",
});
```

You can add additional methods and properties to your subclasses of `Moralis.Object`.

```javascript
// A complex subclass of Moralis.Object
const Monster = Moralis.Object.extend(
  "Monster",
  {
    // Instance methods
    hasSuperHumanStrength: function () {
      return this.get("strength") > 18;
    },
    // Instance properties go in an initialize method
    initialize: function (attrs, options) {
      this.sound = "Rawr";
    },
  },
  {
    // Class methods
    spawn: function (strength) {
      const monster = new Monster();
      monster.set("strength", strength);
      return monster;
    },
  }
);

const monster = Monster.spawn(200);
alert(monster.get("strength")); // Displays 200.
alert(monster.sound); // Displays Rawr.
```

To create a single instance of any Moralis Object class, you can also use the `Moralis.Object` constructor directly. `new Moralis.Object(className)` will create a single Moralis Object with that class name.

If you’re already using ES6 in your codebase. You can subclass `Moralis.Object` with the `extends` keyword:

```javascript
class Monster extends Moralis.Object {
  constructor() {
    // Pass the ClassName to the Moralis.Object constructor
    super("Monster");
    // All other initialization
    this.sound = "Rawr";
  }

  hasSuperHumanStrength() {
    return this.get("strength") > 18;
  }

  static spawn(strength) {
    const monster = new Monster();
    monster.set("strength", strength);
    return monster;
  }
}
```

However, when using `extends`, the SDK is not automatically aware of your subclass. If you want objects returned from queries to use your subclass of `Moralis.Object`, you will need to register the subclass, similar to what we do on other platforms.

```javascript
// After specifying the Monster subclass...
Moralis.Object.registerSubclass("Monster", Monster);
```

Similarly, you can use `extends` with `Moralis.User`.

```javascript
class CustomUser extends Moralis.User {
  constructor(attributes) {
    super(attributes);
  }

  doSomething() {
    return 5;
  }
}
Moralis.Object.registerSubclass("_User", CustomUser);
```

In addition to queries, `logIn` and `signUp` will return the subclass `CustomUser`.

```javascript
const customUser = new CustomUser({ foo: "bar" });
customUser.setUsername("username");
customUser.setPassword("password");
customUser.signUp().then((user) => {
  // user is an instance of CustomUser
  user.doSomething(); // return 5
  user.get("foo"); // return 'bar'
});
```

`CustomUser.logIn` and `CustomUser.signUp` will return the subclass `CustomUser`.

## Save Objects

### <mark style="color:green;">myObject.save()</mark>

Let’s say you want to save the `Monster` described above to the Moralis Cloud. The interface is similar to a `Backbone.Model`, including the `save` method:

{% tabs %}
{% tab title="JS" %}

```javascript
const Monster = Moralis.Object.extend("Monster");
const monster = new Monster();

monster.set("strength", 1024);
monster.set("ownerName", "Aegon");
monster.set("canFly", true);

monster.save().then(
  (monster) => {
    // Execute any logic that should take place after the object is saved.
    alert("New object created with objectId: " + monster.id);
  },
  (error) => {
    // Execute any logic that should take place if the save fails.
    // error is a Moralis.Error with an error code and message.
    alert("Failed to create new object, with error code: " + error.message);
  }
);
```

{% endtab %}

{% tab title="React" %}

```javascript
import { useNewMoralisObject } from "react-moralis";

export default function App() {
  const { save } = useNewMoralisObject("Monster");

  const saveObject = async () => {
    const data = {
      strength: 1024,
      ownerName: "Aegon",
      canFly: true,
    };

    save(data, {
      onSuccess: (monster) => {
        // Execute any logic that should take place after the object is saved.
        alert("New object created with objectId: " + monster.id);
      },
      onError: (error) => {
        // Execute any logic that should take place if the save fails.
        // error is a Moralis.Error with an error code and message.
        alert("Failed to create new object, with error code: " + error.message);
      },
    });
  };

  return <button onClick={saveObject}>Call The Code</button>;
}
```

{% endtab %}

{% tab title="Unity" %}

```csharp
using Moralis.Platform.Objects;
using MoralisUnity;
using Newtonsoft.Json;

public class Monster : MoralisObject
{
    public int strength { get; set; }
    public string ownerName { get; set; }
    public bool canFly { get; set; }
    // base class is required
    public Monster() : base("Monster"){}
}
public async void SaveObjectToDB()
    {
      try {
        Monster monster = Moralis.Create<Monster>();
        monster.strength = 1024;
        monster.ownerName = "Aegon";
        monster.canFly = true;
        await monster.SaveAsync();
        print("Created new object");
      }
      catch (Exception e){
        print("Failed to create new object, with error code: " + e);
      }
    }
```

{% endtab %}
{% endtabs %}

After this code runs, you will probably be wondering if anything really happened. To make sure the data was saved, you can look at the "Data Browser" in your "Moralis Dashboard". You should see something like this:

```javascript
objectId: "xWMyZ4YEGZ", strength: 1024, ownerName: "Aegon", canFly: true,
createdAt:"2011-06-10T18:33:42Z", updatedAt:"2011-06-10T18:33:42Z"
```

There are two things to note here:

1. You didn’t have to configure or set up a new class called `Monster` before running this code.
2. Your Moralis app lazily creates this class for you when it first encounters it.

There are also a few fields you don’t need to specify that are provided as a convenience. `objectId` is a unique identifier for each saved object. `createdAt` and `updatedAt` represent the time that each object was created and last modified in the cloud. Each of these fields is filled in by Moralis, so they don’t exist on a `Moralis.Object` until a save operation has been completed.

If you prefer, you can set attributes directly in your call to `save` instead.

```javascript
const Monster = Moralis.Object.extend("Monster");
const monster = new Monster();

monster
  .save({
    strength: 1024,
    ownerName: "Aegon",
    canFly: true,
  })
  .then(
    (monster) => {
      // The object was saved successfully.
    },
    (error) => {
      // The save failed.
      // error is a Moralis.Error with an error code and message.
    }
  );
```

{% hint style="warning" %}
By default, the classes you create will have no permissions set - meaning that anyone can write data into the class and read data from the class. Please see [Security Docs](https://docs.moralis.io/moralis-dapp/database/security) about securing your classes and adding permissions.
{% endhint %}

By default the classes you create will have no permissions set meaning that anyone can write data into the class and read data from the class. Please see [Security Docs](https://docs.moralis.io/moralis-dapp/database/security) about securing your classes and adding permissions.

#### SAVING NESTED OBJECTS

You may add a `Moralis.Object` as the value of a property in another `Moralis.Object`. By default, when you call `save()` on the parent object, all nested objects will be created and/or saved as well in a batch operation. This feature makes it really easy to manage relational data as you don’t have to take care of creating the objects in any specific order.

```javascript
const Child = Moralis.Object.extend("Child");
const child = new Child();

const Parent = Moralis.Object.extend("Parent");
const parent = new Parent();

parent.save({ child: child });
// Automatically the object Child is created on the server
// just before saving the Parent
```

In some scenarios, you may want to prevent this default chain save. For example, when saving a monster’s profile that has an owner property pointing to an account owned by another user to which you don’t have write access. In this case, setting the option `cascadeSave` to `false` may be useful:

```javascript
const Monster = Moralis.Object.extend("Monster");
const monster = new Monster();
monster.set("ownerAccount", ownerAccount); // Suppose `ownerAccount` has been created earlier.

monster.save(null, { cascadeSave: false });
// Will save `teamMember` wihout attempting to save or modify `ownerAccount`
```

#### CLOUD CODE CONTEXT

You may pass a `context` dictionary that is accessible in cloud code `beforeSave` and `afterSave` triggers for that `Moralis.Object`. This is useful if you want to condition certain operations in cloud code triggers on ephemeral information that should not be saved with the `Moralis.Object` in the database. The context is ephemeral in the sense that it vanishes after the cloud code triggers for that particular `Moralis.Object` as it has been executed. For example:

```javascript
const Monster = Moralis.Object.extend("Monster");
const monster = new Monster();
monster.set("species", "Dragon");

const context = { notifyMonsterGuild: true };
await monster.save(null, { context: context });
```

The context is then accessible in cloud code:

```javascript
Moralis.Cloud.afterSave("Monster", async (req) => {
  const notifyMonsterGuild = req.context.notifyMonsterGuild;
  if (notifyMonsterGuild) {
    // Notify the guild about new monster.
  }
});
```

## Retrieve Objects

### <mark style="color:green;">myObject.get()</mark>

Saving data to the cloud is fun, but it’s even more fun to get that data out again.

If the `Moralis.Object` has been uploaded to the server, you can use the `objectId` to retrieve it using a `Moralis.Query`:

{% tabs %}
{% tab title="JS" %}

```javascript
const Monster = Moralis.Object.extend("Monster");
const query = new Moralis.Query(Monster);

//get monster with id xWMyZ4YEGZ
query.get("xWMyZ4YEGZ").then(
  (monster) => {
    // The object was retrieved successfully.
  },
  (error) => {
    // The object was not retrieved successfully.
    // error is a Moralis.Error with an error code and message.
  }
);
```

{% endtab %}

{% tab title="React" %}

```javascript
import { useMoralisQuery } from "react-moralis";

export default function App() {
  const { fetch } = useMoralisQuery(
    "Monster",
    (query) => query.equalTo("objectId", "xWMyZ4YEGZ"),
    [],
    { autoFetch: false }
  );

  const objectIdQuery = () => {
    fetch({
      onSuccess: (monster) => {
        // The object was retrieved successfully.
      },
      onError: (error) => {
        // The object was not retrieved successfully.
        // error is a Moralis.Error with an error code and message.
      },
    });
  };

  return <button onClick={objectIdQuery}>Call The Code</button>;
}
```

{% endtab %}
{% endtabs %}

To get the values out of the `Moralis.Object`, use the `get` method:

```javascript
const strength = monster.get("strength");
const ownerName = monster.get("ownerName");
const canFly = monster.get("canFly");
```

Alternatively, the `attributes` property of the `Moralis.Object` can be treated as a JavaScript object, and even destructured.

```javascript
const { strength, ownerName, canFly } = result.attributes;
```

{% hint style="info" %}
The four special reserved values are provided as properties and cannot be retrieved using the ‘**get**’ method nor modified with the ‘**set**’ method:

```
const updatedAt = monster.updatedAt;
const objectId = monster.id;
const createdAt = monster.createdAt;
const acl = monster.getACL();
```

{% endhint %}

### <mark style="color:green;">myObject.fetch()</mark>

If you need to refresh an object you already have with the latest data currently in the Moralis Cloud, you can call the `fetch` method like so:

```javascript
myObject.fetch().then(
  (myObject) => {
    // The object was refreshed successfully.
  },
  (error) => {
    // The object was not refreshed successfully.
    // error is a Moralis.Error with an error code and message.
  }
);
```

### <mark style="color:green;">myObject.isDataAvailable()</mark>

If you need to check if an object has been fetched, you can call the `isDataAvailable()` method:

```javascript
if (!myObject.isDataAvailable()) {
  await myObject.fetch();
}
```

## Update Objects

### <mark style="color:green;">myObject.set()</mark>

Updating an object is simple. Just set some new data on it and call the save method. For example:

{% tabs %}
{% tab title="JS" %}

```javascript
// Create the object.
const Monster = Moralis.Object.extend("Monster");
const monster = new Monster();

monster.set("strength", 1024);
monster.set("energy", 1337);
monster.set("owner", "Aegon");
monster.set("canFly", false);
monster.set("skills", ["pwnage", "flying"]);

monster.save().then((monster) => {
  // Now let's update it with some new data. In this case, only canFly and energy
  // will get sent to the cloud. ownerName hasn't changed.
  monster.set("canFly", true);
  monster.set("energy", 1338);
  return monster.save();
});
```

{% endtab %}

{% tab title="React" %}

```javascript
import { useNewMoralisObject } from "react-moralis";

export default function App() {
  const { save } = useNewMoralisObject("Monster");

  const updateObject = async () => {
    const data = {
      strength: 1024,
      energy: 1337,
      owner: "Aegon",
      canFly: false,
      skills: ["pwnage", "flying"],
    };

    save(data, {
      onSuccess: (monster) => {
        monster.set("canFly", true);
        monster.set("strength", 1338);
        return monster.save();
      },
    });
  };

  return <button onClick={updateObject}>Call The Code</button>;
}
```

{% endtab %}
{% endtabs %}

Moralis automatically figures out which data has changed so only “dirty” fields will be sent to the Moralis Cloud. You don’t need to worry about squashing data that you didn’t intend to update.

### <mark style="color:green;">myObject.increment()</mark>

#### COUNTERS

The above example contains a common use case. The strength field is a counter that we’ll need to continually update with the monster's latest energy. Using the above method works but it’s cumbersome and can lead to problems if you have multiple clients trying to update the same counter.

To help with storing counter-type data, Moralis provides methods that automatically increment (or decrement) any number field. So, the same update can be rewritten as:

```javascript
monster.increment("energy");
monster.save();
```

You can also increment by any amount by passing in a second argument to `increment`. When no amount is specified, 1 is used by default.

### <mark style="color:green;">myObject.addUnique()</mark>

#### ARRAYS <a href="#arrays" id="arrays"></a>

To help with storing array data, there are three operations that can be used to automatically change an array associated with a given key:

- `add` append the given object to the end of an array field.
- `addUnique` add the given object only if it isn’t already contained in an array field. The position of the insert is not guaranteed.
- `remove` remove all instances of the given object from an array field.

```javascript
monster.addUnique("skills", "flying");
monster.addUnique("skills", "kungfu");
monster.save();
```

Note that it is not currently possible to automatically add and remove items from an array in the same save. You will have to call `save` in between every different kind of array operation.

## Destroy Objects

### <mark style="color:green;">myObject.destroy()</mark>

To delete an object from the cloud:

```javascript
myObject.destroy().then(
  (myObject) => {
    // The object was deleted from the Moralis Cloud.
  },
  (error) => {
    // The delete failed.
    // error is a Moralis.Error with an error code and message.
  }
);
```

### <mark style="color:green;">myObject.unset()</mark>

You can delete a single field from an object with the `unset` method:

```javascript
// After this, the ownerName field will be empty
myObject.unset("ownerName");

// Saves the field deletion to the Moralis Cloud.
// If the object's field is an array, call save() after every unset() operation.
myObject.save();
```

{% hint style="warning" %}
Please note that the use of **object.set(null)** to remove a field from an object is not recommended and will result in unexpected functionality.
{% endhint %}

## Relational Data

Objects may have relationships with other objects. For example, in a blogging application, a `Post` object may have many `Comment` objects. Moralis supports all kinds of relationships:

1. One-to-one
2. One-to-many
3. Many-to-many.

### <mark style="color:green;">One-to-One AND One-to-Many</mark> <a href="#one-to-one-and-one-to-many-relationships" id="one-to-one-and-one-to-many-relationships"></a>

One-to-one and one-to-many relationships are modelled by saving a `Moralis.Object` as a value in the other object.

For example, each `Comment` in a blogging app might correspond to one `Post`.

To create a new `Post` with a single `Comment`, you could write:

{% tabs %}
{% tab title="JS" %}

```javascript
// Declare the types.
const Post = Moralis.Object.extend("Post");
const Comment = Moralis.Object.extend("Comment");

// Create the post
const myPost = new Post();
myPost.set("title", "I'm Hungry");
myPost.set("content", "Where should we go for lunch?");

// Create the comment
const myComment = new Comment();
myComment.set("content", "Let's do Sushirrito.");

// Add the post as a value in the comment
myComment.set("parent", myPost);

// This will save both myPost and myComment
myComment.save();
```

{% endtab %}

{% tab title="React" %}

```javascript
import { useNewMoralisObject } from "react-moralis";

export default function App() {
  const postObject = useNewMoralisObject("Post");
  const commentObject = useNewMoralisObject("Comment");

  const makePost = async () => {
    const postData = {
      title: "I'm Hungry",
      content: "Where should we go for lunch?",
    };

    const commentData = {
      content: "Let's do Sushirrito.",
      parent: await postObject.save(postData),
    };

    commentObject.save(commentData, {
      onSuccess: (comment) => console.log(comment),
      onError: (error) => console.log(error),
    });
  };

  return <button onClick={makePost}>Call The Code</button>;
}
```

{% endtab %}
{% endtabs %}

Internally, Moralis will store the referred-to object in just one place, to maintain consistency. You can also link objects using just their `objectId`'s like so:

```javascript
const post = new Post();
post.id = "1zEcyElZ80";

myComment.set("parent", post);
```

By default, when fetching an object, related `Moralis.Object`'s are not fetched. These objects’ values cannot be retrieved until they have been fetched like so:

```javascript
const post = fetchedComment.get("parent");
await post.fetch();
const title = post.get("title");
```

### <mark style="color:green;">Many-To-Many</mark> <a href="#many-to-many-relationships" id="many-to-many-relationships"></a>

Many-to-many relationships are modelled using `Moralis.Relation`. This works similar to storing an array of `Moralis.Object`'s in a key, except that you don’t need to fetch all of the objects in a relation at once. In addition, this allows `Moralis.Relation` to scale to many more objects than the array of `Moralis.Object` approach.

For example, a `User` may have many `Posts` that she might like. In this case, you can store the set of `Posts` that a `User` likes using `relation`. In order to add a `Post` to the “likes” list of the `User`, you can do:

{% hint style="success" %}
<mark style="color:green;">**const relation = myObject.relation()**</mark>
{% endhint %}

```javascript
const user = Moralis.User.current();
const relation = user.relation("likes");
relation.add(post);
user.save();
```

You can remove a post from a `Moralis.Relation`:

{% hint style="success" %}
<mark style="color:green;">**relation.remove()**</mark>
{% endhint %}

{% code title="Example" %}

```javascript
relation.remove(post);
user.save();
```

{% endcode %}

You can call `add` and `remove` multiple times before calling save:

{% code title="Example" %}

```javascript
relation.remove(post1);
relation.remove(post2);
user.save();
```

{% endcode %}

You can also pass in an array of `Moralis.Object` to `add` and `remove`:

{% hint style="success" %}
<mark style="color:green;">**relation.add()**</mark>
{% endhint %}

{% code title="Example" %}

```javascript
relation.add([post1, post2, post3]);
user.save();
```

{% endcode %}

By default, the list of objects in this relation are not downloaded. You can get a list of the posts that a user likes by using the `Moralis.Query` returned by `query`. The code looks like:

{% hint style="success" %}
<mark style="color:green;">**relation.query()**</mark>
{% endhint %}

```
relation.query().find({success: function(list) {
    // list contains the posts that the current user likes.
  }
});
```

If you only want a subset of the posts, you can add extra constraints to the `Moralis.Query` returned by `query` like this:

```javascript
const query = relation.query();
query.equalTo("title", "I'm Hungry");
query.find({
  success: function (list) {
    // list contains post liked by the current user which have the title "I'm Hungry".
  },
});
```

For more details on `Moralis.Query`, please look at the [Queries](queries.md) portion of this guide. A `Moralis.Relation` behaves similar to an array of `Moralis.Object` for querying purposes, so any query you can do on an array of objects, you can do on a `Moralis.Relation`.

## Data Types

So far we’ve used values with type `String`, `Number`, and `Moralis.Object`. Moralis also supports `Date`s and `null`. You can nest `JSON Object`s and `JSON Array`s to store more structured data within a single `Moralis.Object`. Overall, the following types are allowed for each field in your object:

- String => `String`
- Number => `Number`
- Bool => `bool`
- Array => `JSON Array`
- Object => `JSON Object`
- Date => `Date`
- File => `Moralis.File`
- Pointer => other `Moralis.Object`
- Relation => `Moralis.Relation`
- Null => `null`

Some examples:

{% code title="Example" %}

```javascript
const number = 42;
const bool = false;
const string = "the number is " + number;
const date = new Date();
const array = [string, number];
const object = { number: number, string: string };
const pointer = MyClassName.createWithoutData(objectId);

const BigObject = Moralis.Object.extend("BigObject");
const bigObject = new BigObject();
bigObject.set("myNumber", number);
bigObject.set("myBool", bool);
bigObject.set("myString", string);
bigObject.set("myDate", date);
bigObject.set("myArray", array);
bigObject.set("myObject", object);
bigObject.set("anyKey", null); // this value can only be saved to an existing key
bigObject.set("myPointerKey", pointer); // shows up as Pointer &lt;MyClassName&gt; in the Data Browser
bigObject.save();
```

{% endcode %}

We do not recommend storing large pieces of binary data like images or documents on `Moralis.Object`. We recommend you use `Moralis.File` to store images, documents, and other types of files. You can do so by instantiating a `Moralis.File` object and setting it on a field. See [Files](../files/files.md) for more details.

For more information about how Moralis handles data, check out our documentation on [Data](data.md).

## Tutorial

{% hint style="info" %}
Legacy UI could be present in these videos, some things might be slightly different
{% endhint %}

{% embed url="https://www.youtube.com/watch?v=ypbXe91CTkA" %}
Unity Web3 Database - Storing Offchain Data with Moralis
{% endembed %}

{% embed url="https://youtu.be/kvn5CVjOfxg" %}
What are Moralis Objects?
{% endembed %}

{% embed url="https://youtu.be/Ny6Y42OOv_4" %}
Update Moralis Objects
{% endembed %}

{% embed url="https://youtu.be/ytJp0DBZKwA" %}
Work with Relations
{% endembed %}
