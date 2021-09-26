# Synchronous & Async JavaScript
- JavaScript is a Single-Threaded language, so it has one call stack and one memory heap. This means code is executed in order and must finish one piece before moving onto the next. However, since it is synchronous, this can be harmful (e.g. if a function takes a long time, it freezes everything up).

- The JavaScript Engine (V8 or others) supports asynchronous functions (functions that can happen in the background while the rest of your code executes) with the Web API handling these tasks in the background.
    - The call stack recognizes functions of the Web API and these are handled by the browser
    - Once those tasks are complete, they return and are pushed onto the stack as a callback

# Callbacks

- Async functions were most commonly handled with callbacks in the recent past
- [from MDN:](https://developer.mozilla.org/en-US/docs/Glossary/Callback_function)
```
A callback function is a function passed into another function as an argument, which is then invoked inside the outer function to complete some kind of routine or action.
```
- so callbacks are just functions that get passed into other functions
- callbacks can get out of hand when you need to chain several of them together, causing "callback hell"

# Promises

- A promise is an object that might produce a value at some point in the future
- Promises are asynchronous and have 3 states:
  - *Pending*
  - *Fulfilled*
  - *Rejected*
- Basic syntax:
```js
const p = new Promise((resolve, reject) => {
	
	// Do an async task async task and then...

	if(/* good condition */) {
    const result = {success: trueo};
		resolve(result);
	}
	else {
    const reason = new Error("Failure!");
		reject(reason);
	}
});

p.then(function(result) { 
	/* do something with the result */
}).catch(function() {
	/* error :( */
}).finally(function() {
   /* executes regardless or success for failure */ 
});
```
- If a function is expected to return a promise, but an async action may not be needed, it is possible to call `Promise.resolve()` or `Promise.reject()` without using `new` keyword. E.g:
```js
const cache = {};
const getUserDetail = (username) => {
  // In both cases, cached or not, a promise will be returned

  if (cache[username]) {
  	// Return a promise without the "new" keyword
    return Promise.resolve(cache[username]);
  }

  // Use the fetch API to get the information
  // fetch returns a promise
  return fetch('users/' + username + '.json')
    .then(function(result) {
      cache[username] = result;
      return result;
    })
    .catch(function() {
      throw new Error('Could not find user: ' + username);
    });
}

// getUserDetail then always returns a promise, so can chain .then methods
```

## then

- all promises have a `then` method which allows you to ract to the promise
- The `then` method callback receives the result given to it by the `resolve()` call
- `then` is chainable - each `then` receives the result of the previous `then` return value
```js
new Promise(function(resolve, reject) { 
	// A mock async action using setTimeout
	setTimeout(function() { resolve(10); }, 3000);
})
.then(function(num) { console.log('first then: ', num); return num * 2; })
.then(function(num) { console.log('second then: ', num); return num * 2; })
.then(function(num) { console.log('last then: ', num);});

// From the console:
// first then:  10
// second then:  20
// last then:  40
```

## catch

- `catch` callback is executed when the promise is rejected
- A frequent pattern is sending an `Error` to the `catch`

## finally

- `finally` callback is called regardless of success or failure

## Promise.all

- `Promise.all` method takes an array of promises and fires one callback once they are all resolved
- E.g. could fire off multiple `fetch` requests at once:
```js
const request1 = fetch('/users.json');
const request2 = fetch('/articles.json');

Promise.all([request1, request2])
.then(function(results) {
	// Both promises done!
});
```

## Promise.race

- `Promise.race` triggers as soon as any promise in the array is resolved or rejected, instead of waiting for all of them to be resolved/rejected
- A use case could be triggering requests to a primary and secondary souce, in case one of them are unavailable

# Async / Await

- Newest way to write asynchronous code in JavaScript
- `async` functions are syntactical sugar for `promises`, and the `async` and `await` keywords can help make asynchronous code read more like synchronous code
- `async` keyword is what lets the javascript engine know that you are declaring an asynchronous function. This is required to use `await` inside a function.
- when a function is declared with `async`, it automatically returns a promise
  - returning in an `async` function is the same as resolving a promise
  - throwing an error in an `async` function will reject the promise

```js
async function f() {
  return 1;
  // return Promise.resolve(1); would be the same
}

f().then(alert); // 1
```

## await

- `await` keyword tells javascript to wait until a promise settles (asynchronous action to finish) and returns its result
- Instead of calling `.then()` with promises, simply assign a variable to the result using `await`
```js
async function asyncFuncExample() {
  // only works inside async functions
  const result = await fetch("https://url.com/some/url");

  // wait 3 seconds
  await new Promise((resolve, reject) => setTimeout(resolve, 3000));

  return result;
}
```
- `await` accepts "thenable" objects

## Error Handling

- these are the same:
```js
async function f() {
  await Promise.reject(new Error("Whoops!"));
}
async function f() {
  throw new Error("Whoops!");
}
```

- Since async functions just return a promise, appending a `.catch()` method to the end of the called function can be used to handle errors
```js
const response = await f().catch(alert);
```
- `try/catch` blocks can also be used to handle errors directly inside the async function
- which one to use depends if the error should be handled by the user of the async function or not (if so it will break their promise chain)

## Example

Here are two pieces of code that do the same thing, one using promises and the other async/await:
```js
const img = document.querySelector('img');
fetch('https://url.com/someimgurljson')
  .then(function(response) {
    return response.json();
  })
  .then(function(response) {
    img.src = response.image.url;
  });
```
```js
const img = document.querySelector('img');
async function getImage() {
  const response = await fetch('https://url.com/someimgurljson');
  const imgData = await response.json();
  img.src = imgData.image.url;
}
getImage();
```

## Loops

Waiting for promises non concurrently (executing one after the other):
```js
for (let doc of docs) {
  await db.post(doc);
}
```
When we need to wait for multiple promises concurrently:
- use `for await..of`:
```js
for await (let doc of docs) {
  await db.post(doc);
}
```
- can also wrap promises in `Promise.all` and `await` the results:
```js
const results = await Promise.all([
  fetch(url1),
  fetch(url2),
  ...
]);
```

