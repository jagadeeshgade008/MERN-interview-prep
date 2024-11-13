
# Node.js Interview Preparation

## 1. What is Node.js, and why is it used?
**Answer**: Node.js is an open-source, cross-platform JavaScript runtime built on Chrome’s V8 JavaScript engine. It allows developers to use JavaScript on the server side to build scalable network applications.

**Example**:
```javascript
const http = require('http');
const server = http.createServer((req, res) => {
  res.statusCode = 200;
  res.setHeader('Content-Type', 'text/plain');
  res.end('Hello World');
});
server.listen(3000, () => console.log('Server running on port 3000'));
```
This server can handle thousands of simultaneous connections with non-blocking I/O.

---

## 2. Explain the event-driven architecture of Node.js.
**Answer**: Node.js uses an event-driven architecture where the main server thread listens for events and delegates them to worker threads or callback functions, handling multiple requests asynchronously.

**Example**:
```javascript
const fs = require('fs');
console.log('Start');
fs.readFile('file.txt', 'utf8', (err, data) => {
  if (err) throw err;
  console.log('File content:', data);
});
console.log('End');
```
**Output**:
```
Start
End
File content: (contents of file.txt)
```
Here, file reading is asynchronous, meaning other code executes without waiting for it to finish.

---

## 3. What are Promises, and how do they help in asynchronous programming?
**Answer**: A Promise represents a future value that may be available now, later, or never. They simplify handling of asynchronous operations by replacing callback chains with a cleaner syntax.

**Example**:
```javascript
const fetchData = () => {
  return new Promise((resolve, reject) => {
    setTimeout(() => resolve('Data received'), 1000);
  });
};
fetchData().then(data => console.log(data)).catch(err => console.error(err));
```
This code asynchronously fetches data and handles it using `.then()` and `.catch()`.

---

## 4. How does the `async`/`await` syntax work in Node.js?
**Answer**: `async` functions return a Promise, and `await` pauses the execution of an async function until the Promise is resolved, making asynchronous code easier to read.

**Example**:
```javascript
const fetchData = () => new Promise(resolve => setTimeout(() => resolve('Data fetched'), 1000));

async function getData() {
  const data = await fetchData();
  console.log(data);
}

getData();
```
`await` waits for `fetchData()` to complete before logging the result.

---

## 5. Explain middleware in Express.js.
**Answer**: Middleware in Express.js are functions that have access to the `req` and `res` objects and can modify them or end the request-response cycle. Middleware are used for tasks like logging, authentication, and error handling.

**Example**:
```javascript
const express = require('express');
const app = express();

// Middleware function to log requests
app.use((req, res, next) => {
  console.log(`${req.method} request for '${req.url}'`);
  next(); // Pass control to the next middleware or route
});

app.get('/', (req, res) => res.send('Hello World!'));

app.listen(3000, () => console.log('Server running on port 3000'));
```
Here, every request logs its method and URL before reaching the route handler.

---

## 6. How can you handle errors in Node.js?
**Answer**: In Node.js, errors can be handled with try-catch blocks in synchronous code and error-handling middleware in Express for async code.

**Example** (Express error handler):
```javascript
app.get('/error', (req, res, next) => {
  const err = new Error('Something went wrong');
  next(err); // Pass error to the error-handling middleware
});

app.use((err, req, res, next) => {
  console.error(err.stack);
  res.status(500).send('Something went wrong!');
});
```
This middleware logs the error stack and returns a 500 response.

---

## 7. What is the role of `package.json` in Node.js?
**Answer**: `package.json` is a metadata file that contains information about the project, dependencies, scripts, and configuration. It helps manage the project with tools like NPM.

**Example**:
```json
{
  "name": "my-app",
  "version": "1.0.0",
  "scripts": {
    "start": "node app.js",
    "test": "mocha"
  },
  "dependencies": {
    "express": "^4.17.1"
  }
}
```
Run `npm install` to install dependencies or `npm start` to start the app based on this file.

---

## 8. How do you implement JWT authentication in Node.js?
**Answer**: JSON Web Tokens (JWT) allow secure transmission of data as JSON objects. To implement JWT authentication, generate a token upon login and verify it on subsequent requests.

**Example**:
```javascript
const jwt = require('jsonwebtoken');
const secretKey = 'my_secret_key';

// Generating a token
app.post('/login', (req, res) => {
  const user = { id: 1, username: 'user' };
  const token = jwt.sign(user, secretKey);
  res.json({ token });
});

// Verifying token
app.get('/protected', (req, res) => {
  const token = req.headers['authorization'];
  jwt.verify(token, secretKey, (err, user) => {
    if (err) return res.sendStatus(403);
    res.json({ message: 'Protected data', user });
  });
});
```
This example generates a JWT on login and verifies it before granting access to protected routes.

---

## 9. What is clustering in Node.js, and how does it work?
**Answer**: Clustering allows Node.js to create multiple processes that share the same server port, leveraging multi-core systems for better performance.

**Example**:
```javascript
const cluster = require('cluster');
const http = require('http');
const numCPUs = require('os').cpus().length;

if (cluster.isMaster) {
  for (let i = 0; i < numCPUs; i++) cluster.fork();
} else {
  http.createServer((req, res) => res.end('Hello World')).listen(8000);
}
```
This code creates a worker process for each CPU core to handle requests.

---

## 10. What are some security best practices in Node.js?
**Answer**: Security best practices include:
- **Validate and sanitize inputs** to prevent SQL injection or XSS attacks.
- **Use HTTPS** for secure data transmission.
- **Hide error details** in production environments.
- **Implement rate limiting** to prevent DDoS attacks.

**Example** (Sanitizing input with `express-validator`):
```javascript
const { body, validationResult } = require('express-validator');

app.post('/signup', body('email').isEmail().normalizeEmail(), (req, res) => {
  const errors = validationResult(req);
  if (!errors.isEmpty()) return res.status(400).json({ errors: errors.array() });
  // proceed with user creation
});
```
This code validates and sanitizes email input to ensure secure data handling.

---

## 11. What is the difference between `require` and `import` in Node.js?
**Answer**: `require` is used in CommonJS modules to import functionality, while `import` is used in ES6 modules.

**Example** (CommonJS):
```javascript
const express = require('express');
```

**Example** (ES6):
```javascript
import express from 'express';
```
```javascript

## 12. What is the Event Loop in Node.js?
**Answer**: The Event Loop is a mechanism that allows Node.js to handle asynchronous operations efficiently by processing events in a loop.

## 13. What is the difference between `process.nextTick` and `setTimeout`?
**Answer**: `process.nextTick` schedules a callback to be executed on the next iteration of the event loop, while `setTimeout` schedules a callback to be executed after a specified delay.

**Example**:
```javascript
console.log('Start');
process.nextTick(() => console.log('Next tick'));
setTimeout(() => console.log('Timeout'), 0);
console.log('End');
```
**Output**:
```
Start
Next tick
Timeout
End
```
This demonstrates that `process.nextTick` executes before `setTimeout`.

## 14. What is the purpose of the `events` module in Node.js?
**Answer**: The `events` module provides a way to handle events and event listeners, allowing for a more event-driven architecture.

**Example**:
```javascript
const EventEmitter = require('events');
const myEmitter = new EventEmitter();
```

## 15. What is the purpose of the `stream` module in Node.js?
**Answer**: The `stream` module provides a way to handle streaming data, allowing for efficient handling of large data sets without memory issues.

**Example**:
```javascript
const fs = require('fs');
const stream = fs.createReadStream('largeFile.txt');
```

## 16. What is the purpose of the `crypto` module in Node.js?
**Answer**: The `crypto` module provides cryptographic functionality that includes a set of wrappers for OpenSSL's hash, HMAC, cipher, decipher, sign, and verify functions.

**Example**:
```javascript
const crypto = require('crypto');
const hash = crypto.createHash('sha256');
```

## 17. Explain the concept of middleware in Express.js.
**Answer**: Middleware in Express.js are functions that have access to the `req` and `res` objects and can modify them or end the request-response cycle. Middleware are used for tasks like logging, authentication, and error handling.

**Example**:
```javascript
const express = require('express');
const app = express();

// Middleware function to log requests
app.use((req, res, next) => {
  console.log(`${req.method} request for '${req.url}'`);
  next(); // Pass control to the next middleware or route
});

app.get('/', (req, res) => res.send('Hello World!'));

app.listen(3000, () => console.log('Server running on port 3000'));
```

## 18. Explain sreams in Node.js
**Answer**: Streams are a way to handle data in Node.js efficiently by processing data in chunks rather than loading it all into memory at once.

**Example**:
```javascript
const fs = require('fs');
const stream = fs.createReadStream('largeFile.txt');
```

## 19. Explain different types of streams in Node.js
**Answer**: There are four types of streams in Node.js:
- **Readable**: Streams for reading data.
- **Writable**: Streams for writing data.
- **Duplex**: Streams that are both readable and writable.
- **Transform**: Streams that can modify or transform data as it is written and read.
 
<!-- Explain each type of stream with an example -->
- **Readable**: Streams for reading data.
```javascript
const fs = require('fs');
const readStream = fs.createReadStream('largeFile.txt');
```
- **Writable**: Streams for writing data.
```javascript
const fs = require('fs');
const writeStream = fs.createWriteStream('output.txt');
```
- **Duplex**: Streams that are both readable and writable.
```javascript
const { Duplex } = require('stream');
const duplexStream = new Duplex();
```
- **Transform**: Streams that can modify or transform data as it is written and read.
```javascript
const { Transform } = require('stream');
const transformStream = new Transform();
```

## 20. Explain the concept of Buffers in Node.js
**Answer**: Buffers are a way to handle binary data in Node.js efficiently by storing data in a fixed-size chunk of memory.

**Example**:
```javascript
const buffer = Buffer.from('Hello, world!');
console.log(buffer.toString());
```

## 21. Explain the concept of clustering in Node.js
**Answer**: Clustering in Node.js allows multiple instances of the same application to run on a single machine, distributing the load across multiple cores and improving performance.

**Example**:
```javascript
const cluster = require('cluster');
const http = require('http');
const numCPUs = require('os').cpus().length;

if (cluster.isMaster) {
  for (let i = 0; i < numCPUs; i++) cluster.fork();
} else {
  http.createServer((req, res) => res.end('Hello World')).listen(8000);
}
```

## 22. Explain the concept of child_process in Node.js
**Answer**: The `child_process` module allows you to create new processes and communicate with them using standard input/output streams.

**Example**:
```javascript
const { spawn } = require('child_process');
const child = spawn('ls', ['-lh', '/usr']);
```

## 23. Explain the concept of EventEmitter in Node.js
**Answer**: The `EventEmitter` class is used to implement event-driven architecture in Node.js, allowing you to create custom events and listen for them.

**Example**:
```javascript
const EventEmitter = require('events');
const myEmitter = new EventEmitter();
```

## 24. Explain the concept of DNS in Node.js
**Answer**: DNS (Domain Name System) is a hierarchical and decentralized naming system for computers, services, or any resource connected to the Internet or a private network.

**Example**:
```javascript
const dns = require('dns');
dns.lookup('example.com', (err, address, family) => {
  console.log('address: %j family: IPv%s', address, family);
});
```

## 25. Explain the logging in Node.js
**Answer**: Logging is an important aspect of debugging and monitoring Node.js applications. The `console` object provides methods for logging messages at different levels of severity.

**Example**:
```javascript
console.log('This is a log message');
console.error('This is an error message');

// winston is a popular logging library in Node.js
// example
const winston = require('winston');
const logger = winston.createLogger({
  level: 'info',
  format: winston.format.json(),
  transports: [new winston.transports.Console()],
});

logger.info('This is an info message');
logger.error('This is an error message');
logger.warn('This is a warning message');
logger.debug('This is a debug message');

```

## 26. Explain the concept of npm in Node.js
**Answer**: npm (Node Package Manager) is a package manager for Node.js that allows you to install, update, and manage dependencies.

**Example**:
```javascript
npm install express
```

## 27. Explain the concept of package.json in Node.js
**Answer**: The `package.json` file is used to manage dependencies, scripts, and other metadata for Node.js projects.

**Example**:
```json
{
  "name": "my-app",
  "version": "1.0.0",
  "dependencies": {
    "express": "^4.17.1"
  }
}
```

## 28. Explain the concept of npm scripts in Node.js
**Answer**: npm scripts are used to automate tasks in Node.js projects, such as running tests, starting a server, or building assets.

**Example**:
```json
"scripts": {
  "start": "node app.js",
  "test": "mocha"
}
```

## 29. Explain the concept of npm install in Node.js
**Answer**: The `npm install` command is used to install dependencies for a Node.js project.

**Example**:
```bash
npm install express
```

## 30. Explain the concept of npm publish in Node.js
**Answer**: The `npm publish` command is used to publish a Node.js package to the npm registry.

**Example**:
```bash
npm publish
```

## 31. Create a simple Node.js application and add CURD operations
**Answer**:
 ```javascript
    
    import express from 'express';
    import mongoose from 'mongoose';

    const app = express();

    app.use(express.json());

    app.get('/', (req, res) => res.send('Hello World'));

    app.post('/create', (req, res) => {
        const { name, age } = req.body;
        res.send(`User created with name: ${name} and age: ${age}`);
    });

    app.get('/read', (req, res) => {
        res.send('User read');
    });

    app.put('/update', (req, res) => {
        res.send('User updated');
    });

    app.delete('/delete', (req, res) => {
        res.send('User deleted');
    });

    app.listen(3000, () => console.log('Server running on port 3000'));

 ```

 ## 32. Create a simple Node.js application and add authentication
 **Answer**:
 ```javascript
  import express from 'express';
  import jwt from 'jsonwebtoken';

  const app = express();

  app.use(express.json());

  const secretKey = 'my_secret_key';

  app.post('/login', (req, res) => {
    const { username, password } = req.body;
    const user = { id: 1, username };
    const token = jwt.sign(user, secretKey);
    res.json({ token });
  });

  app.get('/protected', (req, res) => {
    const token = req.headers['authorization'];
    jwt.verify(token, secretKey, (err, user) => {
      if (err) return res.sendStatus(403);
      res.json({ message: 'Protected data', user });
    });
  });

  app.listen(3000, () => console.log('Server running on port 3000'));
 ```


















