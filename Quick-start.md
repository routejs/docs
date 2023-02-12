## Quick start

Routejs is the fast http routing engine for nodejs, so it's requires nodejs.
Download and install nodejs if you have not installed.

## Installation

Install using npm:

```console
$ npm i @routejs/router
```

Install using yarn:

```console
$ yarn add @routejs/router
```

## Example

```js
const { Router } = require("@routejs/router");
const http = require("http");

// Create routejs app
const app = new Router();

// Set routes
app.get("/", function (req, res) {
  res.end("Ok");
});

// Create 404 page not found error
app.use(function (req, res) {
  res.writeHead(404).end("404 Page Not Found");
});

// Create node http server
const server = http.createServer(app.handler())

// Start node http server
server.listen(3000);
```
