# Play with MongoDB

## Create Database

### The use Command

MongoDB **use DATABASE_NAME** is used to create database. The command will create a new database if it doesn't exist, otherwise it will return the existing database.

Eg: create a database with name **```<mydb>```**,

    use mydb
    switch to db mydb

### Check currently selected database

    db
    mydb

### Check database list

    show dbs

    local 0.00000GB
    test  0.00000GB

### Insert document into database

You need to insert at least one document into it.

    db.movie.insert({"name":"tutorials point"})
    show dbs
    local 0.00000GB
    mydb  0.00000GB
    test  0.00000GB

The default database in MongoDB is test.

## Drop Database

### The dropDatabase() Method

MongoDB **db.dropDatabase()** command is used to drop a existing database.

    $ ./mongo.exe
    MongoDB shell version v3.4.1
    connecting to: mongodb://127.0.0.1:27017
    MongoDB server version: 3.4.1
    show dbs
    admin  0.000GB
    local  0.000GB
    mydb   0.000GB
    test   0.000GB
    use mydb
    switched to db mydb
    db.dropDatabase()
    { "dropped" : "mydb", "ok" : 1 }
    show dbs
    admin  0.000GB
    local  0.000GB
    test   0.000GB


## Create Collection

MongoDB **db.createCollection(name, option)** command is used to create collection.

In the command, **name** is name of collection to be created, **option** is a document and is used to specify configuration of collecion.

|**Parameter**|**Type**|**Description**|
|:-----------:|:------:|:-------------:|
| Name | String | Name of the collecion to be created|
|Options| Document | (Optional) Specify options about memory size and indexing|

Options parameter is optional, so you need to specify only the name of the collection. Following is the list of options you can use

|**Field**|**Type**|**Description**|
|:-----------:|:------:|:-------------:|
| capped | Boolean | (Optional) If true, enalbes a capped collection. capped colleciton is a fixed size collection that automatically overwrites its oldest entries when it reaches its maximum size. **If you specify true, you need to specify size parameter also.**|
| autoIndexID | Boolean | (Optional) If true, automatically create index on _id field.s Default value is false.|
| size | number | (Optional) Specifies a maximum size in bytes for a capped collection, **If capped is true, then you need to specify this field also**|
| max | number | (Optional) Specifies the maximum number of documents allowed in the capped collection.|

While inserting the document, MongoDB firest checks size field of capped collection, then it checks max field.

Eg:

    switched to db test
    db.createCollection("mycollection")

>Note: sometimes there is no return after createCollection and the process stukes.

## Drop collection

MongoDB's **db.collection.drop()** is used to drop a collection from the database.

Eg:

    show collections
    mycollection
    test
    db.mycollection.drop()
    true
    show collections
    test

## Data Types

MongoDB supports many datatypes. 

- **String** - This is the most commonly used datatype to store the data. String in MongoDB must be UTF-8 valid.
- **Integer** − This type is used to store a numerical value. Integer can be 32 bit or 64 bit depending upon your server.

- **Boolean** − This type is used to store a boolean (true/ false) value.

- **Double** − This type is used to store floating point values.

- **Min/ Max keys** − This type is used to compare a value against the lowest and highest BSON elements.

- **Arrays** − This type is used to store arrays or list or multiple values into one key.

- **Timestamp** − ctimestamp. This can be handy for recording when a document has been modified or added.

- **Object** − This datatype is used for embedded documents.

- **Null** − This type is used to store a Null value.

- **Symbol** − This datatype is used identically to a string; however, it's generally reserved for languages that use a specific symbol type.

- **Date** − This datatype is used to store the current date or time in UNIX time format. You can specify your own date time by creating object of Date and passing day, month, year into it.

- **Object ID** − This datatype is used to store the document’s ID.

- **Binary data** − This datatype is used to store binary data.

- **Code** − This datatype is used to store JavaScript code into the document.

- **Regular expression** − This datatype is used to store regular expression.

## Insert Document

To insert data into MongoDB collection, you need to use MongoDB's **insert()** or **save()** command is as follows -

    db.COLLECTION_NAME.insert(document);

Eg:

    db.users.find()
    { "_id" : ObjectId("58863760f5c760b0324a6778"), "username" : "liaojianguo" }
    db.users.insert({"username":"pingping","group":"singer"})
    WriteResult({ "nInserted" : 1 })
    db.users.find()
    { "_id" : ObjectId("58863760f5c760b0324a6778"), "username" : "liaojianguo" }
    { "_id" : ObjectId("58863960386d5d3a2cc623ca"), "username" : "pingping", "group" : "singer" }

> Note: It seems ; is mandatory as an end

Here mycol is our collection name, as created in the previous chapter. If the collection doesn't exist in the database, then MongoDB will create this collection and then insert a document into it.

In the inserted document, if we don't specify the _id parameter, then MongoDB assigns a unique ObjectId for this document.

_id is 12 bytes hexadecimal number unique for every document in a collection. 12 bytes are divided as follows −

    _id: ObjectId(4 bytes timestamp, 3 bytes machine id, 2 bytes process id, 
    3 bytes incrementer)

To insert multiple documents in a single query, you can pass an array of documents in insert() command.   

    db.post.insert([
       {
          title: 'MongoDB Overview', 
          description: 'MongoDB is no sql database',
          by: 'tutorials point',
          url: 'http://www.tutorialspoint.com',
          tags: ['mongodb', 'database', 'NoSQL'],
          likes: 100
       },
        
       {
          title: 'NoSQL Database', 
          description: 'NoSQL database doesn't have tables',
          by: 'tutorials point',
          url: 'http://www.tutorialspoint.com',
          tags: ['mongodb', 'database', 'NoSQL'],
          likes: 20, 
          comments: [   
             {
                user:'user1',
                message: 'My first comment',
                dateCreated: new Date(2013,11,10,2,35),
                like: 0 
             }
          ]
       }
    ])

To insert the document you can use db.post.save(document) also. If you don't specify _id in the document then save() method will work same as insert() method. If you specify _id then it will replace whole data of document containing _id as specified in save() method.

## Query Document

    db.COLLECTION_NAME.find()
    db.COLLECTION_NAME.find().pretty() // show results in a formatted way

### RDBMS Where Clause Equivalents in MongoDB

|Operation|Syntax|Example|RDBMS Equivalent|
| ------- | ---- | ----- | -------------- |
|Equality |```{<key>:<value>}``` |db.mycol.find(```{"by":"tutorials point"}```).pretty()|where by = 'tutorials point'|
|Less Than|```{<key>:{$lt:<value>}}```|db.mycol.find(```{"likes":{$lt:50}}```).pretty()|where likes < 50|
|Less Than Equals|```{<key>:{$lte:<value>}}```  db.mycol.find(```{"likes":{$lte:50}}```).pretty()|where likes <= 50|
|Greater Than |```{<key>:{$gt:<value>}}```| db.mycol.find(```{"likes":{$gt:50}}```).pretty()|where likes > 50|
|Greater Than Equals|```{<key>:{$gte:<value>}}```  |db.mycol.find(```{"likes":{$gte:50}}```).pretty()|where likes >= 50|
|Not Equals|```{<key>:{$ne:<value>}}```| db.mycol.find(```{"likes":{$ne:50}}```).pretty()|where likes != 50|



### AND in MongoDB

    db.mycol.find(
    {
      $and: [
         {key1: value1}, {key2:value2}
      ]
    }
    ).pretty()
### OR in MongoDB

    db.mycol.find(
    {
      $or: [
         {key1: value1}, {key2:value2}
      ]
     }
    ).pretty()

### Using AND and OR Together

    db.mycol.find({"likes": {$gt:10}, $or: [{"by": "tutorials point"},
    {"title": "MongoDB Overview"}]}).pretty()