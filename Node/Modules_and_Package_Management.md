
# Node.js Modules and Package Management

## Modules in Node.js
Node.js has a modular architecture, and each JavaScript file can be a module.

### CommonJS vs ES6 Modules
- **CommonJS**: Uses `require` and `module.exports`.
- **ES6**: Uses `import` and `export`.

## NPM (Node Package Manager)
NPM is used to manage project dependencies. Initialize a project with `npm init`.

## Example: Custom Module
1. Create a module `greet.js`:
   ```javascript
   module.exports = function(name) {
       return `Hello, ${name}!`;
   };
   ```

2. Import and use the module:
   ```javascript
   const greet = require('./greet');
   console.log(greet('Alice'));
   ```
