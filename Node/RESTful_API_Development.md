
# RESTful API Development

## REST API Basics
REST APIs follow standard HTTP methods for CRUD operations.

### Example: Basic CRUD API with Express and MongoDB
```javascript
const express = require('express');
const mongoose = require('mongoose');
const app = express();

mongoose.connect('mongodb://localhost:27017/myapp', { useNewUrlParser: true, useUnifiedTopology: true });

const UserSchema = new mongoose.Schema({ name: String, age: Number });
const User = mongoose.model('User', UserSchema);

app.use(express.json());

// Create user
app.post('/users', async (req, res) => {
    const user = new User(req.body);
    await user.save();
    res.send(user);
});

app.listen(3000, () => console.log('Server running on port 3000'));
```
