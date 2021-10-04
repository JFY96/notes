# Intro

Instead of creating an app that hosts both the database and view templates, many developers are separating these into separate projects, hosting their backend and database on a server, then using a service such as GitHub Pages to host their frontend. This technique is sometimes referred to as the `Jamstack`.

There are some benefits in organizing your project this way:
- it allows your project to be more modular instead of combining business logic with view logic
- allows you to use a single backend source for multiple frontend applications, such as website, desktop app, mobile app
	- developers may enjoy this pattern as they like using frontend frameworks to create SPAs

# Server and Client

Don't mistake client application *always* for frontend and server application *always* for backend.

A frontend application is usually seen in the browser, and a backend usually performs business logic that shouldn't be exposed in a browser and often connects to a database.

The terms client and server are a matter of perspective. A backend application (1) which *consumes* another backend application (2) becomes a client application (1) for the server application (2). However same backend application (1) is still the server for another client application which is frontend.

```js
// Frontend -> Backend 1 -> Backend 2 -> Database

// Frontend: Client of Backend 1
// Backend 1: Server for Frontend, also Client of Backend 2
// Backend 2: Server for Backend 1
```

# REST

The structure of an API can take many forms, however, it is conventional to follow REST.
REST stands for Representational State Transfer, a popular and common organizational method for your APIs which corresponds with CRUD actions.

Following established patterns such as REST make your API:
- more maintainable
- easier for other developers to integrate with your API

When designing REST APIs we have to take into account security, performance, and ease of use for API consumers.

**Note most of the subsections below apply to HTTP APIs and may not be specific to REST**

## What is a REST API?

A REST API is an application programming interface that conforms to specific architectural constraints, like stateless communication and cacheable data. It is not a protocol or standard.

It is an architecture that leverages the HTTP protool to enable communication between a client and server application.

A server application that offeres a REST API is also called a *RESTful server*. Servers that don't follow the REST architecture 100% are called RESTish than RESTful.

## Endpoint paths

The important piece we need to think about is how to **organize our endpoint URIs** (Uniform Resource Identifier).

REST APIs are resource based, which means instead of having names like `/getPostComments`, we refer **directly to the resource** and use HTTP verbs such as `GET`, `POST`, `PUT` and `DELETE` to determine the action. Verbs should be avoided in endpoint paths as the HTTP request method already has the verb.

These verbs often correspond to CRUD operations:
- C for Create: HTTP POST
- R for Read: HTTP GET
- U for Update: HTTP PUT
- D for Delete: HTTP DELETE

Typically this takes the form of 2 URI's per resource
- one for the whole collection
- one for a single object in that collection

e.g. a list of blog posts from `/posts` and a specific post from `/post/:postid`

You can also nest collections this way, and this is good practice if it makes sense to group those that have associated information.

e.g. a list of coments on a single post would be `/posts/:postid/comments` and a single comment `/posts/:postid/comments/:commentid`

| Verb | Action | Example | |
| :--- | :--- | :--- | :--- |
| POST | Create | `POST /posts` | Creates a new blog post |
| GET | Read | `GET /posts/:postid` | Fetches a single post |
| PUT | Update | `PUT /posts/:postid` | Updates a single post |
| DELETE | Delete | `DELETE /posts` | Deletes a single post |

## JSON

An aspect of REST is that it is no opinionated about the payload format (JSON, XML etc), but once the format is chosen, you should stick to it for your entire API.

However, there is usually a good reason for using JSON for request payload and also sending responses. JSON is the standard for transferring data and almost every networked technology can use it. Programming languages (both server-side and client) usually have built in methods to encode and decode JSON.

Other ways to transfer data, such as XML, are not as widely supported by frameworks so manipulating this data isn't as easy on the client side. JSON is the preferred choice.

To make sure when our REST API app responds with JSON that clients interpret it as such. `Content-Type` in the response header should be set to `application/json`. Some frameworks may set it automatically, but others may parse the data according to the `Content-Type` response header.

With Express, we can use ~~`body-parser.json()`~~ `express.json()` middleware to parse the JSON request body, and call `res.json` method (instead of `res.send()` or `res.render()`) to pass the data we want to return. 

```js
app.use(express.json());

app.post('/', (req, res) => {
  res.json(req.body);
});
```
`express.json()` parses JSON request body into a JS object and assigns it to the `req.body` object.

## Handling errors

To eliminate confusion for API users when an error occurs, errors should be handled gracefully and a suitable HTTP response codes should be returned. This gives maintainers of the API information to understand the problem that occurerd.

Example of response after failed validation in express:
```js
return res.status(409).json({ error: 'Resource already exists' });
```

## filtering, sorting, pagination

Databases can get very large, so it can be a bad idea to return everything at once. We need ways to filter results and also paginate data so only a few results are retuned at a time.

Filtering and pagination both increase performance by reducing the usage of server resources.

A query string should be able have parameters to filter down the results. For example `/employees?lastName=Smith&age=30`.

Likewise, we can accept a `page` query parameter to return a group of entries in the position `(page - 1) * 20` to `page * 20` if we wanted to return 20 results at a time.

It is also a good idea to allow sorting in the query string, again in the form of query paramteres.
E.g. `http://example.com/articles?sort=+author,-datepublished` will sort by `author` in alphabetical order and datepublished from most recent to least recent (`+` means ascending and `-` means descending).

## Good security practices

Since private information is often sent, the communication between client/server should also be.
Hence using `SSL/TLS` is a must. A `SSL` certificate is usually free or cost very little, so it is important to load onto a server in order to make our REST APIs communicate over secure channels.

For certain REST APIs, it is important to check access of certain resources, so users don't get access to information they are not authorised to. There should be role checks on users to check permissions and admins should have the power to add/remove features from each user accordingly.

## Caching data

To improve performance, we can add caching to return data from local memory cache instead of querying the database every time. This can mean users get data quicker but the data may be outdated.

There are many caching solutions like `Redis`, in-memory caching, and more. Express has `apicache` middleware that doesn't require much configuration.

If caching, then the `Cache-Control` header should be set to help users effectively use your caching system.

## Versioning

There should be different versions of API if making changes to them that may break clients. This versioning can be done according to semantic version (`2.0.6` to indicate major version 2, 6th patch).

This way, old endpoints can be gradually phased out instead of forcing everyone to move to a new one right awawy. The is especially important if API is public.

Versioning is usually done with `/v1/`, `/v2/` etc added at the start of the endpoint URL path, but it can also be done in other ways such as passing a custom request header, or accept header.

# CORS

The **same-origin policy** is an important security measure that basically says "Only requests from the same origin (the same IP address or URL) should be allowed to access this API". 

Here are examples in comparison to `http://store.company.com/dir/page.html`:

| URL | Outcome | Reason |
| :--- | :--- | :--- |
| `http://store.company.com/dir2/other.html` | Same origin | Only path differs |
| `http://store.company.com/dir/inner/another.html` | Same origin | Only path differs |
| `https://store.company.com/page.html` | Failure | Different protocol |
| `http://store.company.com:81/dir/page.html` | Failure | Different port (`http://` is port 80 by default) |
| `http://news.company.com/dir/page.html` | Failure | Different host |

This is a problem for APIs that need access from different origins, so to enable that we need to set up **CORS** (Cross-origin resource sharing).

This is easy to do in Express, as there is a middleware that does all the work. This is provided by npm package `cors`.

Note for development it may be acceptable to just allow access from any origin, but for a real project in a production environment, it is recommended to block access from any origin *except* you frontend website(s).

# CURL for REST APIs

From Wikipedia: "*cURL [...] is a computer software project providing a library and command-line tool for transferring data using various protocols.*"

Since REST is an architecture that uses HTTP, a server that exposes a RESTful API can be consumed with cURL, because HTTP is one of the various protocols.

# REST API with Express

Express is a perfect choice for a server when it comes to creating and exposing APIs to communicate as a client with your server application.

Here are some example routes
```js
app.get('/', (req, res) => {
  return res.send('Received a GET HTTP method');
});
 
app.post('/', (req, res) => {
  return res.send('Received a POST HTTP method');
});
 
app.put('/', (req, res) => {
  return res.send('Received a PUT HTTP method');
});
 
app.delete('/', (req, res) => {
  return res.send('Received a DELETE HTTP method');
});
```

Once the Express server is running (on port 3000), we execute cURL commands to test it:
```bash
curl http://localhost:3000
-> Received a GET HTTP method
 
curl -X POST http://localhost:3000
-> Received a POST HTTP method
 
curl -X PUT http://localhost:3000
-> Received a PUT HTTP method
 
curl -X DELETE http://localhost:3000
-> Received a DELETE HTTP method
```

The above example operated on the root URI, which doesn't really represent a resource in REST. A resource could be a user resource:
```js
app.get('/users', (req, res) => {
  return res.send('GET HTTP method on user resource');
});
 
app.post('/users', (req, res) => {
  return res.send('POST HTTP method on user resource');
});
 
app.put('/users/:userId', (req, res) => {
  return res.send(`PUT HTTP method on user/${req.params.userId} resource`);
});
 
app.delete('/users/:userId', (req, res) => {
  return res.send(`DELETE HTTP method on user/${req.params.userId} resource`);
});
```

Note in order to delete or update a user resource, you would need to know the exact user. For this we assign unique identifiers with parameters in the URI, and the callback function holds the URI's parameter in the request objects properties.

## Making sense of REST with Express

Naming
```js 
// Express Route's Method <=> HTTP Method <=> REST Operation
// Express Route's Path <=> URI <=> REST Resource
```

Reading and writing data from and to a database from a client is possible due to a backend application which enables you to write a interface (REST API) for CRUD operations:

```js
// Client -> (REST API -> Server) -> Database
```

Note the REST API belongs to the server application.

This can be taken one step further as multiple server applications offer REST APIs (or non REST APIs). 
```js
//        -> (GraphQL API -> Server) -> Database
// Client
//        -> (REST API -> Server) -> Database
```

## Example continued

For the example, we use hardcoded data rather than a database.

```js
let users = {
  1: {
    id: '1',
    username: 'Robin Wieruch',
  },
  2: {
    id: '2',
    username: 'Dave Davids',
  },
};
 
let messages = {
  1: {
    id: '1',
    text: 'Hello World',
    userId: '1',
  },
  2: {
    id: '2',
    text: 'By World',
    userId: '2',
  },
};
```

Here are routes for reading whole list of users and a single user by identifier:
```js
app.get('/users', (req, res) => {
  return res.send(Object.values(users));
});
 
app.get('/users/:userId', (req, res) => {
  return res.send(users[req.params.userId]);
});
```

Similarly for the message resource
```js
app.get('/messages', (req, res) => {
  return res.send(Object.values(messages));
});
 
app.get('/messages/:messageId', (req, res) => {
  return res.send(messages[req.params.messageId]);
});
```

For creating a resource (HTTP POST), we can get data from payload in a body `req.body`:
```js
app.post('/messages', (req, res) => {
  const id = uuidv4(); // unique id
  const message = {
    id,
    text: req.body.text,
  };
 
  messages[id] = message;
 
  return res.send(message);
});
```

Remember to use the middleware (based on `body-parser`) that transforms body types from our request object:
```js
app.use(express.json());
```

Note JSON data that comes from the request objects body tag comes as a JSON string, so for types other than a string would need converting.
```js
const date = Date.parse(req.body.date);
const count = Number(req.body.count);
```

To test it with curl:
```bash
curl -X POST -H "Content-Type:application/json" http://localhost:3000/messages -d '{"text":"Hi again, World"}'
```

For deleting a resource (HTTP DELETE), it is very similar:
```js
app.delete('/messages/:messageId', (req, res) => {
  const {
    [req.params.messageId]: message,
    ...otherMessages
  } = messages;
 
  messages = otherMessages;
 
  return res.send(message);
});
```

To test it with curl
```bash
curl -X DELETE http://localhost:3000/messages/1
```

# Architectural constraints for REST

There are 6 architectural constraints that define a RESTful system:
- Client-sever architecture
- Statelessness
- Cacheability
- Layered system
- Code on demand (optional)
- Uniform interface

The technical definition gets complicated, but these are generally covered by default by using Express to output JSON.

## Statelessness

Being stateless is an important characteristic of RESTful services. It should be possible to create multiple instances to balance the incoming traffic evenly between the servers. The term *load balancing* is what is used when having multiple servers at your hands.

A server shouldn't keep the state (e.g. authenticated user), except for in a database, and the client always has to send this information along with each request. Using the user information from the client, a middleware can be used to take care of authentication on an applciation-level and provide the session state (e.g. auth user) to every route in your Express application.

# API Security

Securing your API is important. To authenticate useres, using a username and password would work, but this is complicated by the face that the frontend and backend code is separated.

A common strategy is to generate and pass a secure **token** between our backend and frontend code. Doing so will make sure our user's username and password are not compromised and also give us the ability to expire our user's session for added security.

The basic idea is that when a user signs in to our app, a secure token is created, and then for all subsequent requests that token is passed in the header of our request object

# JSON Web Tokens

From *jwt.io/*:
JSON Web Token (JWT) is an open standard that defines a compact and self-contained way for securely transmitting information between parties as a JSON object. This information can be verified and trusted because it is digitally signed. JWTs can be signed using a secret (with the HMAC algorithm) or a public/private key pair using RSA or ECDSA.

JWTs are most commonly used for authorization. Once a user is logged in, each subsequent request will include the JWT, allowing the server to verify the routes, services, resources that are permitted for that user/token.

JWTs provide a stateless solution to authentication by removing the need to track session data on the server. Instead, JWTs allow us to safely and securely store our session data directly on the client in the form of a JWT.

## Structure

A JWT typically has the structure `xxxxx.yyyyy.zzzzz`, 3 parts seperated by `.`, which are:
- **Header**
	- Contains two parts: type of token (JWT), and signing algorithm being used (e.g. `HMAC` `SHA256` or `RSA`)
	- example. 
	```json
	{
		"alg": "HS256",
		"typ": "JWT"
	}
	```
- **Payload**
	- Payload contains the claims
	- Claims are statements about an entity (user) and additional data, and there are three types:
		- **Registered Claims** - set of predefined claims (not mandatory but recommened) to provide a set of useful, interoperable claims. E.g. *iss* (issuer), *exp* (expiration time), *sub* (subject), *aud* (audience). Note these are 3 chars long as they should be compact.
		- **Public Claims** - defined at will but to avoid collisions they should be defined in the *IANA JSON Web Token Registry*
		- **Private Claims** - custom claims created to share information between parties that agree on using them, that are neither registered or public.
	- example. 
	```json
	{
		"sub": "1234567890",
		"name": "John Doe",
		"admin": true
	}
	```
- **Signature**
	- Take the encoded header, encoded payload, a secret, the algorithm specified, and sign that.
	- e.g. if using `HMAC SHA256` then it will be created like this: 
	```js
	HMACSHA256(base64UrlEncode(header) + "." +base64UrlEncode(payload), secret)
	```
	- signature is used to verify the message wasn't changed along the way, and for tokens with private keys, it can also verify that the sender is who it says it is.

## Usage

JWTs are credentials, so care must be taken to prevent security issues.

To use a JWT, the user agent should send the JWT typically in the **Authorization** header using the **Bearer** schema.
```
Authorization: Bearer <token>
```
If the token is sent in the `Authorization` header, Cross-Origin Resource Sharing (CORS) won't be an issue as it doesn't use cookies.

### General logic on how it may work

- When the user logs in, the backend creates a signed token and returns it in response
	- User sends POST request over HTTPS with authentication details e.g. `{ username, password }`
	- Server determines if valid user
	- If user authentication succeeds, then the server sends back a token 
- The client saves the JWT locally and sends it back in every subsequent request that needs authentication
	- the JWT should contain an encoded user identifier in JSON format signed by our back-end server
	- To prevent potential XSS attacks, the token should be stored in cookies or 
- All requests needing authentication pass through a middleware that checks the provided token and allows the request only if the token is verified

## Advantages

- JWTs remove the need to keep track of sessions on the back end. Instead, session data is encoded in the JWT payload. This means one less data source to worry about in our architecture with sessionless auth.
	- the tradeoff here being the size of a JWT scales proportionally to the size of its payload
		- luckily a payload like `{ user_id, expiry }` is usually enough
- The auth pipeline is simple, so implementation should be intuitive and quick

## Best practices against common attacks

JWTs and best practices can protect us against:
- XSS and SQL / noSQL injection attacks
- Brute-forcing of user credentials attacks
- An attacker getting a hold of a user’s JWT / cookie
- An attacker getting a copy of or read credentials to our database

### XSS attacks

TODO

## With Node/Express

```
npm install jsonwebtoken
```

If using `PassportJS` then the `passport-jwt` strategy can be used.

# HATEOAS

Hypermedia as the Engine of Application State

TODO