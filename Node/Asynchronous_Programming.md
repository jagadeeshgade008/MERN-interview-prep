
# Asynchronous Programming in Node.js

## Callbacks, Promises, and Async/Await
Node.js uses asynchronous programming to handle I/O operations.

### Example: Using Promises
```javascript
const fetchData = () => {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            resolve('Data fetched');
        }, 1000);
    });
};

fetchData().then(data => console.log(data));
```

### Example: Async/Await
```javascript
async function getData() {
    const data = await fetchData();
    console.log(data);
}

getData();
```
