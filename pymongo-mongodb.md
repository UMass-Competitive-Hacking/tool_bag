# Using pymongo (mongoDB with Python)
## References:
**Database-level**:  http://api.mongodb.com/python/current/api/pymongo/database.html
**Collection-level**: http://api.mongodb.com/python/current/api/pymongo/collection.html

## Overview

### How Mongo works
MongoDB works with layers.  The layers go from Client -> Database -> Collections -> Documents

```
Client (client = MongoClient())
   |
   |
   V
Database (db = client.DatabaseName)
   |
   |
   V
Collections (coll = db.CollectionName)
   |
   |
   V
Documents (for item in coll:  print item)
```

**What do these mean?**

### Client
A Client is the primary pointer to Mongo in general.  Basically says "I'm writing code, and I want to use Mongo".  Client will act as your ambassador.

```python
from pymongo import MongoClient
client = MongoClient()
```

Now, we can see all the different databases we have to choose from with...

```python
client.database_names()
```

This will be a list like...

```
[u'local',u'test']
```

DON'T TOUCH 'local' or 'test'.  They are put there by default, and I don't know what deleting them will do (local is the base template used for db creation (duplication of the base template)).


### Database
The Database is the database; it stores your information in different 'folders' (collections)

```python
db = client.DatabaseName
```

This line both **selects** and **creates** a database (the latter, only if the database doesn't exist)

Get a list of your collections with...

```python
db.collection_names()
```

### Collections
Collections are an organizational layer for all your data
Think of these as the folders on you local system: you don't have EVERY SINGLE FILE in the C:\filename.file directory, instead it's in C:\some\path\filename.file.  That way, to find your code-porn stash you go into C:\Users\Hacker\Documents\DoNotOpen\Nope\Code\porn.file

In Mongo we make Collections (only one level, rather than infinite)

```python
docs = db.CollectionName
```

Where CollectionName is the name of you Collection

### Where these points leave us
After going down through the layers, we're left with links to:

````
client ---> MongoClient
db     ---> Specific DatabaseNames
coll   ---> Specific Collection in db
docs   ---> List of elements
```

Meaning to switch databases, all you need to do is make another instance of a pointer (newdb = client.DatabaseName2) or reset your db pointer to another Database (db = client.DatabaseName2)