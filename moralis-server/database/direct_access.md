
# How to connect directly to the Mongo DB instance that runs on your Moralis Server

You find the IP and port where to connect to Mongo DB in your Moralis server settings, 
you will also need to whitelist the IP that will connect to that mongo db instance (also in server settings).
After that you can connect to that database and make queries directly there, for example this is a python script that makes a simple query:

```python
import pprint
import pymongo

MONGO_HOST = "MONGO_HOST_IP_FROM_ADMIN_INTERFACE"
MONGO_PORT = MONGO_HOST_PORT_FROM_ADMMIN_INTERFACE

con = pymongo.MongoClient(MONGO_HOST, MONGO_PORT)
user_table = con['parse']['_User']
pprint.pprint(user_table.find_one())
```

Note: \_User table is a particular example of table that starts with \_, most of the tables will not start with \_.

After you have direct access to mongo db, you can do your own dumps for the database or use a tool with interface to see what is in the database.
You can also add indexes where needed.
