---
description: Bulk Queries are optimised to do large numbers of database operations fast.
---

# Bulk Queries

If you need to insert, update or delete many rows at once it's recommended to use Bulk Queries.

{% hint style="info" %}
**Bulk Queries are fast and efficient but they don't trigger any** [**LiveQuery**](live-queries.md) **events or any** [**Triggers**](../cloud-code/triggers.md)**.**
{% endhint %}

Because Bulk Queries have no overhead they are very fast and are executed directly on the database. Bulk Queries only work in [Cloud Code](../cloud-code/).

**When to use:**

{% hint style="success" %}
If you need to run fast queries on the database without triggering Live Queries or Triggers.
{% endhint %}

**When not to use:**

{% hint style="danger" %}
If you need Live-Queries or Triggers to work as a result of these queries.
{% endhint %}

## Bulk Write

Inserts one or several objects into a Class.

#### Options:

* `className`: Class name in which you want to insert rows
* `rows:` Array of rows to be inserted, each row needs to have `update` object containing the column data

```javascript
// insert these 2 rows into the database 
let foodsToInsert = [
  { update: { name: "Apple", color: "green" } },
  { update: { name: "Orange", color: "orange" } },
];
                     
Moralis.bulkWrite("Food", foodsToInsert);
```

## Bulk Update

Updates one or several columns on the first found object for each filter.

#### Options:

* `className`: Class name in which you want to insert rows
* `filters:` Array of updates to make, each update needs to specify `filter` and `update` objects. The former is specifying which column to base the selection on and the latter is specifying which column to update in the selection.

{% hint style="info" %}
**Note:** this query expects always the filter to return 1 row for each update, if multiple rows are returned only the first one will be updated.
{% endhint %}

```javascript
// update the first Food where name is Apple and set color to red 
// also update the first Food where name is Lemon and set color to yellow
let foodsToUpdate = [
  { filter: { name: "Apple" }, update: { color: "red" } },
  { filter: { name: "Lemon" }, update: { color: "yellow" } },
];
                     
Moralis.bulkUpdate("Food", foodsToUpdate);
```

## Bulk Update Many

Updates one or several columns on all found objects for each filter.

#### Options:

* `className`: Class name in which you want to insert rows
* `filters:` Array of updates to make, each update needs to specify `filter` and `update` objects. The former is specifying which column to base the selection on and the latter is specifying which column to update in the selection.

```javascript
// update the all Food where name is Apple and set color to red 
// also update all Food where name is Lemon and set color to yellow
let foodsToUpdate = [
  { filter: { name: "Apple" }, update: { color: "red" } },
  { filter: { name: "Lemon" }, update: { color: "yellow" } },
];
                     
Moralis.bulkUpdateMany("Food", foodsToUpdate);
```

## Bulk Delete

Deletes the first found object for each filter.

#### Options:

* `className`: Class name in which you want to insert rows
* `filters:` Array of updates to make, each update needs to specify `filter` and `update` objects. The former is specifying which column to base the selection on and the latter is specifying which column to update in the selection.

{% hint style="info" %}
**Note:** this query expects always the filter to return 1 row for each update, if multiple rows are returned only the first one will be deleted.
{% endhint %}

```javascript
// delete the first Food where name is Apple
// also delete the first Food where color is purple 
let foodsToDelete = [{filter: {"name" : "Apple"}},
                     {filter: {"color" : "purple"}}]
                     
Moralis.bulkDelete("Food", foodsToDelete)
```

## Bulk Delete Many

Deletes all objects found for each filter.

#### Options:

* `className`: Class name in which you want to insert rows
* `filters:` Array of updates to make, each update needs to specify `filter` and `update` objects. The former is specifying which column to base the selection on and the latter is specifying which column to update in the selection.

```javascript
// deletes all Food where name is Apple
// also delete all Food where color is purple 
let foodsToDelete = [
  { filter: { name: "Apple" } },
  { filter: { color: "purple" } },
];
                     
Moralis.bulkDeleteMany("Food", foodsToDelete);
```
