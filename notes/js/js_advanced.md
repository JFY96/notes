Great explanation video by Philip Roberts - Channel JSConf : https://www.youtube.com/watch?v=8aGhZQkoFbQ

# Call Stack

- one thread == one call stack == one thing at a time

- Last-In, First-Out (LIFO) data structure containing the address at which execution will resume
    - So it basically records where in the program we are
        - if we step into a function, it is pushed onto the stack
        - if we return from a function, it is popped off the stack

## Stack Trace

- Stack trace is a list of method calls that the application was in the middle of when an error/exception was thrown

## Stack Overflow

- "Stack Overflow" is when the maximum call stack size is exceeded 
- for example, when there are too many calls from a recursive function

# Asynchronous JavaScript

- "Blocking" code happens when a piece of code on the call stack takes a long time to run. To prevent this, asynchronous code is required, and it is possible because of:
    - Execution Stack
    - Web/Browser APIs
    - Task/Function Queue
    - Event Loop

# Concurrency Model and the Event Loop

https://developer.mozilla.org/en-US/docs/Web/JavaScript/EventLoop

- One thing a a time, except not really.

## Process
- Methods are added onto the call stack 
- If a method provided by the web apis is called (e.g. setTimeout), this is handled by the browser
- That method is removed from the call stack and the rest of the code continues to execute
- When the web api function is done, the callback is pushed onto the task queue
- Once the stack is clear, the event loop runs and the callback from the queue is pushed onto the stack

## Event Loop
- Event loop has one simple job - look at the stack and task queue, and if the stack is empty, it takes the first thing on the queue and pushes it onto the stack

## Examples

- If `setTimeout` is called with a time of `0`, the callback from this will not run until the rest of the synchronous code is done, as it needs to wait for the stack to clear
    - It is "deferred"
    - So the time for `setTimeout` isn't a guaranteed time to execution, its the minimum time to execution
- When registering something like a click event, this is pushed and handled by the web apis. When the click event is invoked, the web api is triggered and then the callback is pushed onto the queue, and dealt with when the call stack is clear

## Render Queue

- Browser would ideally like to repaint every 16.6ms (60FPS)
- However, it cannot render if there is code on the call stack (needs to be clear)
- Render Queue has higher priority over the Callback Queue
- "Don't block the event loop" refers to having blocking code on the call stack which prevents rendering and blocks screen actions, causing a slow an non fluid UI
    - Having code that takes a long time run asynchronously rather than in the call stack gives the browser the chance to render in between callbacks

## Tips

- If something that is handled by the web api is triggered alot (such as a scroll event), consider `debouncing`, otherwise the callback queue will get clogged up

# Heap



# Links

https://javascript.plainenglish.io/node-call-stack-explained-fd9df1c49d2e

https://medium.com/@Rahulx1/understanding-event-loop-call-stack-event-job-queue-in-javascript-63dcd2c71ecd

https://github.com/8483/notes/blob/master/topics/javascript.md

