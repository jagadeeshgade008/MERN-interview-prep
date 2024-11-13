
# Testing in Node.js

## Testing Libraries: Mocha, Chai, Jest
Install with `npm install mocha chai jest`.

### Example: Basic Mocha Test
```javascript
const assert = require('chai').assert;
const add = (a, b) => a + b;

describe('Addition', function() {
    it('should return 4', function() {
        assert.equal(add(2, 2), 4);
    });
});
```

Run tests using `npm test`.
