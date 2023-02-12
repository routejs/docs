## Middleware

A Middleware is a small piece of code that is used to alter web application `request/response` cycle, middleware function has access of request, response object and next middleware function in `request/response` cycle. 
Middlewares are the building block of routejs.

Middleware functions can be used to perform the following tasks:
- Execute code.
- Make changes to the `request/response` objects.
- End the `request/response` cycle.
- Call the next middleware function in `request/response` cycle.

If the current middleware function does not end the `request/response` cycle, it must call `next()` function to pass control to the next middleware function.

Types of middlewares in unic framework:
- Global middleware
- Route middleware
- Error-handling middleware

### Global Middleware

Let's create a global middleware.

```js
app.use(function (req, res, next) {
    req.counter = 0;
    // Call next middleware
    next();
});

app.use(function (req, res, next) {
    req.counter++;
    next();
});
```

### Route Middleware

Let's create a route middleware.

```js
app.get("/",
  function (req, res, next) {
    req.counter = 0;
    // Calls next middleware
    next();
  },
  function (req, res) {
    res.end(`Counter is ${req.counter}`);
  }
);
```

### Error-handling Middleware

Error-handling middleware is used to handle errors, error-handling middleware always takes four arguments.
You must provide four arguments to identify a middleware as an error-handling middleware function.

Let's create an error-handling middleware.

```js
app.use(function (err, req, res, next) {
    res.writeHead(500).end("Internal Server Error");
});
```

Routejs catches error and automatically call the error-handling middlewares, but you can manually call error-handling middleware by passing error as an argument in `next()` function.
If we pass error as an argument in `next()` function, it will skip all middlewares and only calls the error-handling middleware.

```php
app.get("/", function (req, res) {
    next("error");
});

app.use(function (err, req, res, next) {
    res.writeHead(500).end("Internal Server Error");
});
```
