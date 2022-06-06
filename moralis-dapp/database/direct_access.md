---
description: Access the database directly.
---

# Direct Access

### Connect directly to the Mongo DB instance that runs on your Moralis Server

You find the Mongo DB **IP and port** in your Moralis Dapp settings, you will also need to whitelist the IP that will connect to that mongo DB instance (also in Dapp settings).

#### Node.js

```javascript
const { MongoClient } = require('mongodb');

const MONGO_HOST = 'MONGO_HOST_IP_FROM_ADMIN_INTERFACE';
const MONGO_PORT = 'MONGO_HOST_PORT_FROM_ADMIN_INTERFACE';

// Create a new MongoClient
const client = new MongoClient(`mongodb://${MONGO_HOST}:${MONGO_PORT}`);

async function run() {
  try {
    // Connect the client to the server
    await client.connect();
    // Establish and verify connection
    await client.db('admin').command({ ping: 1 });
    console.log('Connected successfully to server');
  } finally {
    // Ensures that the client will close when you finish/error
    await client.close();
  }
}
run().catch(console.dir);
```

#### Python

```python
import pprint
import pymongo

MONGO_HOST = "MONGO_HOST_IP_FROM_ADMIN_INTERFACE"
MONGO_PORT = MONGO_HOST_PORT_FROM_ADMMIN_INTERFACE

con = pymongo.MongoClient(MONGO_HOST, MONGO_PORT)
user_table = con['parse']['_User']
pprint.pprint(user_table.find_one())
```

{% hint style="info" %}
**Note**: \_User table is a particular example of table that starts with \_, most of the tables will not start with \_.
{% endhint %}

{% hint style="success" %}
After you have established direct access to Mongo DB, you can do your own dumps for the database or use a tool with an interface to see what is in the database. You can also add indexes where needed.
{% endhint %}
