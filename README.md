## Routejs

<p align="center">
  <img src="https://github.com/routejs/docs/blob/main/routejs.jpg" width="400px" alt="routejs logo">
</p>

Routejs is a fast, simple and lightweight http routing library for nodejs. Routejs provide simple and elegant apis for http routing.

## Features
- Fast and flexible routing
- Simple and minimal api
- Named routing
- Grouped route
- Host routing

## Installation
```shell
npm i @routejs/router
```

OR

```shell
yarn add @routejs/router
```

## Simple example

```javascript
const Router = require("@routejs/router");
const http = require("http");

const app = new Router();

app.get("/", function (req, res) {
  res.end("Ok");
});

const server = http.createServer(app.handler());
server.listen(3000);
```

## Url routes example
Routejs is very simple and flexible, it support both object and array based url routing.

Let's create urls file for routes:
```javascript
const { get, post, put, delete } = require("@routejs/router");
const { getBlog, createBlog, updateBlog, deleteBlog } = require("./blogs");

const urls = [
  get("/blog", getBlog),
  post("/blog", createBlog),
  put("/blog", updateBlog),
  delete("/blog", deleteBlog),
];
```

Use urls in routejs:
```javascript
const http = require("http");
const Router = require("@routejs/router");
const urls = require("./urls");

const app = new Router();

app.useRoutes(urls);

const server = http.createServer(app.handler());
server.listen(3000);
```
## License

  [MIT License](https://github.com/routejs/router/blob/main/LICENSE)
