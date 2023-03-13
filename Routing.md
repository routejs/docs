## Routing

In a web application, routing is a process that determines how an application will responds to a particular request.

Routejs provide simple and robust apis for http routing, routejs support object and array based routing, named routing, grouped routing and host based url routing.

Route definition:

```js
router.METHOD("PATH", HANDLER);
```

Where:
- `router` is an instance of routejs router.
- `METHOD` is an HTTP request method, in lowercase.
- `PATH` is a request path or an endpoint.
- `HANDLER` is a function executed when the route path is matched.


Example:

```js
app.get("/", function (req, res) {
  res.end("GET");
});

app.post("/", function (req, res) {
  res.end("POST");
});
```

## Index
- [Http methods](#Http-methods)
- [Route parameters](#Route-parameters)
- [Named routes](#Named-routes)
- [Group routes](#Group-routes)
- [Host routes](#Host-routes)
- [Array based routing](#Array-based-routing)

## Http methods

Routejs support wide range of http request methods, all supported http request methods are the following:
- CHECKOUT
- COPY
- DELETE
- GET
- HEAD
- LOCK
- MERGE
- MKACTIVITY
- MKCOL
- MOVE
- NOTIFY
- OPTIONS
- PATCH
- POST
- PROPFIND
- PURGE
- PUT
- REPORT
- SEARCH
- SUBSCRIBE
- TRACE
- UNLOCK
- UNSUBSCRIBE
- VIEW

Example:

```js
// Handle get http request
app.get('/', function (req, res) {
  res.end("GET");
});

// Handle post http request
app.post("/", function (req, res) {
  res.end("POST");
});

// OR

app
  .get('/', function (req, res) {
    res.end("GET");
  })
  .post("/", function (req, res) {
    res.end("POST");
  });
```

Handle any of the given http request methods:

```js
// Handle get or post http request
app.any(["get", "post"], "/", function (req, res) {
  res.end("Ok");
});
```

Handle all http request methods:

```js
// Handle all http request
app.all("/", function (req, res) {
  res.end("Ok");
});
```

## Route parameters

Routejs supports route parameters, route parameters are used for dynamic routing.

In routejs we can define route parameters using `{}` syntax, we can put any name inside curly brackets and we can access them using `req.params` object.

Example:

```js
app.get("/user/{name}", function (req, res) {
  // Get route parameters using params object
  res.end(req.params.name);
});
```

It will match everything after `/user/` till `/`, for example:
- `/user/abc`
- `/user/xyz/`
- `/user/123`
- `/user/abc?id=10`

Routejs also support all valid regular expression in route parameters.

Regular expression:
```js
app.get("/user/(\\d+)", function (req, res) {
  res.end("Ok");
});
```

Make sure to escape all regular expression special characters, otherwise you might get wrong result.

Named regular expression:
```js
app.get("/user/{id:(\\d+)}", function (req, res) {
  res.end(req.params.id);
});
```

## Named routes

Routejs supports named routing, we can assign unique name to url routes, which can be used to generate url using name.

```js
// Set name to route
app.get("/", function (req, res) {
  res.end("Ok");
}).setName("home");

// Get path of named route
console.log(app.route("home"))
```

## Group routes

In routejs we can group related routes together.

Group defination:

```js
router.group("PATH", CALLBACK);
```

Where:
- `PATH` is a request path or a endpoint.
- `CALLBACK` is a function that accepts router instance as an argument.

Example:

```js
// Group related routes
app.group("/blog", function (router) {
  router.use(function (req, res, next) {
    // Group middleware
    next();
  });

  router.get("/read", function (req, res) {
    res.end("Ok");
  });

  router.post("/create", function (req, res) {
    res.end("Ok");
  });
});

// OR

const blog = new Router();
blog
  .use(function (req, res, next) {
    // Group middleware
    next();
  })
  .get("/read", function (req, res) {
    res.end("Ok");
  })
  .post("/create", function (req, res) {
    res.end("Ok");
  });

app.group("/blog", blog);
```

It will generate the following url routes:
- `/blog/read`
- `/blog/create`

## Host routes

Routejs supports host based url routing.

Route defination:

```js
router.domain("HOST", CALLBACK);
```

Where:
- `HOST` is a host name.
- `CALLBACK` is a function that accepts router instance as an argument.

In routejs by default all routes and middlewares are global, which means they are executed on every request.
If you are using host based url routing, please make sure to set the default host for all global routes and middlewares, so that they are not executed on every request.

Set default host to all routes and middlewares:
```js
const app = new Router({
  host: "localhost:3000"
});
```

If we set default request host to all routes and middlewares, then they are only executed when the request host and path both are matched.

Example:

```js
// Route is only executed when request host is localhost:3000
app.get("/", function (req, res) {
  res.end("Ok");
});

// Route is only executed when request host is blog.localhost:3000
app.domain("blog.localhost:3000", function (router) {
  router.get("/", function (req, res) {
    res.end("Blog");
  });
});
```

Host parameters:

Routejs supports named parameters and regular expression in host based url routing.

```js
app.domain("{name}.localhost:3000", function (router) {
  router.get("/", function (req, res) {
    res.end(req.subdomains.name);
  });
});
```

## Array based routing

Routejs is very simple and flexible, it support both object and array based url routing.

Let's create `urls.js` urls file for url routes:

```js
const { path, use } = require("@routejs/router");

// Url routes
const urls = [
  path("get", "/", (req, res) => res.end("GET")),
  path("post", "/", (req, res) => res.end("POST")),
  path(["get", "post"], "/blog", (req, res) => res.end("Blog")),
  // Create 404 page not found error
  use((req, res) => res.writeHead(404).end("404 Page Not Found")),
];

module.exports = urls;
```

Use urls in routejs app:

```javascript
const { Router } = require("@routejs/router");
const http = require("http");
const urls = require("./urls");

const app = new Router();

// Use url routes
app.use(urls);

http.createServer(app.handler()).listen(3000);
```

In array based routing `path()` function is used to set http routes. We can also use `all()`, `use()` and `domain()` functions.
