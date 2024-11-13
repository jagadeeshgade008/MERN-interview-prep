
# Express.js Framework

## Setting up an Express Server
Install Express using `npm install express`.

### Basic Express Server Example
```javascript
const express = require('express');
const app = express();
const PORT = 3000;

app.get('/', (req, res) => {
    res.send('Hello World!');
});

app.listen(PORT, () => {
    console.log(`Server is running on http://localhost:${PORT}`);
});
```

## Middleware and Routing
Middleware functions are functions that execute during the lifecycle of a request.

## Error Handling
Define error-handling middleware with four arguments `(err, req, res, next)`.
