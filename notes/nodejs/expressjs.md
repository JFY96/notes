# Overview

- Express is the most popular backend web framework for Node.js
- Express is a minimal and flexible framework that provides a bunch of utility functions and tools for web applications
- It provides:
	- Routes (handlers for requests with different HTTP verbs at different URL paths)
	- Integration with rendering engines ("view") to generate responses by inserting data into the templates
	- Ability to set web application settngs like port, location of files (such as templates for rendering) to serve
	- Ability to add additional request "middleware" at any point when handling the request
- Express relies heavily on middleware functions

# Unopinionated

Express is unopinionated - there is no "right way" to handle any particular task so you can use the most suitable tools required. You can insert almost any compatible middleware into the request handling chain, in any order you like, so it makes it very flexible.

There are third party middleware packages to addresses almost any web development problem (e.g. dealing with cookies, sessions, user logins, URL parameteres, POST data, security headers), but choosing the right packages can be a challenge due to the amount of choices.

Since there is no "correct way" to structure a Express application, optimal ways to handle certain problems may not be well-understood or well-documented. This means examples found online for certain middlewares may not be optimal, especially when paired with other middlewares.

# Express code example

```js
const express = require('express');
const app = express();
const port = 3000;

app.get('/', (req, res) => {
  res.send('Hello World!')
});

app.listen(port, () => {
  console.log(`Example app listening on port ${port}!`)
});
```

In this example the code starting with `app.get` shows a *route definition*. This method specifies a callback function that is invoked when there is a HTTP `GET` request with a path `'/'` (relative to site root). The callback function takes `request` and `response` objects as arguments.

# Routing

Routing refers to how an application's endpoints (URIs) response to client requests.
## Route methods

A route method is derived from one of the HTTP methods. They are attached to an instance of the `express` class. Express provides methods to define route handlers for all HTTP verbs:

checkout(), copy(), **delete()**, **get()**, head(), lock(), merge(), mkactivity(), mkcol(), move(), m-search(), notify(), options(), patch(), **post()**, purge(), **put()**, report(), search(), subscribe(), trace(), unlock(), unsubscribe()

`app.all()` is a special routing method - it will be called in response to any HTTP method. It is used for loading middleware functions at a particular path for all request methods.

## Route paths

Route paths define the endpoints at which requests can be made. They can be strings, string patterns, or regular expressions.

- The characters `?`, `+`, `*`, and `()` are subsets of their regular expression counterparts
	- `?` endpoint must have 0 or 1 of the preceding character (or group of)
	- `+` endpoint must have 1 or more of preceding character (or group of)
	- `*` endpoint may have an arbitrary string where the `*` character is placed
	- `()` grouping a set of characters to perform another operation on
- The hyphen `-` and the dot `.` are interpreted literally by string-based paths
- To use `$` in a path string, enclose it escaped in `[]` (e.g. `/data/$book` would be `/data/([\$])book`)

```js
// String examples
app.get('/', ...); // root route /
app.get('/about', ...); // match requests to /about
app.get('/random.text', ...); // match requests to /random.text
app.get('/ab?cd', ...); // match requests to /acd and /abcd
app.get('/ab+cd', ...); // match requests to /abcd, /abbcd, /abbbcd etc
app.get('/ab*cd', ...); // match requests to /abcd, /abxcd, /abRANDOMcd, /ab123cd etc
app.get('/ab(cd)?e', ...); // match requests to //abe and /abcde
// Regular expression examples
app.get(/a/, ...); // match requests to anything with an 'a' in it
app.get(/.*fly$/, ...); // match requests to butterfly and dragonfly, but not butterflyman, dragonflyman etc
```

### Route Parameters

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
// e.g. "/users/:userId(\d+)" to only allow userId with digits
```

### Query Parameters

```js
app.use("/users/:userId", (req, res) => {
    // if request url is users/1234?edit=true
    // req.params = { "userId": "1234" }
    // req.query.edit = "true"
});
```
## Route handlers

Route handlers can be a function, an array of functions, or combinations of both.

```js
app.get('/example', function (req, res, next) {
  console.log('first function callback');
  next();
}, function (req, res) {
  res.send('response!');
})
```

```js
const cb0 = (req, res, next) => {
  console.log('CB0')
  next();
}

const cb1 = (req, res, next) => {
  console.log('CB1')
  next();
}

app.get('/example', [cb0, cb1], function (req, res) {
  res.send('response!');
});
```
## app.route()

`app.route()` allows you to create chainable route handlers for a route path. This is helpful as it reduces redundancy and typos.

```js
app.route('/book')
  .get(function (req, res) {
    res.send('Get a random book')
  })
  .post(function (req, res) {
    res.send('Add a book')
  })
  .put(function (req, res) {
    res.send('Update the book')
  });
```

## express.Router

Used to create modular route handlers. A `Router` instance is a complete routing and middleware system ("mini-app").

Works like application-level middleware but bound to an instance of `express.Router()` instead.
This can be used to split routes across files in a nice way.

Requests can be filtered by path (`'/'`) and method (`POST`, `GET`)

Example:
```js
/* routerExample file */

const express = require('express');
const router = express.Router();

// define middlewares and route
// router.use(...); router.get(...); etc

module.exports = router;
```

```js
/* app */

const routerExample = require('./routerExample');

// load routes and middlewares from routerExample
app.use(routerExample);

// OR

// filter route middleware paths to /example
app.use('/example', routerExample);
```

# Middleware

Set of functions that excecute during the request-response lifecycle.

Request -> Middleware -> ... -> Middleware -> Response

A middleware is just a plain JavaScript function that Express will call for you between the time it receieves a network request and the time it fires off a response (it sits in the *middle*).

Middleware functions have access to the request object, response object, and the next middleware function in the lifecycle. They can:

- Execute any code
- Make changes to the `req` and `res` objects
- End the request-response cycle
- Call the next middleware function in the stack, using `next()`

The **only** difference between a middleware function and a route handler callback is that middleware functions have a third argument `next`.

If not ending the cycle in the current middleware function, then it must call `next()` to pass control to next middleware function (or it will hang).

Note the order that middleware gets executed in your app matters. Middleware will always run in the order that they are instantiated using `app.use()`.

## Application-level middleware

Middleware bound to an instance of the `app` object (Express application).

```js
app.use() // executed for any HTTP request
app.get() // HTTP GET
app.post() // HTTP POST
app.put() // HTTP PUT
```

## Router-level middleware

Same as application-level middleware but instead bound to an instance of `express.Router()`. This middleware allows us to group the route handlers for a particular part of a site together and access them using a common route-prefix.

## Error-handling middleware

Same as other middleware but the functions accept four arguments instead of three

```js
app.use(function (err, req, res, next) {
  console.error(err.stack);
  res.status(500).send('Something broke!');
});
```

These must be called after all other `app.use()` and route calls so that they are the last middleware in the request handling process.

Express has a build-in error handler, which is added at the end of the middleware function stack.

You can pass an error to `next()` then handle it in an error-handling middleware, or if not, it will be handled by the built-in error handler.

## Built-in middleware

``express.static`` to serve static assets such as css, images etc.

Example:
```js
app.use(express.static(path.join(__dirname, 'public'))); // Enable static assets from public folder

app.use('/media', express.static('public-media')); // files are loaded with virtual prefix "/media"
```

``express.json`` - parses incoming requests with JSON payloads

``express.urlencoded`` - parses incoming requests with urlencoded payloads

## Third-party middleware

Third party middleware that are installed using npm and loaded into the application


# Request and Response

`req` or `request` is an object that has data about the incoming request, such as:
- exact URL that was visited
- parameters in the URL
- `body` of the request (useful if user submitting form with data in it)

`res` or `response` is an object that represents the response that Express is going to send back to the user. Typically, you use the information in `req` to determine what your're going to do with the `res` by calling `res.send()` or another method on the object
## Response Methods

These methods on the `res` object can be used to send a response to the client. Doing so will terminate the request-response cycle (if none are called then the request will be left hanging).

- `res.download()` - Prompt a file to be downloaded.
- `res.end()` - End the response process.
- `res.json()` - Send a JSON response.
- `res.jsonp()` - Send a JSON response with JSONP support.
- `res.redirect()` - Redirect a request.
- `res.render()` - Render a view template.
- `res.send()` - Send a response of various types e.g. a string or html
- `res.sendFile()` - Send a file as an octet stream e.g. HTML files
- `res.sendStatus()` - Set the response status code and send its string representation as the response body

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

A template is a text file defining the structure or layout of an output file, with placeholders to represent where data will be inserted when the template is rendered. Also referred to as *views* in Express.

At runtime, the template engine replaces variables in the template file with actual values,
then compiles it into HTML before sending to the client. 

Popular options for express are EJS or Pug.

For templating engines that are supported by express js by default, to load it into the app do:
```js
app.set('view engine', 'ejs'); // Load EJS
```
If that isn't the cass, then it will need to be imported then defined using ``app.engine()`` before doing the above.

It will then use the templating files (e.g. ``.ejs`` files if using EJS) in the ``/views`` directory by default (to change, set app ``views`` property).
```js
// Set directory to contain the templates ('views')
app.set('views', path.join(__dirname, 'views'));
```

Then create routes to render the ``/views`` files (using the ``res.render`` method to send responses) e.g.

```js
res.render('index'); // if view engine is set, do not need to specify the extension
```

```js
res.render('index', {title: '', message: msg}) // render dynamic content, pass variables to template
```

### EJS

Like HTML but <% tags to insert JS code / variables.

```ejs
<% if (items.length > 0) { %>
    <% for (let item of items) { %>
        <h2><%= item.name %></h2>
    <% } %>    
<% } %>
```

- `<%= value %>`    output value is HTML escaped
- `<%- value %>`    output value is unescaped

Includes

```ejs
<%- include('includes/header.ejs') %>

<%- include('includes/tabs.ejs', {tab: tab}) %>
```

### Pug

Example template file from MDN:
```pug
doctype html
html(lang="en")
  head
    title= title
    script(type='text/javascript').
  body
    h1= title

    p This is a line with #[em some emphasis] and #[strong strong text] markup.
    p This line has un-escaped data: !{'<em> is emphasised</em>'} and escaped data: #{'<em> is not emphasised</em>'}.
      | This line follows on.
    p= 'Evaluated and <em>escaped expression</em>:' + title

    <!-- You can add HTML comments directly -->
    // You can add single line JavaScript comments and they are generated to HTML comments
    //- Introducing a single line JavaScript comment with "//-" ensures the comment isn't rendered to HTML

    p A line with a link
      a(href='/catalog/authors') Some link text
      |  and some extra text.

    #container.col
      if title
        p A variable named "title" exists.
      else
        p A variable named "title" does not exist.
      p.
        Pug is a terse and simple template language with a
        strong focus on performance and powerful features.

    h2 Generate a list

    ul
      each val in [1, 2, 3, 4, 5]
        li= val
```
## Sending Emails

- third party node package `nodemailer` is a module for Node.js to allow easy email sending
## Validation and Sanitizing Input

- For actions that have data from user input (e.g. submitting a form), the data should be validated and sanitized
	- Validation checks that entered values are appropriate for each field (correct range, format, etc) and that values have been supplied for all the requried fields
	- Sanitization removes/replaces characters in the data that might potentially be used to send malicious content to the server
- A popular third party node package `express-validator` provides a set of express middlewares that wraps `validator.js` validator and sanitizer functions
- Example of usage:
```js
const { body, validationResult } = require('express-validator');

// additional middleware before handling the data
[
	body('name', 'Empty name').trim().isLength({ min: 1 }).escape(), 
	(req, res, next) => {/*processing*/}
]

body('age', 'Invalid age').optional({ checkFalsy: true }).isISO8601().toDate()
```


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