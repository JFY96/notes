# Local Storage

```js
// setter
localStorage.setItem('key', 'value')
// getter
localStorage.getItem('key')
// remove
localStorage.removeItem('key')
// remove all
localStorage.clear();
```

- use `sessionStorage` instead of `localStorage` if you want the data to be maintained only until the browser window closes
- since `localStorage` can only store strings, this can be worked around to store objects by converting to a JSON string
```js
localStorage.setItem('key', JSON.stringify(car));
const keyVal = JSON.parse(localStorage.getItem('key'));
```

# Cookies

- Stored on client-side
- Sensitive data should not be stored in cookies
    - can viewed and manipulated on client side
- Cookies can be configured when a browser is closed ("Session Cookie"), or when a certain expiry date/age is reached ("Permanent Cookie")
- Cookies are attached to request that is sent to server
- can be sent to another url page
- Works well with sessions, but also used for e.g. tracking

## Setting Headers / Cookies in (Node / Express)
## Setting Headers / Cookies in (Node / Express)

`res.set("Set-Cookie", "test=test")`
or
`res.cookie("test", "test")`

Can pass optional parameters to specify certain properties of the cookie:
`res.cookie("test", "test", {expires: new Date(Date.now() + 900000)})`

Here are some common properties:
- `domain`
- `expires`
- `maxAge`
- `secure`
- `httpOnly`

# Session

- Stored on server-side
- Good for storing sensible data that should persist across requests
- Can store anything in sessions
- Often used for storing user data / authentication status
- Associated with user/client via a Cookie (encrypted)
    - the session data is not saved in cookie itself, just the session id
    - that encrypted cookie for session id is used to identify the user

# Using Session (Node / Express)

- install package `express-session`
    - a session middleware that will handle all cookie parsing and read/write

```js
const session = require("express-session");

// various options available, only secret is required
// use the session middleware
app.use(session({ secret: "my secret", resave: false, saveUninitialized: false }));
```

```js
// to store or access session data, use the request property req.session
app.use("/", (req, res, next) => {
    if (req.session.views) req.session.views++;
    else req.session.views = 1;
});
```

- `session.destroy()` - destroys session and unset `req.session` property
- `session.save()` - Save session back to store. This method is automatically called at end of HTTP response so usually this does not be called, but it is useful if you need to make sure the session is saved to the store before a *redirect*, or in *WebSockets*

## Session Store

- default session storage `MemoryStore` is not designed for production
    - less secure and memory limitations
- a store that utilizes a database is a better option
    - e.g. package `connect-mongodb-session` for `MongoDB`:

```js
const session = require("express-session");
const MongoDBStore = require("connect-mongodb-session")(session);

const app = express();
const store = new MongoDBStore({
    uri,
    collection: "sessions" // name of collection to store sessions
});

// use the session middleware
app.use(
    session({
        secret: "my secret", 
        resave: false, 
        saveUninitialized: false,
        store: store
    })
);
```