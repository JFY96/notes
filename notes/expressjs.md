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
## Route Parameters

```js
app.use("/users/:userId", (req, res) => {
    // if request url is users/1234
    // req.params = { "userId": "1234" }
});

// Can use - and . along with route paramters as they are interpreted literally

app.use("/years/:from-:to/", (req, res) => {
    // if request url is years/2020-2021
    // req.params = { "from": "2020", "to": "2021" }
});

// Regex in parentheses can be used to have more control on string that can be matched
// "/users/:userId(\d+)"
```

## Query Parameters

```js
app.use("/users/:userId", (req, res) => {
    // if request url is users/1234?edit=true
    // req.params = { "userId": "1234" }
    // req.query.edit = "true"
});
```

## Built-in middleware

``express.static`` to serve static assets such as css, images etc.

Example:
```js
app.use(express.static(path.join(__dirname, 'public'))); // Enable static assets from public folder
```

``express.json`` - parses incoming requests with JSON payloads

``express.urlencoded`` - parses incoming requests with urlencoded payloads

## Responses

``res.send()`` to send a typical response sending e.g. a string or html.

``res.sendFile()`` to send e.g. HTML files

``res.sendStatus()``

### Setting Headers / Cookies

`res.set("Set-Cookie", "test=test")`
`res.cookie("test", "test")`

### Response properties

`res.locals` - object that contains response local variables, only available to the views rendered during that request / response cycle. Useful for details that are needed in every view render.
```js
app.use((req, res, next) => {
    res.locals.isAuthenticated = req.user.isLoggedIn;
    next();
});
```

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
## Sending Emails

- third party node package `nodemailer` is a module for Node.js to allow easy email sending
## Validation and Sanitizing Input

- third party node package `express-validator` provides a set of express middlewares that wraps `validator.js` validator and sanitizer functions

## Handling Errors

- Technical / Network Errors
    - e.g. MongoDB server is down
        - Show error page to user
- "Expected" Errors
    - e.g. File cannot be read, database operation fails
        - Inform user, possibly retry
- Bugs / Logical Errors
    - Fix during development!

### Working with Errors

- If error is thrown:
    - Synchronous Code: try-catch
    - Async code: then()-catch(), or try-catch if using async await
    - Should directly handle the error using express error handling function
- For those that won't have error explicity thrown:
    - Should validate values, maybe throw errors and handle directly
- Some options that should be taken depending error:
    - Error page (e.g. 500 error page)
    - Indented page response with error information (e.g. for validation)
    - Redirects (e.g. when user tries to access pages they are not authenticated to)

### Error handling in express

- If synchronous code throws an error, then Express will catch and process it
- For asynchronouos functions, errors must be passed to `next()`so that express can catch and process it.
    - e.g. for promises, chain ``.catch(next)`` to the end
- error-handling middlewares can be used to handle thrown errors
```js
app.use((err, req, res, next) => { // note error-handling middleware have 4 arguments instead of 3
  console.error(err.stack)
  res.status(500).redirect("/500");
})
```
- error-handling middlewares should be defined last, after other ``app.use()`` and route calls

### Uploading files

- To upload a file through a form, enctype `multipart/form-data` is required. `express.urlencoded` does not support multi-part form data parsing, so third package middlewares should be used instead for this purpose.
- `multer` is a popular option for handling `multipart/form-data`