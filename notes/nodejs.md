# Node

Runtime environment for JavaScript code.

# Node REPL

Nodes programming language environment (console window)

- R - Read (User Input)
- E - Evaluate (User Input)
- P - Print (Output / Result)
- L - Loop (Wait for new input)

# How the web works
Client -> Request -> Server -> Response -> Client
- Domain and IP
- Client and Server
- Request and Response

# HTTP, HTTPS

Hyper Text Transfer Protocol - A Protocol for Transferring Data which is understood by Browser and Server

Hyper Text Transfer Protocol Secure - HTTP + Data Encryption (during Transmission)

# Some common core modules

- http - e.g.launch a server, send requests
- https - e.g. launch a SSL server
- fs
- path - utility for working with file and directory paths
- os
- events

## Path

``__dirname`` or ``path.dirname(__filename)`` gives the directory name of the current module.

Use path methods like ``path.join()`` over basic string operations as it will deal with unneccessary delimiters, and it will work out which path separator is requied (forward/back slash depending on Windows/Linux/other platforms).

``path.dirname(require.main.filename)`` gives a path for the entry point of the applilcation. Preferable to using ``__dirname``.

# Node lifecycle and event loop

node app.js -> Start executing script -> Parse code, register variables, functions, events -> event loop (Keeps running as long as event listeners are registerd)

A simple http server in node.js will register an event loop which will keep listening for http requests. createServer() never finishes by default.

Note: this is single-threaded

process.exit() will stop the event loop

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

# Import / Export
TODO
## CommonJS
require
module.exports / exports (shortcut for node)

TODO

``require.main.filename``, only available in a CommonJS environment, gives the entry point for the current application.
``require.main`` servers the same purpose as ``process.mainModule``. That is now deprecated as the ``process`` global object is shared with non-CommonJS environment, but ``mainModule`` is a CommonJS only feature, so use within ECMAScript modules is unsupported.

## ES6 Modules

TODO (code-snippet)

