<!-- mongo queries with node.js -->

# MongoDB Queries

## 1. How to connect to MongoDB with Node.js?

```javascript
const { MongoClient } = require('mongodb');

const uri = 'mongodb://localhost:27017';
const client = new MongoClient(uri);

client.connect();
```

## 2. How to create a new MongoDB database?

```javascript
const db = client.db('mydatabase');
```

## 3. How to create a new MongoDB collection?

```javascript
const collection = db.collection('mycollection');
```

## 4. How to insert a new document into a MongoDB collection?

```javascript
const result = await collection.insertOne({ name: 'John', age: 30 });
console.log(result);
```

## 5. How to find a document in a MongoDB collection?

```javascript
const result = await collection.findOne({ name: 'John' });
console.log(result);
```

## 6. How to update a document in a MongoDB collection?

```javascript
const result = await collection.updateOne({ name: 'John' }, { $set: { age: 31 } });
console.log(result);
```

## 7. How to delete a document from a MongoDB collection?

```javascript
const result = await collection.deleteOne({ name: 'John' });
console.log(result);
```

## 8. How to close the connection to MongoDB?

```javascript
client.close();
```

## 9. How to use MongoDB with Express.js?

```javascript
const express = require('express');
const app = express();
```

## 10. How to use MongoDB with Express.js and Mongoose?

```javascript
const mongoose = require('mongoose');

mongoose.connect('mongodb://localhost:27017/mydatabase');
```

# MongoDB Queries with Mongoose

## 1. How to create a new Mongoose model?

```javascript
const mongoose = require('mongoose');

const userSchema = new mongoose.Schema({
  name: String,
  age: Number
});

const User = mongoose.model('User', userSchema);

// how to insert a new document into a Mongoose model?
const user = new User({ name: 'John', age: 30 });
await user.save();

```

## 2. How to find a document in a Mongoose model?

```javascript
const user = await User.findOne({ name: 'John' });
console.log(user);
```

## 3. How to update a document in a Mongoose model?

```javascript
const user = await User.updateOne({ name: 'John' }, { $set: { age: 31 } });
console.log(user);
```

## 4. How to delete a document from a Mongoose model?

```javascript
const user = await User.deleteOne({ name: 'John' });
console.log(user);
```

## 5. How to close the connection to MongoDB?

```javascript
mongoose.connection.close();
```

