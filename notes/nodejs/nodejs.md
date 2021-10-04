# Node

Runtime environment for JavaScript code.

From Node.js website:
`As an asynchronous event driven JavaScript runtime, Node is designed to build scalable network applications`

On a basic level, node allows you to run JS code on a machine (e.g. local computer/server) without having to go through a web browser (where JS is desinged to run in).
# Overview

- Node.js runs the V8 JavaScript engine, which allows it to be very performant.
- Node is an `asynchronous event driven` JavaScript runtime.
	- `asynchronous` in this sense means you do not try to predict the exact sequence in which every line will run
	- `event driven` means the code is written as a collection of smaller functions that get called in response to specific events such as network request
	- events are things such as network requests and database queries
		- This functionality is facilitated through the use of callbacks
- A Node.js application runs in a single process, without creating a new thread for every request
	- Node.js provides a set of asynchronous I/O primitives in its Standard library that prevent JS code from bloacking.
	- Generally libraries in Node.js are written using non-blocking paradigms
- when Node.js perforams an I/O operation (e.g. reading from network, accessing database or filesystem), instead of blocking the thread and wasting CPU cyles waiting, Node.js will resume the operations when the response comes back.

There are 2 types of events in Node:
- System Events: C++ core from a library called libuv (e.g. finished reading a file)
- Custom Events: JavaScript core

# Difference betwen Node.js and browser environment

- In node there is no `document`, `window` and all other objects that are provided by the browser
- Node.js emits browser specific JS APIs but adds support for more traditional OS APIs provided thorugh its modules, like filesystem access functionality
- In node you control the environment usually, since you know which version of Node.js you will run the application on. In the browser you don't get to choose which browser your visitors will use
	- so you can write all the modern ES6+ JavaScript that your Node.js version supports without using something like Babel to transpile your code
- Node.js uses the CommonJS module system, while in the browser the ES Modules standard is popular
	- `require()` in Node.js and `import` in the browser

# Node in the CLI
 
After installation, `node` should be a globally available command.

To execute a file (in current directory):
```bash
node app.js
```

# Node REPL

Nodes programming language environment (console window)

- R - Read (User Input)
- E - Evaluate (User Input)
- P - Print (Output / Result)
- L - Loop (Wait for new input)

Can be accessed in the terminal by:
```bash
node
```

You can explore properties/methods by doing e.g. `Number.` (with a dot) and press `tab`.
This also works with `global.`

If you type `_` after some code, it will print the result of the last operation.

Dot commands such as `.help`, `.editor`, `.break`, `.clear`, `.load`, `.save`, `.exit` are available.
# Common core modules

- `http` - networking e.g.launch a server, send requests
- `https` - e.g. launch a SSL server
- `fs` - working with file system
- `path` - utility for working with file and directory paths
- `os`
- `events` - work with events in Node.js

## Path

``__dirname`` or ``path.dirname(__filename)`` gives the directory name of the current module.

Use path methods like ``path.join()`` over basic string operations as it will deal with unneccessary delimiters, and it will work out which path separator is requied (forward/back slash depending on Windows/Linux/other platforms).

``path.dirname(require.main.filename)`` gives a path for the entry point of the applilcation. Preferable to using ``__dirname``.

## http and https - building http server

`createServer()` method of `http` creates a new HTTP server and returns it. The server returned can then be set to listen to a specific port and host name.

```js
const http = require('http');

const hostname = process.env.HOST; // e.g. '127.0.0.1'
const port = process.env.PORT; // e.g. 3000

const server = http.createServer((req, res) => {
  res.statusCode = 200; // successful response
  res.setHeader('Content-Type', 'text/html');
  res.end('<h1>Hello World</h1>'); 
});

server.listen(port, hostname, () => {
	// callback
  console.log(`Server running at http://${hostname}:${port}/`);
});
```

When a request is received, the `request event` is called, providing a `request` (`http.IncomingMessage` object) and a response (`http.ServerResponse` object) to work with. These two objects are essential to handle the HTTP call. In the above example these are the `req` and `res` parameters.

The request provides the request details which can be used to access headers and request data.

The response is used to return data to the caller.

### Example HTTP GET Request

From nodejs.dev
```js
const https = require('https')
const options = {
  hostname: 'example.com',
  port: 443,
  path: '/todos',
  method: 'GET'
}

const req = https.request(options, res => {
  console.log(`statusCode: ${res.statusCode}`)

  res.on('data', d => {
    process.stdout.write(d)
  })
})

req.on('error', error => {
  console.error(error)
})

req.end()
```
### Example HTTP POST Request

From nodejs.dev
```js
const https = require('https')

const data = new TextEncoder().encode(
  JSON.stringify({
    todo: 'Buy the milk 🍼'
  })
)

const options = {
  hostname: 'whatever.com',
  port: 443,
  path: '/todos',
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
    'Content-Length': data.length
  }
}

const req = https.request(options, res => {
  console.log(`statusCode: ${res.statusCode}`)

  res.on('data', d => {
    process.stdout.write(d)
  })
})

req.on('error', error => {
  console.error(error)
})

req.write(data)
req.end()
```

`PUT` and `DELETE` also use this same format but with the corresponding `options.method` value

## fs system
```js
const fs = require('fs');
```
- contains methods that let you work with the file system
- not all methods are async by default, but they can also work synchronously by appending `Sync`
	- e.g. `fs.write()` and `fs.writeSync()`

### writing files

```js
const content = 'Some content!';
fs.writeFile('/Users/joe/test.txt', content, err => {
  if (err) {
    console.error(err);
    return;
  }
  //file written successfully
})
```
By default, this will replace contents of the file if it already exists. The behaviour can be modified by specifying a flag:
- `r+` open the file for reading and writing
- `w+` open the file for reading and writing, positioning the stream at the beginning of the file. The file is created if it does not exist
- `a` open the file for writing, positioning the stream at the end of the file. The file is created if it does not exist
- `a+` open the file for reading and writing, positioning the stream at the end of the file. The file is created if it does not exist

```js
fs.writeFile('/Users/joe/test.txt', content, { flag: 'a+' }, err => {});
```
### reading files
```js
fs.readFile('/Users/joe/test.txt', 'utf8' , (err, data) => {
  if (err) {
    console.error(err);
    return;
  }
  console.log(data);
})
```

`fs.readFile()` reads the full content of the file in memory before returning the data, so big files will impeact memory consumption and speed of execution. A better option is to read the file using `streams`.

## events module

```js
const EventEmitter = require('events');
const door = new EventEmitter();
```

The `EventEmitter` class is used to handle events. This object has `on` and `emit` methods:
- `emit` is used to trigger an event
- `on` is used to add a callback function to be executed when the event is triggered

```js
eventEmitter.on('start', () => { // same as eventEmitter.addListener
  console.log('started');
});

eventEmitter.emit('start');
```

The callback can have arguments by passing them as additional arguemnts in emit:
```js
eventEmitter.on('start', (start, end) => {
  console.log(`from ${start} to ${end}`);
});

eventEmitter.emit('start', 1, 100);
```

`EventEmitter` also has several other methods like:
- `once()` - add a one time listener
- `removeListener()` or `off()` - remove event listener from an event
- `removeAllListeners()` - remove all listeners from event

# exit from a node app

To terminate a node application:
- `ctrl C` if running a program in the console
- `process` core module provides `process.exit()` which will force the process to terminate
	- this is usually a bad idea as any pending callbacks, network requests being sent, any filesystem processes or processing writing to stdout/stderr will all be aborted right away
	- When program ends, node will return an exit code
- send a `SIGTERM` signal with a process signal handler setup
	- Signals are a POSIX intercommunication system: a notification sent to a process in order to notify it of an event that occurred
	- `SIGKILL` is the signal that tells a process to immediately terminate (like `process.exit()`)
	- `SIGTERM` is the signal that tells a process to gracefully terminate. 
```js
// handler
process.on('SIGTERM', () => {
  server.close(() => {
    console.log('Process terminated')
  })
});

// sending signal
// (can be done inside program, or another function/app that knows the PID of the process to be terminated)
process.kill(process.pid, 'SIGTERM');
```

# Environment Variables

- `process` core module provides `env` property which contains all environment variables that were set up the process was started
- you can create an `.env` file in the root directory of project, then use `dotenv` package to load them during runtime
```bash
# .env file
USER_ID="239482"
USER_KEY="foobar"
NODE_ENV="development"
```
```js
require('dotenv').config();

process.env.NODE_ENV // "development"
```

# Node lifecycle and event loop

node app.js -> Start executing script -> Parse code, register variables, functions, events -> event loop (Keeps running as long as event listeners are registerd)

A simple http server in node.js will register an event loop which will keep listening for http requests. `createServer()` never finishes by default.

Note: this is single-threaded

# Request & Response Headers

https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers

# Streams & Buffers
TODO

Used to parse request data in chunks - especially needed when handling and manipulate large data like video, large files.

A buffer is a temporary memory that a stream takes to hold some data until it is consumed.

TODO

# Single Thread, Event Loop

https://nodejs.org/en/docs/guides/event-loop-timers-and-nexttick/

Event Loop - handle event callbacks

    Each iteration:
    - Timers (Execute setTimeout, setInterval callbacks)
    - Pending callbacks (Execute I/O related callbacks that were deferred)
        - I/O - Input & Output - Disk and Network Operations (~Blocking Operations)
    - Poll (Retrieve new I/O events, execute their callbacks)
    - Check (Execute setImmediate() callbacks)
    - Close Callbacks (Execute all 'close' event callbacks)
    - Then exit if no more open events callbacks (In a node environment it will not exit as there is a listener)

Worker Pool - do heavy lifting (different threads!)
TODO

# Dont Block the Event Loop

https://nodejs.org/en/docs/guides/dont-block-the-event-loop/

Using non-blocking asynchronous APIs is important (even more so than in the browser) as Node is a single-threaded event-driven execution environment. There shouldn't be any synchronous methods that take a long time to complete, as they will not only block the current request, but every other request being handled by the web application.

## CommonJS

``require.main.filename``, only available in a CommonJS environment, gives the entry point for the current application.
``require.main`` servers the same purpose as ``process.mainModule``. That is now deprecated as the ``process`` global object is shared with non-CommonJS environment, but ``mainModule`` is a CommonJS only feature, so use within ECMAScript modules is unsupported.

## process.env

- `process.env` global variable is injected by Node at runtime for your applicatoin to use
- it represents the state of the system environment when you application starts
- e.g. if the system has a `PATH` variable set, it can be accessed via `process.env.PATH`

## ES6 Modules (Import/Export) support

To use this in node, either:
- Save files with `.mjs` extension
- Add `{ "type": "module }` to `package.json`