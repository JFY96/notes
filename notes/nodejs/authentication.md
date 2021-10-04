# General idea for implementing authentication

- User sends login request
- if valid (validate against database), create session to connect user
- send 200 response and store session id as cookie on client
- Session stores user details, allowing the user access to certain resources
# CSRF Attacks

- Cross-Site Request Forgery
- Exploit of a website where unauthorized commands are submitted from a user that the web app trusts

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

# PassportJS

A better way to do authentication is to use PassportJS - an Authentication middleware for Nodejs.

```bash
npm install passport
```

PassportJS uses what they call *Strategies* to authenticate users. They have over 500 of these strategies - e.g. using username and apssword, Twitter, google oauth, jwts etc.

## username-and-password (also called LocalStrategy by them)

Way to authenticate users via a username and password - traditional and widely used.
Provided by the `passport-local` module.

```bash
npm install passport-local
```

Example using mongoose (mongoDB) to get user details:

```js
const session = require("express-session");
const passport = require("passport");
const LocalStrategy = require("passport-local").Strategy;

const User = require('../models/user');

// (1) - Set up LocalStrategy
passport.use(
	new LocalStrategy((username, password, done) => {
		User.findOne({ username: username }, (err, user) => {
			if (err) return done(err);
			if (!user) return done(null, false, { message: "Incorrect username" });
			if (user.password !== password) return done(null, false, { message: "Incorrect password" });
			return done(null, user);
		});
	})
);

// (2) - Sessions and serialization
passport.serializeUser(function(user, done) {
	done(null, user.id);
});

passport.deserializeUser(function(id, done) {
	User.findById(id, function(err, user) {
		done(err, user);
	});
});

app.use(session({ secret: "cats", resave: false, saveUninitialized: true }));
app.use(passport.initialize());
app.use(passport.session());

// (3) - route for log in POST
app.post('/log-in',
	passport.authenticate('local', {
		successRedirect: '/',
		failureRedirect: '/',
	})
);

// (4) - route for log out POST
app.get('/log-out', (req, res) => {
	req.logout();
	res.redirect('/');
});
```

`(1)` sets up the function which will be called when using `passport.authenticate()`. It basically takes a username and password, tries to find the user in our DB, and then makes suer that the use's password matches the given password. If it all works out, then it authenticates our user and moves on.

To allow our user to stay logged in, the app passport will use some data to create a cookie which is stored in the user's browser. `(2)` defines what bit of information passport is looking for when creating and decoding the cookie. We need to define these functions to make sure that whatever bit of data it's looking for actually exists in our database.

`(3)` sets up the route for our log in post (e.g. when form is submitted). It calls `password.authenticate()` that performs numerous functions behind the scenes. Among other things, it checks request body for `username` and `password` parameters then runs the `LocalStrategy` function defined earlier to check if username/password exist in our db. It then creates a session cookie that gets stored in users browser, that is used in future requests to see if the user is logged in. It can also redirect to different routes depending on whether login was success/failure.

`(4)` defines the log out route. The passport middleware adds a logout function to the `req` object so it is as easy as calling `req.logout()`. 

## express tip

You can acess various local variables throughout your entire app with the `locals` object, so we can write a custom middleware to simpliify how to access our current user in our views:
```js
app.use(function(req, res, next) {
  res.locals.currentUser = req.user;
  next();
});
```
Make sure to insert this between instantiating the passport middleware and before rendering views.

# Encryption

- passwords should be stored in a hashed way
- npm package `bcryptjs` is an optimised bcrypt in JS
	- `bcrypt` (written in C++) is quicker, but it may be a pain to get installed
```js
bcrypt.hash(password, 12) // to hash a password (async)
bcrypt.compare(password, hashedPassword) // to check/compare a password (async)
```
- the `crypto` node library has useful methods supporting hashes, authentication, ciphers etc.
	- e.g. if we wanted a token to use for password resetting, `crypto.randomBytes` can crytographically generate a number of bytes for this purpose

Using this with the about passport example:
```js
// changed direct string comparison "if (user.password !== password)" to instead use bcrypt compare 
passport.use(
	new LocalStrategy((username, password, done) => {
		User.findOne({ username: username }, (err, user) => {
			if (err) return done(err);
			if (!user) return done(null, false, { message: "Incorrect username" }); // user not found
			bcrypt.compare(password, user.password, (err, res) => {
				if (res) { // passwords match! log user in
					return done(null, user);
				 } else { // passwords do not match!
					return done(null, false, { message: "Incorrect password" });
				 }
			});
		});
	})
);

// creating a password hash in the sign up
app.post('/sign-up', (req, res, next) => {
	bcrypt.hash(req.body.password, 10, (err, hashedPassword) => {
		if (err) next(err);
		
		const user = new User({
			username: req.body.username,
			password: hashedPassword,
		}).save()
			.then(() => res.redirect('/'))
			.catch(err => next(err));
	});
});
```