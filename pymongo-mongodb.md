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
coll = db.CollectionName
```

Where CollectionName is the name of you Collection.


### Documents
Finally, documents are the individual elements in your database (each member of a roster, etc.).

To list your documents, you need to iterate over a Collection's find() object

```python
for doc in coll.find():
     print doc
```

Selecting individual documents can be done by passing find() a part of for what you are looking.
e.g.: Say we have a collection (pointer: coll) that has these documents:
```
>>> for d in coll.find(): print d

{u'x': 1, u'_id': ObjectId('58234f9cdfd09406909f9887')}
{u'y': 1, u'_id': ObjectId('58234fc3dfd09406909f9888')}
{u'_id': ObjectId('58234fc7dfd09406909f9889'), u'z': 1}
{u'a': 5, u'_id': ObjectId('58234fccdfd09406909f988a')}
```

You can select the 2nd document ('y':1) by passing some key:value pair into the find() function that's from the collection base.

```
>>> for doc in coll.find( {'y':1} ):
...    print doc

{u'y': 1, u'_id': ObjectId('58234fc3dfd09406909f9888')}
```

**Updating**
If you want to update a single key:value pair (non-list), you can do so with ```update_one()```:
```
>>> for d in coll.find(): print d

{u'x': 1, u'_id': ObjectId('58234f9cdfd09406909f9887')}
{u'y': 1, u'_id': ObjectId('58234fc3dfd09406909f9888')}
{u'_id': ObjectId('58234fc7dfd09406909f9889'), u'z': 1}
{u'a': 5, u'_id': ObjectId('58234fccdfd09406909f988a')}

>>> coll.update( {'y' : 1}, {"$inc" : { 'y' : 3 }})
>>> for d in coll.find(): print d

{u'x': 1, u'_id': ObjectId('58234f9cdfd09406909f9887')}
{**u'y': 4**, u'_id': ObjectId('58234fc3dfd09406909f9888')}
{u'_id': ObjectId('58234fc7dfd09406909f9889'), u'z': 1}
{u'a': 5, u'_id': ObjectId('58234fccdfd09406909f988a')}
```
Notice the "$inc" in the second command.  This stands for incriment, and that whole key:value pair { "$inc" : { 'y' : 3 } } means **incriment** the value of the key **'y'** by **3**.

I won't go through every kind of update, but **$push** is an important one for lists...
```
>>> for d in coll.find(): print d

{u'x': [], u'_id': ObjectId('582355cadfd0940754ccce23'), u'id': 1}
{u'y': [], u'_id': ObjectId('582355d0dfd0940754ccce24'), u'id': 2}

>>> coll.update( {'id' : 1}, {"$push" : { 'x' : 'this' }})
>>> for d in coll.find(): print d

{u'x': [u'this'], u'_id': ObjectId('582355cadfd0940754ccce23'), u'id': 1}
{u'y': [], u'_id': ObjectId('582355d0dfd0940754ccce24'), u'id': 2}
```

**Purge documents** with coll.delete_many({})

### Where these points leave us
After going down through the layers, we're left with links to:

````
client ---> MongoClient
db     ---> Specific DatabaseNames
coll   ---> Specific Collection in db
docs   ---> List of elements
```

Meaning to switch databases, all you need to do is make another instance of a pointer (newdb = client.DatabaseName2) or reset your db pointer to another Database (db = client.DatabaseName2)
