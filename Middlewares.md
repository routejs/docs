## Middleware

A Middleware is a small piece of code that is used to alter web application `request/response` cycle, middleware function has access of request, response object and next middleware function in `request/response` cycle. 
Middlewares are the building block of routejs.

Middleware functions can be used to perform the following tasks:
- Execute code.
- Make changes to the `request/response` objects.
- End the `request/response` cycle.
- Call the next middleware function in `request/response` cycle.

If the current middleware function does not end the `request/response` cycle, it must call `next()` function to pass control to the next middleware function.

In routejs everything is middleware, type of middlewares are following:
- Global middleware
- Route middleware
- Error-handler middleware

Middleware defination:

```js
app.use(CALLBACKS);
```

Where:
- `CALLBACKS` is the callback function or router instance.

Example:

```js
app.use(function (req, res, next) {
  next();
});

app.use(
  function (req, res, next) {
    next();
  },
  function (req, res, next) {
    next();
  },
);
```

### Global Middleware

Global middleware is a function that called on every request.

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

Route middleware is a function that executed when request route is matched.

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

### Error-handler Middleware

Error-handler middleware is used to handle errors in `request/response` cycle.

Error-handler middleware always takes four arguments, where the first argument is the `error` object.
You must provide four arguments to identify a middleware as an error-handling middleware function.

```js
app.use(function (err, req, res, next) {
    res.writeHead(500).end("Internal Server Error");
});
```

Routejs catches error and automatically call the Error-handler middlewares.
If we pass error as an argument in `next()` function, it will skip all middlewares and only calls the next error-handler middleware.

```php
app.get("/", function (req, res) {
    next("error");
});

app.use(function (err, req, res, next) {
    res.writeHead(500).end("Internal Server Error");
});
```
