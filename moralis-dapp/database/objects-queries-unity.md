---
description: Storing, Updating, Retrieving and Deleting Data in a Moralis Server on Unity.
---

# Objects

## Create MoralisObject

```csharp
//using namespaces
using Moralis.Platform.Objects;
using MoralisUnity.Platform.Queries;
using MoralisUnity;
using Newtonsoft.Json;

// code
public class Monster : MoralisObject
{
    public int strength { get; set; }
    public string Name { get; set; }
    public bool canFly { get; set; }
    // base class is required
    public Monster() : base("Monster"){}
}
public async void SaveObjectToDB()
    {
      try {
        Monster monster = await Moralis.Create<Monster>();
        monster.strength = 1024;
        monster.Name = "Aegon";
        monster.canFly = true;
        await monster.SaveAsync();
        print("Created new object");
      }
      catch (Exception e){
        print("Failed to create new object, with error code: " + e);
      }
    }

```

# Queries

### Basic Queries

Queries provide a way to retrieve information from your Moralis database

```csharp
//using namespaces
using Moralis.Platform.Objects;
using MoralisUnity.Platform.Queries;
using MoralisUnity;
using Newtonsoft.Json;

// code
 public async void RetrieveObjectFromDB()
    {
        MoralisQuery<Monster> monster = await Moralis.Query<Monster>();
        monster = monster.WhereEqualTo("Name", "Aegon");
        IEnumerable<Monster> result = await monster.FindAsync();
        foreach(Monster mon in result)
        {
            print("My strength is " + mon.strength + " and can i fly ? " + mon.canFly);
            // output : My strength is 1024 and can i fly ? true
        }
    }
```

### Updating an Object

updating objects in the db

```csharp
public async void updateObjectInDB()
    {
        MoralisQuery<Monster> monster = await Moralis.Query<Monster>();
        monster = monster.WhereEqualTo("Name", "Aegon");
        IEnumerable<Monster> result = await monster.FindAsync();
        foreach(Monster mon in result)
        {
            mon.Name = "the_great_mage";
            mon.strength = 6000;
            await mon.SaveAsync();
            Debug.Log("The monster is " + mon.Name + " with " + mon.strength + " strength");
            // output : The monster is the_great_mage with 6000 strength
        }
    }
```

you create a query instance

```csharp
MoralisQuery<Monster> monster = await Moralis.Query<Monster>()
```

### Query params / Query constraints

you can search for different params like:

<details>
<summary> Query params / Query constraints</summary>

{% embed url="https://github.com/MoralisWeb3/web3-unity-sdk/blob/main/Runtime/Core/Platform/Queries/MoralisQuery.cs" %}
Moralis Query
{% endembed %}

code syntax is below description

```csharp
// Sorts the results in ascending order by the given key.
OrderBy(string key)
// Sorts the results in descending order by the given key.
OrderByDescending(string key)
// Sorts the results in ascending order by the given key, after previous ordering has been applied.
ThenBy(string key)
//Sorts the results in descending order by the given key, after previous ordering has been applied.
ThenByDescending(string key)
// Include nested MoralisObjects for the provided key. You can use dot notation to specify which fields in the included objects should also be fetched.
Include(string key)
//Restrict the fields of returned MoralisObjects to only include the provided key. If this is called multiple times, then all of the keys specified in each of the calls will be included
Select(string key)
//Skips a number of results before returning. This is useful for pagination of large queries
Skip(int count)
// Controls the maximum number of results that are returned
Limit(int count)
// Adds a constraint to the query that requires a particular key's value to be contained in the provided list of values.
WhereContainedIn<TIn>(string key, IEnumerable<TIn> values)
// Add a constraint to the querey that requires a particular key's value to be a list containing all of the elements in the provided list of values.
WhereContainsAll<TIn>(string key, IEnumerable<TIn> values)
// Adds a constraint for finding string values that contain a provided string. This will be slow for large data sets.
WhereContains(string key, string substring)
// Adds a constraint for finding objects that do not contain a given key.
WhereDoesNotExist(string key)
// Adds a constraint to the query that requires that a particular key's valuedoes not match another MoralisQuery. This only works on keys whose values are MoralisObjects or lists of MoralisObjects.
WhereDoesNotMatchQuery<TOther>(string key, MoralisQuery<TOther> query)
// Adds a constraint for finding string values that end with a provided string. This will be slow for large data sets.
WhereEndsWith(string key, string suffix)
// Adds a constraint to the query that requires a particular key's value to be equal to the provided value.
WhereEqualTo(string key, object value)
// Adds a constraint for finding objects that contain a given key.
WhereExists(string key)
// Adds a constraint to the query that requires a particular key's value to be greater than the provided value.
WhereGreaterThan(string key, object value)
// Adds a constraint to the query that requires a particular key's value to be greater or equal to than the provided value.
WhereGreaterThanOrEqualTo(string key, object value)
// Adds a constraint to the query that requires a particular key's value to be less than the provided value
WhereLessThan(string key, object value)
// Adds a constraint to the query that requires a particular key's value to be less than or equal to the provided value
WhereLessThanOrEqualTo(string key, object value)
// Adds a regular expression constraint for finding string values that match the provided regular expression. This may be slow for large data sets.
WhereMatches(string key, Regex regex, string modifiers)
// Adds a regular expression constraint for finding string values that match the provided regular expression. This may be slow for large data sets
WhereMatches(string key, Regex regex)
WhereMatches(string key, string pattern, string modifiers = null)
WhereMatches(string key, string pattern)
// Adds a constraint to the query that requires a particular key's value to match a value for a key in the results of another MoralisQuery
WhereMatchesKeyInQuery<TOther>(string key, string keyInQuery, MoralisQuery<TOther> query)
// Adds a constraint to the query that requires a particular key's value does not match any value for a key in the results of another MoralisQuery
WhereDoesNotMatchesKeyInQuery<TOther>(string key, string keyInQuery, MoralisQuery<TOther> query)
// Adds a constraint to the query that requires that a particular key's value matches another MoralisQuery. This only works on keys whose values are MoralisObjects or lists of MoralisObjects
WhereMatchesQuery<TOther>(string key, MoralisQuery<TOther> query)
/// Adds a proximity-based constraint for finding objects with keys whose GeoPoint values are near the given point
WhereNear(string key, MoralisGeoPoint point)
// Adds a constraint to the query that requires a particular key's value to be contained in the provided list of values
WhereNotContainedIn<TIn>(string key, IEnumerable<TIn> values)
// Adds a constraint to the query that requires a particular key's value not to be equal to the provided value
WhereNotEqualTo(string key, object value)
// Adds a constraint for finding string values that start with the provided string. This query will use the backend index, so it will be fast even with large data sets
WhereStartsWith(string key, string suffix)
// Add a constraint to the query that requires a particular key's coordinates to be contained within a given rectangular geographic bounding box
WhereWithinGeoBox(string key, MoralisGeoPoint southwest, MoralisGeoPoint northeast)
// Adds a proximity-based constraint for finding objects with keys whose GeoPoint values are near the given point and within the maximum distance given
WhereWithinDistance(string key, MoralisGeoPoint point, MoralisGeoDistance maxDistance)
```

</details>

### compound queries

```csharp
MoralisQuery<Monster> monster = await Moralis.Query<Monster>();
MoralisQuery<Monster> monsterStrength = monster.WhereGreaterThanOrEqualTo("strength", 200 );
MoralisQuery<Monster> monstercanFly = monster.WhereEqualTo("canFly", false);
MoralisQuery<Monster> monster1 = Moralis.BuildOrQuery<PlayerData>(new MoralisQuery<PlayerData>[] { monsterName,monsterSkill }).OrderByDescending("strength");
IEnumerable<PlayerData> result = await monster1.FindAsync();
```

### Moralis User object

This is an object reserve for the \_User class that handles user data in the dapp

```csharp
MoralisUser user = Moralis.Create<MoralisUser>();
```
