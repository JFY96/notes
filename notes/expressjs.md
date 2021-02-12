Popular backend framework for Node.js

Express adds a bunch of utility functions and tools and a clear set of rules on how apps should be build (middleware). Express.js relies heavily on middleware functions.

## Middleware

Set of functions that excecute during the request-response lifecycle.

Request -> Middleware -> ... -> Middleware -> Response

Middleware functions have access to the request object, response object, and the next middleware function in the lifecycle. They can:

- Execute any code
- Make changes to the `req` and `res` objects
- End the request-response cycle
- Call the next middleware function in the stack, using `next()`

If not ending the cycle in the current middleware function, then it must call `next()` to pass control to next middleware function (or it will hang).

## Application-level middleware

Middleware bound to an instance of the `app` object (Express application).

```js
app.use() // executed for any HTTP request
app.get() // GET
app.post() // POST
app.put() // PUT
```

## Router-level middleware

Works like application-level middleware but bound to an instance of `express.Router()` instead.
This can be used to split routes across files in a nice way.

### Routing

Requests can be filtered by path (``'/'``) and method (POST, GET)

To use the router:
```js
app.use(router);
app.use('/admin', router); // filter route middleware paths to /admin
```
## Built-in middleware

``express.static`` to serve static assets such as css, images etc.

Example:
```js
app.use(express.static(path.join(__dirname, 'public'))); // Enable static assets from public folder
```

``express.json``

``express.urlencoded``

## Responses

``res.send()`` to send a typical response sending e.g. a string or html.

``res.sendFile()`` to send e.g. HTML files

``res.sendStatus()``



## Templating Engines

A template engine allows static template files to be used in an application.

At runtime, the template engine replaces variables in the template file with actual values,
then compiles it into HTML before sending to the client. 

Popular options for express are EJS or Pug.

For templating engines that are supported by express js by default, to load it into the app do:
```js
app.set('view engine', 'ejs'); // Load EJS
```
If that isn't the cass, then it will need to be imported then defined using ``app.engine()`` before doing the above.

It will then use the templating files (e.g. ``.ejs`` files if using EJS) in the ``/views`` directory by default (to change, set app ``views`` property).

Then create routes to render the ``/views`` files (using the ``res.render`` method to send responses) e.g.

```js
res.render('index'); // if view engine is set, do not need to specify the extension
```

```js
res.render('index', {title: '', message: msg}) // render dynamic content, pass variables to template
```

### EJS

Like HTML but <% tags to insert JS code / variables.

```
<% if (items.length > 0) { %>
    <% for (let item of items) { %>
        <h2><%= item.name %></h2>
    <% } %>    
<% } %>
```

- <%= value %>    output value is HTML escaped
- <%- value %>    output value is unescaped

Includes

```
<%- include('includes/header.ejs') %>

<%- include('includes/tabs.ejs', {tab: tab}) %>
```
