# General idea for implementing authentication

- User sends login request
- if valid (validate against database), create session to connect user
- send 200 response and store session id as cookie on client
- Session stores user details, allowing the user access to certain resources

# Express - Protecting Routes

- Middleware should be added to check for user authentication for routes that need to be protected

```js
const auth = (req, res, next) => {
    if (!req.session.authenticated) {
        return res.redirect("/login");
    }
    next();
}

// Can chain middleware for the routes that want to be protected
app.get("/admin", auth, (req, res, next) => {
    // code
})
```

# Encryption

- passwords should be stored in a hashed way
- npm package `bcryptjs` is an optimised bcrypt in JS
```js
bcrypt.hash(password, 12) // to hash a password (async)
bcrypt.compare(password, hashedPassword) // to check/compare a password (async)
```
- the `crypto` node library has useful methods supporting hashes, authentication, ciphers etc.
    - e.g. if we wanted a token to use for password resetting, `crypto.randomBytes` can crytographically generate a number of bytes for this purpose

# CSRF Attacks

- Cross-Site Request Forgery
- Exploit of a website where unauthorized commands are submitted from a user that the web app trusts
- 

## CSRF Token

- A CSRF Token is a unique, unpredictable value that is generated server side and transmitted to client in a way that it is included in a subsequent HTTP request made by client. 
- When a later request is made, the expected token included in the request is validated against. The request is rejected if token is missing/invalid.

- npm package `csurf` is a CSRF Protection Middleware for express
```js
const csrf = require("csurf");
const csrfProtection = csrf(); // can pass options if required 
app.use(csrfProtection); // middleware

// The CSRF token is obtained from the req.csrfToken() call on the server-side
```
For POSTs, hidden field in form should have name `_csrf` (EJS used here):
```html
<input type="hidden" name="_csrf" value="<%= csrfToken %>"
```