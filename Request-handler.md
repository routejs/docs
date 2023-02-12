## Request handler

Request handler is used to handle incoming requests and execute matched routes.

Routejs provide flexible request handler, you can get ready to use request handler or you can setup your own custom request handler.

Routejs `app.handler()` function return a request handler that accepts `req` and `res` as an argument and can be directly used in the node http server.

Example:

```js
// Get request handler
const handler = app.handler();

const server = http.createServer(handler);
server.listen(3000);
```

In routejs you can manually handle the incoming http requests.
Routejs `app.handle()` function is used to handle the http request.

Defination:

```js
app.handle({
  host,
  method,
  url,
  request,
  response
});
```

Where:
- `host` is the http host.
- `method` is the http request method.
- `url` is the http request url.
- `request` is the http request object that is passed to all middlewares.
- `response` is the http response object the is passed to all middlewares.

Example:

```js
http.createServer(function (req, res) {
  // Handle the http requests
  app.handle({
    host: req.host,
    method: req.method,
    url: req.url,
    request: req,
    response: res
  });
});
```
