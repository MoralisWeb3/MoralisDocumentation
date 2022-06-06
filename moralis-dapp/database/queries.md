---
description: Querying Data in Moralis Server.
---

# Queries

We've already seen how a `Moralis.Query` with `get` can retrieve a single `Moralis.Object` from Moralis. There are many other ways to retrieve data with `Moralis.Query`. You can retrieve multiple objects at once, put conditions on the objects you wish to retrieve, and more.

## Basic Queries

In many cases, `get` isn't powerful enough to specify which objects you want to retrieve. `Moralis.Query` offers different ways to retrieve a list of objects rather than just a single object.

The general pattern is to create a `Moralis.Query`, put conditions on it, and then retrieve an `Array` of matching `Moralis.Object`s using `find`. For example, to retrieve the monster that have a particular `ownerName`, use the `equalTo` method to constrain the value for a key.

{% tabs %}
{% tab title="JS" %}
```javascript
const Monster = Moralis.Object.extend("Monster");
const query = new Moralis.Query(Monster);
query.equalTo("ownerName", "Aegon");
const results = await query.find();
alert("Successfully retrieved " + results.length + " monsters.");
// Do something with the returned Moralis.Object values
for (let i = 0; i < results.length; i++) {
  const object = results[i];
  alert(object.id + " - " + object.get("ownerName"));
}
```
{% endtab %}

{% tab title="React" %}
```javascript
import { useMoralisQuery } from "react-moralis";

export default function App() {
  const { fetch } = useMoralisQuery(
    "Monster",
    (query) => query.equalTo("ownerName", "Aegon"),
    [],
    { autoFetch: false }
  );

  const basicQuery = async () => {
    const results = await fetch();
    alert("Successfully retrieved " + results.length + " monsters.");
    // Do something with the returned Moralis.Object values
    for (let i = 0; i < results.length; i++) {
      const object = results[i];
      alert(object.id + " - " + object.get("ownerName"));
    }
  };

  return <button onClick={basicQuery}>Call The Code</button>;
}
```
{% endtab %}

{% tab title="Unity" %}
```csharp
using Moralis.Platform.Objects;
using MoralisWeb3ApiSdk;
using Newtonsoft.Json;

 public async void RetrieveObjectFromDB()
    {
        MoralisQuery<Monster> monster = Moralis.Query<Monster>().WhereEqualTo("ownerName", "Aegon");
        IEnumerable<Monster> result = await monster.FindAsync();
        foreach(Monster mon in result)
        {
            print("My strength is " + mon.strength + " and can i fly ? " + mon.canFly);
            // output : My strength is 1024 and can i fly ? true
        }
    }
```
{% endtab %}
{% endtabs %}

{% hint style="success" %}
For an introduction video tutorial [**Click Here**](queries.md#tutorials)
{% endhint %}

## Using Master Key

There are cases when a master key is needed for a query,

For example, if you want to get the list of all the users, you can only do that in a cloud function using a master key - <mark style="color:green;">**because a user can not read the info for the other users due to ACL**</mark>.

{% code title="Example" %}
```javascript
query.find({ useMasterKey: true });
```
{% endcode %}

## Query Constraints

{% hint style="success" %}
For a Query Constraints video tutorial [**Click Here**](queries.md#tutorials)
{% endhint %}

There are several ways to put constraints on the objects found by a `Moralis.Query`. You can filter out objects with a particular key-value pair with `notEqualTo`:

```javascript
query.notEqualTo("ownerName", "Daenerys");
```

You can give **multiple constraints**, and objects will only be in the results if they match all of the constraints. In other words, it's like an **AND** of constraints.

```javascript
query.notEqualTo("ownerName", "Daenerys");
query.greaterThan("ownerAge", 18);
```

You can **limit the number of results** by setting `limit`. By default, results are limited to 100. In the old Moralis hosted backend, the maximum limit was 1,000, but Moralis Dapps now removed that constraint:

```javascript
query.limit(10); // limit to at most 10 results
```

If you want **exactly one result**, a more convenient alternative may be to use `first` instead of using `find`.

{% tabs %}
{% tab title="JS" %}
```javascript
const Monster = Moralis.Object.extend("Monster");
const query = new Moralis.Query(Monster);
query.equalTo("ownerEmail", "daenerys@housetargaryen.com");
const object = await query.first();
```
{% endtab %}

{% tab title="React" %}
```javascript
import { useMoralisQuery } from "react-moralis";

export default function App() {
  const { fetch } = useMoralisQuery(
    "Monster",
    (query) =>
      query.equalTo("ownerEmail", "daenerys@housetargaryen.com").first(),
    [],
    { autoFetch: false }
  );

  const singleQuery = () =>
    fetch({
      onSuccess: (result) => console.log(result),
    });

  return <button onClick={singleQuery}>Call The Code</button>;
}
```
{% endtab %}
{% endtabs %}

You can **skip the first results** by setting <mark style="color:green;">`skip`</mark>. In the old Moralis hosted backend, the maximum skip value was 10,000, but Moralis Dapps now removed that constraint. This can be useful for **pagination**:

```javascript
query.skip(10); // skip the first 10 results
```

{% hint style="success" %}
For a Query pagination video tutorial [**Click Here**](queries.md#tutorials)
{% endhint %}

To get the **total number of rows** in a table satisfying your query, for e.g. pagination purposes - you can use <mark style="color:green;">`withCount`</mark>.

{% hint style="info" %}
**Note:** Enabling this flag will change the structure of the response, see the example below.
{% endhint %}

Example - Let's say you have 200 rows in a table called `Monster`:

{% tabs %}
{% tab title="JS" %}
```javascript
const Monster = Moralis.Object.extend("Monster");
const query = new Moralis.Query(Monster);

query.limit(25);

const results = await query.find(); // [ Monster, Monster, ...]

// to include count:
query.withCount();
const response = await query.find(); // { results: [ Monster, ... ], count: 200 }
```
{% endtab %}

{% tab title="React" %}
```javascript
const limitQuery = useMoralisQuery("Monster", (query) => query.limit(25), [], {
  autoFetch: false,
});

const getLimitedQuery = () =>
  limitQuery.fetch({
    onSuccess: (result) => console.log(result), // [ Monster, Monster, ...]
  });

const queryCount = useMoralisQuery(
  "Monster",
  (query) => query.withCount(),
  [],
  {
    autoFetch: false,
  }
);

const getQueryWithCount = () =>
  queryCount.fetch({
    onSuccess: (result) => console.log(result), // { results: [ Monster, ... ], count: 200 }
  });
```
{% endtab %}
{% endtabs %}

{% hint style="warning" %}
Сount operations can be slow and expensive.
{% endhint %}

If you only want to get the count without objects - use [Counting Objects](queries.md#counting-objects).

To **sort** on sortable types like numbers and strings, you can control the order in which results are returned:

```javascript
// Sorts the results in ascending order by the strength field
query.ascending("strength");

// Sorts the results in descending order by the strength field
query.descending("strength");
```

To do **comparisons in queries** for sortable types :

```javascript
// Restricts to wins < 50
query.lessThan("wins", 50);

// Restricts to wins <= 50
query.lessThanOrEqualTo("wins", 50);

// Restricts to wins > 50
query.greaterThan("wins", 50);

// Restricts to wins >= 50
query.greaterThanOrEqualTo("wins", 50);
```

To **retrieve objects matching in a list of values**, you can use <mark style="color:green;">`containedIn`</mark>, providing an array of acceptable values. This is often useful to replace multiple queries with a single query.

For example- if you want to retrieve monsters owned by any monster owner in a particular list:

```javascript
// Finds monsters from any of Jonathan, Dario, or Shawn
query.containedIn("ownerName", [
  "Jonathan Walsh",
  "Dario Wunsch",
  "Shawn Simon",
]);
```

To **retrieve objects that do not match any of several values**, you can use <mark style="color:green;">`notContainedIn`</mark>, providing an array of acceptable values.

For example, if you want to retrieve monsters from monster owners besides those in a list:

```javascript
// Finds monsters from anyone who is neither Jonathan, Dario, nor Shawn
query.notContainedIn("ownerName", [
  "Jonathan Walsh",
  "Dario Wunsch",
  "Shawn Simon",
]);
```

To **retrieve objects that have a particular key set**, you can use <mark style="color:green;">`exists`</mark>. Conversely, if you want to **retrieve objects without a particular key set**, you can use <mark style="color:green;">`doesNotExist`</mark>.

```javascript
// Finds objects that have the strength set
query.exists("strength");

// Finds objects that don't have the strength set
query.doesNotExist("strength");
```

To **get objects where a key matches the value of a key in a set of objects resulting from another query.** You can use the <mark style="color:green;">`matchesKeyInQuery`</mark> method.

For example, if you have a class containing sports teams and you store a user's hometown in the user class, you can issue one query to find the list of users whose hometown teams have winning records. The query would look like this:

```javascript
const Team = Moralis.Object.extend("Team");
const teamQuery = new Moralis.Query(Team);
teamQuery.greaterThan("winPct", 0.5);
const userQuery = new Moralis.Query(Moralis.User);
userQuery.matchesKeyInQuery("hometown", "city", teamQuery);
// results has the list of users with a hometown team with a winning record
const results = await userQuery.find();
```

Conversely, to **get objects where a key does not match the value of a key in a set of objects resulting from another query**, use <mark style="color:green;">`doesNotMatchKeyInQuery`</mark>. For example, to find users whose hometown teams have losing records:

```javascript
const losingUserQuery = new Moralis.Query(Moralis.User);
losingUserQuery.doesNotMatchKeyInQuery("hometown", "city", teamQuery);
// results has the list of users with a hometown team with a losing record
const results = await losingUserQuery.find();
```

To **filter rows based on `objectId`'s from pointers in a second table**, you can use dot notation:

```javascript
const rolesOfTypeX = new Moralis.Query("Role");
rolesOfTypeX.equalTo("type", "x");

const groupsWithRoleX = new Moralis.Query("Group");
groupsWithRoleX.matchesKeyInQuery(
  "objectId",
  "belongsTo.objectId",
  rolesOfTypeX
);
groupsWithRoleX.find().then(function (results) {
  // results has the list of groups with role x
});
```

To **restrict the fields returned** by calling <mark style="color:green;">`select`</mark> with a list of keys.

For example, To retrieve documents that contain only the `strength` and `ownerName` fields (and also special built-in fields such as `objectId`, `createdAt`, and `updatedAt`):

{% tabs %}
{% tab title="JS" %}
```javascript
const Monster = Moralis.Object.extend("Monster");
const query = new Moralis.Query(Monster);
query.select("strength", "ownerName");
query.find().then(function (monsters) {
  // each of the monsters will only have the selected fields available.
});
```
{% endtab %}

{% tab title="React" %}
```javascript
import { useMoralisQuery } from "react-moralis";

export default function App() {
  const { fetch } = useMoralisQuery(
    "Monster",
    (query) => query.select("strength", "ownerName"),
    [],
    { autoFetch: false }
  );

  const getSelectedQuery = () =>
    fetch({
      onSuccess: (monster) => {
        // each of the monsters will only have the selected fields available.
      },
    });

  return <button onClick={getSelectedQuery}>Call The Code</button>;
}
```
{% endtab %}
{% endtabs %}

Similarly, to **remove undesired fields while retrieving the rest** use <mark style="color:green;">`exclude`</mark>:

{% tabs %}
{% tab title="JS" %}
```javascript
const Monster = Moralis.Object.extend("Monster");
const query = new Moralis.Query(Monster);
query.exclude("ownerName");
query.find().then(function (monsters) {
  // Now each monster will have all fields except `ownerName`
});
```
{% endtab %}

{% tab title="React" %}
```javascript
import { useMoralisQuery } from "react-moralis";

export default function App() {
  const { fetch } = useMoralisQuery(
    "Monster",
    (query) => query.exclude("ownerName"),
    [],
    { autoFetch: false }
  );

  const queryExcludingParam = () =>
    fetch({
      onSuccess: (monsters) => {
        // Now each monster will have all fields except `ownerName`
      },
    });

  return <button onClick={queryExcludingParam}>Call The Code</button>;
}
```
{% endtab %}
{% endtabs %}

The remaining fields can be fetched later by calling `fetch` on the returned objects:

```javascript
query
  .first()
  .then(function (result) {
    // only the selected fields of the object will now be available here.
    return result.fetch();
  })
  .then(function (result) {
    // all fields of the object will now be available here.
  });
```

{% hint style="info" %}
Remember to make sure that addresses are in lowercase when making query constraints against an address column as described in [**Address Casing**](../automatic-transaction-sync/address-casing.md)
{% endhint %}

## Queries on Array Values

For keys with an array type, you can find objects where the key's array value contains 2 by:

```javascript
// Find objects where the array in arrayKey contains 2.
query.equalTo("arrayKey", 2);
```

You can also find objects where the key's array value contains each of the elements 2, 3, and 4 with the following:

```javascript
// Find objects where the array in arrayKey contains all of the elements 2, 3, and 4.
query.containsAll("arrayKey", [2, 3, 4]);
```

## Query Testing in the Dashboard

{% hint style="info" %}
**Note**: It's possible to run query code directly in the "Moralis Dashboard!". It can be found by navigating to "API Console > JS Console" in the menu. This is a great place to play around while first building your queries as it doesn't require any setup or initialization. It functions exactly like the JavaScript console in the browser except that it has direct access to the Moralis SDK and master key.
{% endhint %}

Type the query exactly as you would in the client or cloud code. Include a `console.log()` to print out the results then press the "Run" button. Some differences to watch out for:

* Need to use the `Parse` keyword instead of `Moralis`
  * i.e `new Parse.Query("EthTokenTransfers")`
  * This will likely be fixed in a future version (Moralis is a fork of Parse).
* Don't escape `$` in queries.
* You can use the master key - `const results = query.find({ useMasterKey: true })`

{% hint style="success" %}
The code can be saved between sessions by clicking "Save".
{% endhint %}

## Queries on String Values

#### StartsWith Search

Use <mark style="color:green;">**`startsWith`**</mark> to restrict to string values that start with a particular string. Similar to a MySQL LIKE operator, this is indexed so it's efficient for large datasets:

```javascript
// Finds barbecue sauces that start with "Big Daddy's".
const query = new Moralis.Query(BarbecueSauce);
query.startsWith("name", "Big Daddy's");
```

The above example will match any `BarbecueSauce` objects where the value in the "name" String key starts with "Big Daddy's". For example, both "Big Daddy's" and "Big Daddy's BBQ" will match, but "big daddy's" or "BBQ Sauce: Big Daddy's" will not.

Queries that have regular expression constraints are very expensive, especially for classes with over 100,000 records. Moralis restricts how many operations that can run on a particular app at any given time.

#### Full-Text Search

Use <mark style="color:green;">**`fullText`**</mark> for efficient search capabilities. Text indexes are automatically created for you. Your strings are turned into tokens for fast searching.

{% hint style="info" %}
**Note**: Full-Text Search can be resource-intensive. Ensure the cost of using indexes is worth the benefit, see [storage requirements & performance costs of text indexes.](https://docs.mongodb.com/manual/core/index-text/#storage-requirements-and-performance-costs)
{% endhint %}

```javascript
const query = new Moralis.Query(BarbecueSauce);
query.fullText("name", "bbq");
```

The above example will match any `BarbecueSauce` objects where the value in the "name" String key contains "bbq". For example, both "Big Daddy's BBQ", "Big Daddy's bbq", and "Big BBQ Daddy" will match.

```javascript
// You can sort by weight / rank. ascending() and select()
const query = new Moralis.Query(BarbecueSauce);
query.fullText("name", "bbq");
query.ascending("$score");
query.select("$score");
query
  .find()
  .then(function (results) {
    // results contains a weight / rank in result.get('score')
  })
  .catch(function (error) {
    // There was an error.
  });
```

{% hint style="info" %}
For "Case" or "Diacritic Sensitive" search, please use the [REST API](http://docs.parseplatform.org/rest/guide/#queries-on-string-values).
{% endhint %}

## Relational Queries

{% hint style="success" %}
For a Query pagination video tutorial [**Click Here**](queries.md#tutorials)
{% endhint %}

There are several ways to issue queries for relational data. **To retrieve objects where a field matches a particular `Moralis.Object`**, you can use <mark style="color:green;">**`equalTo`**</mark> just like for other data types.

For example, if each `Comment` has a `Post` object in its `post` field, you can fetch comments for a particular `Post`:

```javascript
// Assume Moralis.Object myPost was previously created.
const query = new Moralis.Query(Comment);
query.equalTo("post", myPost);
// comments now contains the comments for myPost
const comments = await query.find();
```

To **retrieve objects where a field contains a `Moralis.Object` that matches a different query**, you can use <mark style="color:green;">**`matchesQuery`**</mark>. In order to find comments for posts containing images, you can do:

```javascript
const Post = Moralis.Object.extend("Post");
const Comment = Moralis.Object.extend("Comment");
const innerQuery = new Moralis.Query(Post);
innerQuery.exists("image");
const query = new Moralis.Query(Comment);
query.matchesQuery("post", innerQuery);
// comments now contains the comments for posts with images.
const comments = await query.find();
```

To **retrieve objects where a field contains a `Moralis.Object` that does not match a different query**, you can use <mark style="color:green;">**`doesNotMatchQuery`**</mark>. In order to find comments for posts without images, you can do:

```javascript
const Post = Moralis.Object.extend("Post");
const Comment = Moralis.Object.extend("Comment");
const innerQuery = new Moralis.Query(Post);
innerQuery.exists("image");
const query = new Moralis.Query(Comment);
query.doesNotMatchQuery("post", innerQuery);
// comments now contains the comments for posts without images.
const comments = await query.find();
```

You can also do relational queries by `objectId`:

```javascript
const post = new Post();
post.id = "1zEcyElZ80";
query.equalTo("post", post);
```

To **return multiple types of related objects in one query** use the<mark style="color:green;">**`include`**</mark> method.

For example, let's say you are retrieving the last ten comments, and you want to retrieve their related posts at the same time:

```javascript
const query = new Moralis.Query(Comment);

// Retrieve the most recent ones
query.descending("createdAt");

// Only retrieve the last ten
query.limit(10);

// Include the post data with each comment
query.include("post");

// Comments now contains the last ten comments, and the "post" field
const comments = await query.find();
// has been populated. For example:
for (let i = 0; i < comments.length; i++) {
  // This does not require a network access.
  const post = comments[i].get("post");
}
```

To do **multi-level includes using dot notation**. Use the below query.

For example, If you wanted to include the post for a comment and the post's author as well you can do:

```javascript
query.include(["post.author"]);
```

{% hint style="success" %}
You can issue a query with multiple fields included by calling `include` multiple times. This functionality also works with `Moralis.Query` helpers like `first` and `get`.
{% endhint %}

## Counting Objects

{% hint style="info" %}
**Note**: In the old Moralis hosted backend, count queries were rate limited to a maximum of 160 requests per minute. They also returned inaccurate results for classes with more than 1,000 objects. But, Moralis Dapp has removed both constraints and can count objects well above 1,000.
{% endhint %}

**To count how many objects match a query, but you do not need to retrieve all the objects that match**, you can use <mark style="color:green;">**`count`**</mark> instead of <mark style="color:green;">**`find`**</mark>.

For example, to count how many monsters have been owned by a particular monster owner:

```javascript
const Monster = Moralis.Object.extend("Monster");
const query = new Moralis.Query(Monster);
query.equalTo("ownerName", "Aegon");
const count = await query.count();
alert("Aegon has owned " + count + " monsters");
```

## Compound Queries

{% hint style="success" %}
For an advance Queries video tutorial [**Click Here**](queries.md#tutorials)
{% endhint %}

For more complex queries, you might need compound queries. A compound query is a logical combination (e. g. "and" or "or") of subqueries.

{% hint style="info" %}
**Note:** We do not support GeoPoint or non-filtering constraints (e.g. `near`, `withinGeoBox`, `limit`, `skip`, `ascending`/`descending`, `include`) in the subqueries of the compound query.
{% endhint %}

### OR-ed Query Constraints

To **find objects that match one of several queries**, you can use <mark style="color:green;">**`Moralis.Query.or`**</mark> method to construct a query that is an OR of the queries passed in.

For instance, if you want to find players who either have a lot of wins or a few wins, you can do:

```javascript
const lotsOfWins = new Moralis.Query("Player");
lotsOfWins.greaterThan("wins", 150);

const fewWins = new Moralis.Query("Player");
fewWins.lessThan("wins", 5);

const mainQuery = Moralis.Query.or(lotsOfWins, fewWins);
mainQuery
  .find()
  .then(function (results) {
    // results contains a list of players that either have won a lot of games or won only a few games.
  })
  .catch(function (error) {
    // There was an error.
  });
```

### AND-ed Query Constraints

To **find objects that match all conditions**, you normally would use just one query. You can add additional constraints to the newly created `Moralis.Query` that act as an 'and' operator.

```javascript
const query = new Moralis.Query("User");
query.greaterThan("age", 18);
query.greaterThan("friends", 0);
query
  .find()
  .then(function (results) {
    // results contains a list of users both older than 18 and having friends.
  })
  .catch(function (error) {
    // There was an error.
  });
```

Sometimes the world is more complex than this simple example and you may need a compound query of subqueries. You can use `Moralis.Query.and` method to construct a query that is an AND of the queries passed in.

For instance, if you want to find users in the age of 16 or 18 who have either no friends or at least two friends, you can do:

```javascript
const age16Query = new Moralis.Query("User");
age16Query.equalTo("age", 16);

const age18Query = new Moralis.Query("User");
age18Query.equalTo("age", 18);

const friends0Query = new Moralis.Query("User");
friends0Query.equalTo("friends", 0);

const friends2Query = new Moralis.Query("User");
friends2Query.greaterThan("friends", 2);

const mainQuery = Moralis.Query.and(
  Moralis.Query.or(age16Query, age18Query),
  Moralis.Query.or(friends0Query, friends2Query)
);
mainQuery
  .find()
  .then(function (results) {
    // results contains a list of users in the age of 16 or 18 who have either no friends or at least 2 friends
    // results: (age 16 or 18) and (0 or >2 friends)
  })
  .catch(function (error) {
    // There was an error.
  });
```

## Aggregate

Queries can be made using aggregates, allowing you to retrieve objects over a set of input values. The results will not be `Moralis.Object`s since you will be aggregating your own fields.

{% hint style="warning" %}
**`MasterKey`is Required.**
{% endhint %}

Aggregates use stages to filter results by piping results from one stage to the next. The output from the previous stages becomes the input for the next stage.

You can **create a pipeline using an Array or an Object**.

```javascript
const pipelineObject = {
  group: { objectId: "$name" },
};

const pipelineArray = [{ group: { objectId: "$name" } }];
```

{% hint style="success" %}
For a Query aggregates video tutorial [**Click Here**](queries.md#tutorials)
{% endhint %}

For a list of available stages please refer to [Mongo Aggregate Documentation](https://docs.mongodb.com/v3.2/reference/operator/aggregation/).

{% hint style="info" %}
**Note**: Most operations in the [Mongo Aggregate Documentation](https://docs.mongodb.com) will work with Moralis Server, but `_id` do not exist. Please replace with `objectId`.
{% endhint %}

### Match

Match pipeline is similar to `equalTo`.

{% embed url="https://youtu.be/isl3JZwNVqg" %}

```javascript
const pipeline = [{ match: { name: "BBQ" } }];

const query = new Moralis.Query("User");

query
  .aggregate(pipeline)
  .then(function (results) {
    // results contains name that matches 'BBQ'
  })
  .catch(function (error) {
    // There was an error.
  });
```

You can match by comparison.

```javascript
const pipeline = [{ match: { score: { $gt: 15 } } }];

const query = new Moralis.Query("User");

query
  .aggregate(pipeline)
  .then(function (results) {
    // results contains score greater than 15
  })
  .catch(function (error) {
    // There was an error.
  });
```

You can read more about what operators are available in the [Mongo query operators docs](https://docs.mongodb.com/v3.6/reference/operator/query/).

### Lookup (Join)

The `lookup` pipeline is similar to `LEFT OUTER JOIN` in SQL. It will match documents in another collection and bring them into the results as an array property. Use this for collections that were not created with an explicit relation (see [Relational Data](objects.md#relational-data), [Relational Queries](queries.md#relational-queries)).

{% embed url="https://youtu.be/T7of3eP5CQ0" %}

For example, in a portfolio app, you might want to display all of a user's token transactions. In Moralis, these are stored in the `EthTokenTransfers` collection. This collection has all the info needed except the token name and number of decimals, which are needed to display this info in a nice way on the front-end. The extra info is in the `EthTokenBalance` collection and to get it we need a `lookup`. For more details check out the[ Mongo lookup docs](https://docs.mongodb.com/v3.6/reference/operator/aggregation/lookup/).

#### Simple Lookup (Single Equality)

Unfortunately for our use case, the`EthTokenBalance` collection has info for not just our current user, but all users. This means the token name is repeated for every user that also holds that token. This means joining on more than one attribute and it requires a different syntax which will be covered below. For now, let's assume we have another collection called `Token` which looks like this:

```javascript
Token {
  objectId: string,
  token_address: string,
  symbol: string,
  name: string,
  decimals: number
}
```

Then you would define a cloud function like this (aggregate queries must be run in cloud code).

{% hint style="success" %}
**Remember to escape the `$` like `\$` to prevent parsing errors** <mark style="color:green;">(this is now fixed!)</mark>.
{% endhint %}

```javascript
// this goes in the Moralis server Cloud Functions section
Moralis.Cloud.define("getUserTokenTransfers", function (request) {
  const userAddress = request.params.userAddress;
  const query = new Moralis.Query("EthTokenTransfers");

  const pipeline = [
    // only transfers to or from userAddress
    {
      match: {
        $expr: {
          $or: [
            { $eq: ["$from_address", userAddress] },
            { $eq: ["$to_address", userAddress] },
          ],
        },
      },
    },

    // join to Token collection on token_address
    {
      lookup: {
        from: "Token",
        localField: "token_address",
        foreignField: "token_address",
        as: "token",
      },
    },
  ];

  return query.aggregate(pipeline);
});
```

The output would look like the following where `as: "token"` specifies the name for the output array of matching documents in the `Token` table. This array will have a length greater than 1 if the join is 1-to-many instead of 1-to-1.

```javascript
[
  {
    // EthTokenTransfer collection attributes
    "block_timestamp":{...}
    "token_address": "0x8762db106b2c2a0bccb3a80d1ed41273552616e8"
    "from_address": "0xba65016890709dbc9491ca7bf5de395b8441dc8b"
    "to_address": "0x29781d9fca70165cbc952b8c558d528b85541f0b"
    "value": "29803465370630263212090"
    "transaction_hash": "0xfc611dbae7c67cd938fca58a0cc2fe46d224f71b91472d4c9241e997e6440ac1"
    "token": [
      {
        // Token collection attributes from lookup
        "_id": "HKA8dzHG1A"
        "token_address": "0x8762db106b2c2a0bccb3a80d1ed41273552616e8"
        "symbol": "RSR"
        "name": "Reserve Rights"
        "decimals": 18
      }
    ]
  }
]
```

#### Complex Join (multiple equality)

Joining on multiple attributes requires a nested pipeline in the `lookup`, but gives more flexibility when defining how the collections should be joined. The above `query`can be re-written to join `EthTokenTranfers` with `EthTokenBalance`.

```javascript
// this goes in the Moralis server Cloud Functions section
Moralis.Cloud.define("getUserTokenTransfers", function(request) {
  const userAddress = request.params.userAddress;
  const query = new Moralis.Query("EthTokenTransfers");

  const pipeline = [
    // only transfers to or from userAddress
    {match: {$expr: {$or: [
      {$eq: ["$from_address", userAddress]},
      {$eq: ["$to_address", userAddress]},
    ]}}},

      // join to EthTokenBalance on token_address and userAddress
    {lookup: {
      from: "EthTokenBalance",
      let: { tokenAddress: "$token_address", userAddress: userAddress},
      pipeline: [
        {$match: {$expr: {$and: [
          { $eq: ["$token_address", "$$tokenAddress"] },
          { $eq: ["$address", "$$userAddress"] },
      ]}}],
      as: "EthTokenBalance",
    }}
  ];

  return query.aggregate(pipeline);
});
```

The `lookup` pipeline uses a `match` stage to specify the additional join conditions. Also, take note of the `let` which defines variables to be used by the lookup pipeline. This is required as the lookup pipeline does not have direct access to the attributes in the original collection. See the _"Specify Multiple Join Conditions with $lookup"_ section in the [MongoDB $lookup docs](https://docs.mongodb.com/v3.6/reference/operator/aggregation/lookup/) for more info.

### Project (Select)

Project pipeline is similar to `keys` or `select`, add or remove existing fields.

{% embed url="https://youtu.be/8ksKYxMiTZw" %}

```javascript
const pipeline = [{ project: { name: 1 } }];

const query = new Moralis.Query("User");

query
  .aggregate(pipeline)
  .then(function (results) {
    // results contains only name field
  })
  .catch(function (error) {
    // There was an error.
  });
```

### Group

{% embed url="https://youtu.be/xAoBRCoAhMU" %}

Group pipeline is similar to `distinct`.

You can group by field:

```javascript
// score is the field. $ before score lets the database know this is a field
const pipeline = [{ group: { objectId: "$score" } }];

const query = new Moralis.Query("User");

query
  .aggregate(pipeline)
  .then(function (results) {
    // results contains unique score values
  })
  .catch(function (error) {
    // There was an error.
  });
```

You can apply collective calculations like $sum, $avg, $max, $min.

```javascript
// total will be a newly created field to hold the sum of score field
const pipeline = [{ group: { objectId: null, total: { $sum: "$score" } } }];

const query = new Moralis.Query("User");

query
  .aggregate(pipeline)
  .then(function (results) {
    // results contains sum of score field and stores it in results[0].total
  })
  .catch(function (error) {
    // There was an error.
  });
```

If you want to perform calculations on a column that contains numerical values but they're stored as strings in the database, you have to cast the value to a numerical data type. For example, a uint256 stored in a string column should be converted with `$toLong`.

```javascript
 { group: { objectId: '$nftItem',  priceOfAllNfts: { $sum: {$toLong : '$price'} } } },
```

The collection of documents matching the grouping field can be accessed with `$$ROOT`, and can be pushed into an array using the `$push` accumulator.

```javascript
// games will be an array of all the games played by each unique player
const pipeline = [
  { group: { objectId: "$playerId", games: { $push: "$$ROOT" } } },
];

const query = new Moralis.Query("GameRound");

query
  .aggregate(pipeline)
  .then(function (results) {
    // results contains a collection of players with their respective games played
  })
  .catch(function (error) {
    // There was an error.
  });
```

You can group by the column of a child document by using dot notation. However, you must wrap it in quotation marks.

```javascript
 { group: { objectId: '$player.username',  games: { $push: "$$ROOT" } } },
```

### Sort

{% embed url="https://youtu.be/aUZfictZevc" %}

The sorting stage is used to sort the results by a given column in a specific order.

To specify sort order, `1` is used to sort in ascending order, and `-1` is used to sort in descending order.

```javascript
// This will sort the rows in ascending order based on the username field
{
  sort: {
    username: 1;
  }
}
```

You can sort by several columns in a given order.

```javascript
 // This will sort the rows in ascending order based on the country field,
 // and then in descending order on the city field
 { sort : { country: 1, city: -1 } }
```

### Skip

The skip stage skips over the specified number of rows that is passed into the stage. This is often used when implementing pagination functionality.

```javascript
// This stage will skip the first 5 rows passed into this stage
{
  skip: 5;
}
```

### Limit

The limit stage only includes the first `n` number of rows that are passed into the stage.

```javascript
// This stage will throw away all rows except the first 5
{
  limit: 5;
}
```

### Count

The count stage returns the number of rows passed into the stage assigned to a variable name.

```javascript
// This stage will return the number of rows passed into the stage
// and store it in the variable numberOfNfts
{
  count: "numberOfNfts";
}
```

## Distinct

Queries can be made using `distinct`, allowing you to find unique values for a specified field.

{% hint style="warning" %}
**`MasterKey`is required.**
{% endhint %}

```javascript
const query = new Moralis.Query("User");

query
  .distinct("age")
  .then(function (results) {
    // results contains unique age
  })
  .catch(function (error) {
    // There was an error.
  });
```

You can also restrict results by using `equalTo`.

```javascript
const query = new Moralis.Query("User");
query.equalTo("name", "foo");
query
  .distinct("age")
  .then(function (results) {
    // results contains unique age where name is foo
  })
  .catch(function (error) {
    // There was an error.
  });
```

## Read Preference

When using a MongoDB replica set, you can use the `query.readPreference(readPreference, includeReadPreference, subqueryReadPreference)` function to choose from which replica the objects will be retrieved. The `includeReadPreference` argument chooses from which replica the included pointers will be retrieved and the `subqueryReadPreference` argument chooses in which replica the subqueries will run. The possible values are `PRIMARY` (default), `PRIMARY_PREFERRED`, `SECONDARY`, `SECONDARY_PREFERRED`, or `NEAREST`. If the `includeReadPreference` argument is not passed, the same replica chosen for `readPreference` will also be used for the includes. The same rule applies for the `subqueryReadPreference` argument.

```javascript
query.readPreference("SECONDARY", "SECONDARY_PREFERRED", "NEAREST");
```

## Tutorials

{% embed url="https://youtu.be/l0NvTvNxpQo" %}
Introduction to Queries
{% endembed %}

{% embed url="https://youtu.be/XgGpXSYu2Us" %}
Query Constraints
{% endembed %}

{% embed url="https://youtu.be/2CaHZhgtsZc" %}
Query Pagination
{% endembed %}

{% embed url="https://youtu.be/vtNkic3e3AU" %}
Queries Relations
{% endembed %}

{% embed url="https://youtu.be/dKFY5w49Bbc" %}
Advanced Queries
{% endembed %}

{% embed url="https://youtu.be/MkC6ZSGI84g" %}
Queries using Aggregates
{% endembed %}
