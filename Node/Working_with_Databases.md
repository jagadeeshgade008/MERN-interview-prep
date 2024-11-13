
# Working with Databases in Node.js

## Connecting to MongoDB
Install MongoDB client library using `npm install mongodb mongoose`.

### Example: Basic CRUD with MongoDB
```javascript
const mongoose = require('mongoose');

mongoose.connect('mongodb://localhost:27017/myapp', {useNewUrlParser: true, useUnifiedTopology: true});

const UserSchema = new mongoose.Schema({
    name: String,
    age: Number
});

const User = mongoose.model('User', UserSchema);

// Create a new user
const newUser = new User({ name: 'Alice', age: 25 });
newUser.save().then(() => console.log('User Created'));
```
