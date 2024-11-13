
# Introduction to Node.js

## What is Node.js?
Node.js is an open-source, cross-platform, JavaScript runtime environment that executes JavaScript code outside of a web browser. It is built on Chrome's V8 JavaScript engine.

## Key Features
- **Non-blocking I/O**: Event-driven architecture enables handling multiple requests efficiently.
- **Single-threaded**: Although single-threaded, Node.js can manage concurrent connections efficiently.
- **Cross-platform**: Runs on Windows, Linux, Unix, macOS.

## Example: Basic Node.js Server
```javascript
const http = require('http');

const server = http.createServer((req, res) => {
  res.statusCode = 200;
  res.setHeader('Content-Type', 'text/plain');
  res.end('Hello, World!');
});

server.listen(3000, () => {
  console.log('Server running at http://localhost:3000/');
});
```
Run this script with `node filename.js` to start the server.
