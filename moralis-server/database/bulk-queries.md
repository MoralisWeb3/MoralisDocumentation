---
description: Bulk Queries are optimised to do large numbers of database operations fast.
---

# ⚡ Bulk Queries

If you need to insert, update or delete many rows at once it's recommended to use Bulk Queries.

**Bulk Queries are fast and efficient but they don't trigger any** [**LiveQuery**](live-queries.md) **events nor any** [**Triggers**](../cloud-code/triggers.md)**.**

Because Bulk Queries have no overhead they are very fast and are executed directly on the database.

**When to use:**

If you need to run fast queries on the database without triggering Live Queries or Triggers.

**When not to use:**

If you need Live-Queries or Triggers to work as result of these queries.

## Bulk Write

Inserts one or several objects into a Class.&#x20;

#### Options:

* `className`: Class name in which you want to insert rows
* `rows:` Array of rows to be inserted, each row needs to have `update` object containing the column data

```javascript
// insert these 2 rows into the database 
let foodsToInsert = [{update: {"name" : "Apple", "color" : "green"},
                     {update: {"name" : "Orange", "color" : "orange"}]
                     
Moralis.bulkWrite("Food", foodsToInsert)
```

## Bulk Update

Updates several columns on the first found object for each filter.

If the object doesn't exist it gets created.

#### Options:

* `className`: Class name in which you want to insert rows
* `filters:` Array of updates to make, each update needs to specify `filter` and `update` objects. The former is specifying which column to base the selection on and the latter is specifying which column to update in the selection.

**Note:** this query expects always the filter to return 1 row for each update, if multiple rows are returned only the first one will be updated.

```javascript
// update the first Food where name is Apple and set color to red 
// also update the first Food where name is Lemon and set color to yellow
let foodsToUpdate = [{filter: {"name" : "Apple"}, update:{ "color" : "red"}},
                     {filter: {"name" : "Lemon"}, update:{ "color" : "yellow"}}]
                     
Moralis.bulkUpdate("Food", foodsToUpdate)
```

## Bulk Update Many

Updates several columns on all found objects for each filter.

If the objects doesn't exist they get created.

#### Options:

* `className`: Class name in which you want to insert rows
* `filters:` Array of updates to make, each update needs to specify `filter` and `update` objects. The former is specifying which column to base the selection on and the latter is specifying which column to update in the selection.

```javascript
// update the all Food where name is Apple and set color to red 
// also update all Food where name is Lemon and set color to yellow
let foodsToUpdate = [{filter: {"name" : "Apple"}, update:{ "color" : "red"}},
                     {filter: {"name" : "Lemon"}, update:{ "color" : "yellow"}}]
                     
Moralis.bulkUpdateMany("Food", foodsToUpdate)
```

## Bulk Delete

Deletes the first found object for each filter.

#### Options:

* `className`: Class name in which you want to insert rows
* `filters:` Array of updates to make, each update needs to specify `filter` and `update` objects. The former is specifying which column to base the selection on and the latter is specifying which column to update in the selection.

**Note:** this query expects always the filter to return 1 row for each update, if multiple rows are returned only the first one will be deleted.

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
let foodsToDelete = [{filter: {"name" : "Apple"}},
                     {filter: {"color" : "purple"}}]
                     
Moralis.bulkDeleteMany("Food", foodsToDelete)
```
