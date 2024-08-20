**What is BSON in MongoDB.?**

- Mongo DB store the data in the form BSON . BSON means binary JSON.
- Json only store text , number datatypes , But BSON can store , binary data type.
- apart that it can also id's , geo spatial data ,  boolean , regex.
- Due to BSON , efficient storage space and scan speed.


**Define Structure of ObjectID.?**

- Its contains total of 12 bytes 
    - Timestamp 
    - Machine ID 
    - Process ID 
    - 3 bytes increment counter
- Its effective for indexing


**What is a profiler's role in MongoDB?**

- It display info about each database activity 
- you can use this profiler to discover queries and write operation taking long time.


**What is the primary key in MongoDB?**

- primary key serves as unique indentifier in a collections
- object id is created by default for every document inside collections.
- By default index will be created on this primary key 


**Different type of indexes?**

- single field index  : ﻿`﻿db.mycollection.createIndex({fieldName : 1});` 
- compound index : `﻿db.mycollection.createIndex({fieldName : 1 , fieldAge : -1});` 
- text index (used for text-based search) : `﻿db.mycollection.createIndex({fieldName : "text"});` 
- geo-spacial index : `﻿db.mycollection.createIndex({fieldName : "2dsphere"});` 
- timetolive index : `﻿db.mycollection.createIndex({fieldName : 1} {`expireAfterSecond :3600`});` 
- multikey index : 
    - Its an index applied on  multiple values in an array field.


What is the covered query?

- when query doesn't need to scan the document , instead rely entirely on index is called covered queries
- here the all the fields were indexed.
### How Does MongoDB Ensure High Availability and Scalability?
- Availability can be achieved using  replica sets
- Scalability can be achived using sharding.


### Explain the Concept of Replica Sets in MongoDB.
- The replica set contains two type of node  one primary and  multiple secondary node
- primary node receives all write operations and secondary gets all read operation
- ops logs will collects all the write info from primary node
- the secondary node will sync with the primary node using ops logs time to time basis


### What are the Advantages of Using MongoDB Over Other Databases?
- Flexibility: no need for fixed schemas, dynamic schemas and unstructured 
- scalability: Its uses horizontal scaling called sharding for scalability.
- High Availablity: It uses replica sets for High Availablity
- performance: Due to the usage of BSON format,it has efficient data storage and accessing 


###  What is Sharding, and How Does It Work in MongoDB?
- Distributing the data across multiple servers
- It allows horizonal scaling , mainly used when dealing with large data sets
- Its splits large data in to chunks and stored accross multiple machines
- Its uses concept config server where it stores all the shards info.


### How Does MongoDB Handle Data Consistency?
- Whenever the MongoDB trying to do write operations. its written in primary node and jouranal log. So if the primary crashes while writing , It will journal log  to reinstate the state which it used to be.
- Write concerns :  acknowledgment from primary only, or acknowledgment from primary and secondaries
- Replica sets  : To maintain data consistency , it uses Ops log to get up to date with primary node


###  What is the Role of Journaling in MongoDB, and How Does It Impact Performance?
- It ensure data durability and crash recovery 
- It will record all the changes to data even before writing to the database
- These logs will used  to recover , when crashes happens


**What is the role of Ops log ?**

- Its a capped collections for monitoring prmiary node
- Its used  during replication mechanism
- Secondary nodes will use this ops log for getting insync with primary
###  Explain the Concept of Write Concern and Its Importance in MongoDB.
- It refers the level of acknowledgment requested from MongoDB for write operations.
    - **Unacknowledged:** MongoDB doesn't wait for acknowledgment.
    - **Acknowledged:** MongoDB waits for acknowledgment from the primary node.
    - **Majority:** MongoDB waits for acknowledgment from the majority of replica set members, ensuring data durability.
- Write concern depend on data durability and performance
- If level is high, we should expect the latency.
```
db.collection.insert(
   { item: "product", qty: 100 },
   { writeConcern: { w: "majority", j: true, wtimeout: 5000 } }
)
```
###  How do you perform data import and export in MongoDB?
```
mongoimport --db mydatabase --collection mycollection --file data.json

mongoexport --db mydatabase --collection mycollection --out data.json
```
### What are MongoDB Aggregation Pipelines and How are They Used?
- It uses a big data paradigm called mapper and reducer.
- It will transform the results and aggregate them to give collected output. It is used for complex queries.
- data will go through multiple stages , where each stage operate such as filter , sort ,match, group , project , unwind, lookup .. etc


### What are TTL Indexes, and How are They Used in MongoDB?
- These indexes only sustain until the mentioned period
- `﻿db.mycollection.createIndex({"field":1} , {`expireAfterSecond : 3600`})` 


### Explain the Concept of Geospatial Indexes in MongoDB?
- These are special indexes  that supports querying on geo spacial data (locations & coordinates)
- They enable efficient queries for proximity, intersections, and other spatial relationships.
- `﻿db.places.createIndex({ location: "2dsphere" });` 
 



### How do you handle schema design and data modeling in MongoDB?
- unlike the RelationDB , there is no need of normalization , relationship finding , keys and all
- We have two type of models here 
    - Embedded model: All the schema will be in one document, and nested document if possible.
    - Normalized Model : It uses references instead of nested data.
- We need to decide which model suits as per the requirement
- create indexes for efficient query performance
- design schema which support horizontal scaling and replication


### Explain the Differences Between WiredTiger and MMAPv1 Storage Engines
- WiredTiger : 
    - Its a new storage engine for MongoDB 
    - It enable **document level  concurrency**  , its allows multiple operations to be performed .
    - uses write a head log for data consistency
    - It supports compression
- MMAPv1:
    - It an old storage engine  for MongoDB
    - It support **collection level concurency  , ** limits perfomance under heavy writes
    - No compression


### How to Handle Transactions in MongoDB?
- To achieve CURD operations in MongoDB  we need creation a session and do whatever the operation we need and close the session . 
- If any operation fails with in the session  , entire transaction is aborted.
```
const session = client.startSession();

session.startTransaction();

try {
  db.collection1.insertOne({ name: "Alice" }, { session });
  db.collection2.insertOne({ name: "Bob" }, { session });
  session.commitTransaction();
} catch (error) {
  session.abortTransaction();
} finally {
  session.endSession();
}
```


### What are Capped Collections, and When are They Useful?
- These are fixed size collections , which will automatically override the old document after specific period.
- They maintain insertion order first in first out
- mainly used for cache , logging , monitoring.`﻿` 
- `db.createCollection("logs", { capped: true, size: 100000 });` 


### How to Handle Backups and Disaster Recovery in MongoDB?
- Using of Mongodump and Mongorestore  for regular  binary backups.
- taking of file system snapshots for consistent data file backups
- If we are Mongo Atlas  , we can levarge the cloud backups.
- maintaining replica sets for  high availability


### What are Change Streams in MongoDB, and How are They Used?
- It allows real time changes to data on collections.
- They provide a powerful way to implement event-driven architectures by capturing insert, update, replace, and delete operations.
- To use change stream , you need to use cursor.
```
const changeStream = db.collection('orders').watch();
changeStream.on('change', (change) => {
  console.log(change);
});
```


### How to Optimize MongoDB Queries for Performance?
- creating indexes
- using projections
- **Index Hinting: **Use index hints to force the query optimizer to use a specific index.
- query analysis using explain method
- aggregation pipeline for complex queries


### How to Implement Full-Text Search in MongoDB?
- first create an index on field 
- `﻿db.mycollection.createIndex({'fieldname'` : 'text' })
- `﻿db.mycollection.find({$text : {$search : "text name"}})` 


**What is the difference between find and findOne.?**

- findOne return the first value it macthes in the collections
- find will also return the first values it macthes  , but it also retrive more the one document 
- find method will return a cursor .cursor  give the result in batch during iteration.
- we can use limit , skip  , sort , toArray()  like methods on find method.


**what is a cursor in MongoDB, how it is used.?**

- cursor is a power which iterates over result of array
- Instead of returning the entire data, it returns one doc at a time or in batches.
- It is particularly useful when handling large datasets, that might be too memory intensive.


**How to perform case insensitive search in MongoDB.?**

`﻿db.mycollection.find({fieldName : { $regex : "search term " , $options : "i"}})` 



**How do you perform regular expression search ?**

`﻿db.mycollection.find("fieldName : {$regex : /[0-9][a-z]/}});` 



**Difference between addToSet and push operator**? 

- Both of these add value to an array field in the document . but addtoSet maintain unique values.


**what is the use of lookup operator ?**

- It is used to join multiple collections in MongoDB.
- Its aggregation pipeline stage where it allows left outer join on collections
```
-> users
[
    { "_id": 1, "name": "Alice" },
    { "_id": 2, "name": "Bob" }
]

-> orders
[
    { "_id": 1, "user_id": 1, "total_amount": 50 },
    { "_id": 2, "user_id": 2, "total_amount": 30 },
    { "_id": 3, "user_id": 1, "total_amount": 20 }
]


db.orders.aggregate([
    {
        $lookup: {
            from: "users",
            localField: "user_id",
            foreignField: "_id",
            as: "user"
        }
    },
    {
        $unwind: "$user"
    }
])

//output
[
    {
        "_id": 1,
        "user_id": 1,
        "total_amount": 50,
        "user": { "_id": 1, "name": "Alice" }
    },
    {
        "_id": 2,
        "user_id": 2,
        "total_amount": 30,
        "user": { "_id": 2, "name": "Bob" }
    },
    {
        "_id": 3,
        "user_id": 1,
        "total_amount": 20,
        "user": { "_id": 1, "name": "Alice" }
    }
]
```


**How to handle duplicates in MongoDB?**

- MongoDB does enforce unique constraints on the collections by default
- we can add unique constraints if requried
- But if we need to handle dulplicate data already in collection , we can use deleteMany();


**What is the use of explain method in Mongo DB ?**

- It tells about how is query is executed
- it gives insights about query execution plan , indexes usages , number of documeted scanned
- based on the ouput we can optimize our query better.




