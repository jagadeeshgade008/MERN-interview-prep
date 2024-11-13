
# Error Handling and Debugging

## Error Handling
Centralize error handling in Express using middleware.

### Example: Error Handling Middleware
```javascript
app.use((err, req, res, next) => {
    console.error(err.stack);
    res.status(500).send('Something went wrong!');
});
```

## Debugging with Node.js
Use the `--inspect` flag and Chrome DevTools to debug.
