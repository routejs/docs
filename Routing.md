## Routing

In a web application, routing is a process that determines how an application will responds to a particular request.

Route definition:

```js
router.METHOD('PATH', HANDLER);
```

Where:
- `router` is an instance of routejs router.
- `METHOD` is an HTTP request method, in lowercase.
- `PATH` is a request path or an endpoint.
- `HANDLER` is a function executed when the route path is matched.

