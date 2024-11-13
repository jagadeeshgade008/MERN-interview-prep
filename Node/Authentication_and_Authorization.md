
# Authentication and Authorization in Node.js

## JWT (JSON Web Token) Authentication
JWTs are a secure way to transfer data between parties.

### Example: JWT Authentication
```javascript
const express = require('express');
const jwt = require('jsonwebtoken');

const app = express();

app.get('/login', (req, res) => {
    const user = { id: 1, username: 'Alice' };
    const token = jwt.sign({ user }, 'secret_key');
    res.json({ token });
});
```

## Verifying JWT
```javascript
app.post('/protected', (req, res) => {
    const token = req.headers['authorization'];
    jwt.verify(token, 'secret_key', (err, authData) => {
        if(err) res.sendStatus(403);
        else res.json({ message: 'Access granted' });
    });
});
```
