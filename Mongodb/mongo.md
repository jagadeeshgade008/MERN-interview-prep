# MongoDB Interview Questions
 
## Basic Questions
 
### 1. What is MongoDB, and how is it different from a relational database?
 
MongoDB is a NoSQL document-oriented database that stores data in a flexible, JSON-like format called BSON. Unlike relational databases, MongoDB does not use tables or support complex joins. Instead, data is stored in collections of documents.
 
**Key Differences:**
 
- **Schema**: MongoDB is schema-less, allowing each document to have different structures, while relational databases require predefined schemas.
- **Scalability**: MongoDB supports horizontal scaling, making it easier to handle large data volumes.
- **Data Storage**: MongoDB uses BSON (Binary JSON), allowing more data types and faster data access.
 
### 2. Explain how indexing works in MongoDB. Why are indexes important?
 
Indexes in MongoDB are similar to indexes in relational databases. They store a small portion of the data in an ordered form, which significantly improves the speed of data retrieval.
 
**Example**:
```javascript
db.collection.createIndex({ "field_name": 1 });
```
 
Here, `1` indicates an ascending order. Using indexes reduces the time MongoDB takes to fetch documents.
 
### 3. How do you insert a document in MongoDB? Provide an example.
 
You can insert a document using the `insertOne` or `insertMany` methods in MongoDB.
 
**Example**:
```javascript
db.collection.insertOne({
  name: "Alice",
  age: 30,
  occupation: "Engineer"
});
```
 
The document above adds a single entry to the specified collection.
 
### 4. How do you query for documents in MongoDB?
 
MongoDB provides various query methods like `find`, `findOne`, and supports filter conditions using JSON-like syntax.
 
**Example**:
```javascript
db.collection.find({ "age": { $gte: 25 } });
```
 
The query above retrieves all documents where the age field is greater than or equal to 25.
 
### 5. How do you update documents in MongoDB? Provide examples using `updateOne` and `updateMany`.
 
In MongoDB, you can update documents with `updateOne`, `updateMany`, or `replaceOne`.
 
**Example using `updateOne`**:
```javascript
db.collection.updateOne(
  { name: "Alice" },
  { $set: { age: 31 } }
);
```
 
This updates Alice’s age to 31 in a single document.
 
**Example using `updateMany`**:
```javascript
db.collection.updateMany(
  { occupation: "Engineer" },
  { $set: { department: "Engineering" } }
);
```
 
This updates the department field for all documents with the occupation "Engineer".
 
### 6. Explain MongoDB’s aggregation framework. Give an example.
 
MongoDB's aggregation framework allows for data transformation and computation on documents in a collection.
 
**Example**:
```javascript
db.collection.aggregate([
  { $match: { status: "active" } },
  { $group: { _id: "$department", count: { $sum: 1 } } }
]);
```
 
This example filters documents with `status: "active"` and groups them by the `department` field, counting documents in each department.
 
### 7. What is a replica set in MongoDB?
 
A replica set in MongoDB is a group of mongod processes that maintain the same data set, providing high availability and redundancy.
 
**Key Components**:
- **Primary**: Receives all write operations.
- **Secondaries**: Replicate data from the primary and can serve as a failover.
 
### 8. How does sharding work in MongoDB?
 
Sharding allows MongoDB to distribute data across multiple servers, enabling horizontal scaling. MongoDB divides data into smaller chunks and distributes them across shards (nodes).
 
### 9. Explain the difference between `find()` and `aggregate()`.
 
- **find()**: A basic query method, used for retrieving documents based on criteria.
- **aggregate()**: Allows for more complex data processing, such as grouping, sorting, and transformations.
 
---
 
### 10. How do you perform a text search in MongoDB? Provide an example.
 
To perform a text search, you must first create a text index.
 
**Example**:
```javascript
db.collection.createIndex({ description: "text" });
db.collection.find({ $text: { $search: "keyword" } });
```
 
This finds documents containing the word "keyword" in the `description` field.
 
---
 
## Advanced Questions
 
### 11. Explain MongoDB Transactions. How do they work?
 
MongoDB transactions allow you to execute multiple operations within a single ACID transaction across a replica set. Transactions are useful for applications that require atomic operations.
 
**Example**:
```javascript
const session = db.getMongo().startSession();
session.startTransaction();
try {
  db.collection1.insertOne({ field: "value1" }, { session });
  db.collection2.updateOne({ field: "value2" }, { $set: { field: "new_value" } }, { session });
  session.commitTransaction();
} catch (error) {
  session.abortTransaction();
}
session.endSession();
```
 
### 12. How would you perform a `$lookup` operation?
 
The `$lookup` stage is used for performing joins between collections, similar to SQL joins.
 
**Example**:
```javascript
db.orders.aggregate([
  {
    $lookup: {
      from: "customers",
      localField: "customerId",
      foreignField: "_id",
      as: "customerDetails"
    }
  }
]);
```
 
This aggregates data from the `customers` collection with each document in `orders` based on `customerId`.
 
### 13. How does MongoDB handle concurrency?
 
MongoDB uses multi-granularity locking, which includes locks at the database, collection, and document levels. It also employs an optimistic concurrency model using timestamps in a replica set.
 
### 14. What are Change Streams in MongoDB?
 
Change Streams allow applications to listen to real-time changes in a collection. It is commonly used for real-time applications.
 
**Example**:
```javascript
const changeStream = db.collection.watch();
changeStream.on("change", (change) => {
  console.log("Change detected:", change);
});
```
 
### 15. Describe MongoDB’s Write Concern and Read Concern.
 
- **Write Concern**: Specifies the level of acknowledgment requested from MongoDB for write operations.
- **Read Concern**: Defines the consistency and isolation properties for read operations, like `local`, `available`, and `majority`.