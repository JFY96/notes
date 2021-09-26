# V8 JavaScript Engine

- Engine that powers Google Chrome and Node.js
- The thing that takes our JavaScript and executes it while browsing on a Chrome
- V8 provides the runtime environment in which JS executes
- The DOM and other web APIs are provided by the browser

## Performance

V8 is written in `C++` and is continuously improved. The implementation details change over time as the engine evolves, in order to speed up the Web and the Node.js ecosystem.

## Compilation

JS is generally considered a interpreted language, but modern JS enginges no longer just interpret JS, they compile it.

JavaScript is internally compiled by V8 with **just-in-time** (JIT) **compilation** to speed up execution.

Compiling JavaScript makes sense because while it may take a little longer to have the JavaScript ready, once done its going to be much more performant than purely interpreted code.

# Other Engines

- Firefox uses SpiderMonkey
- Safari uses JavaScriptCore