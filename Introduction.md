## Introduction

Routejs is a fast and lightweight http routing engine for [nodejs](http://nodejs.org).
Routejs is very simple and flexible, it supports both object and array based url routing, and provide simple and robust apis for http routing.

## Features

- Fast and lightweight
- Group routing
- Host based routing
- Named routing
- Middleware support
- Object and array based routing
- Regular expression support

## Example

```js
const { Router } = require("@routejs/router");
const http = require("http");

const app = new Router();

app.get("/", function (req, res) {
  res.end("Ok");
});

// Create 404 page not found error
app.use(function (req, res) {
  res.writeHead(404).end("404 Page Not Found");
});

http.createServer(app.handler()).listen(3000);
```

## Url route example

Routejs is very simple and flexible, it support both object and array based url routing.

Let's create `urls.js` urls file for routes:

```js
const { path, use } = require("@routejs/router");

// Url routes
const urls = [
  path("get", "/", (req, res) => res.end("Ok")),
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

## Philosophy

Routejs is a fast http routing engine for nodejs. Routejs highly depends on the middlewares, in routejs everything is a middleware, whether it's a http route, middleware or error-handler.

In routejs, middlewares are executed one-after-another from top to bottom, they are executed in the order that they are registered, so make sure to register routes and middlewares in the right order.

Global middlewares should be registered first before routes and error-handler middleware should be registered at the last after registering all the routes and middlewares.
