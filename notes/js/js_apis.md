# APIs

- `Application Programming Interfaces` are servers that are created for serving data for external (or internal) use
- APIs are typically accessed through URLs
- In most cases, you will have to have an account and request an "API key" from the API service before attempting to fetch data from their endpoints
    - This key will usually have to be included with every data request

# Fetching Data

- In older JS versions, requests were made using an `XMLHttpRequest`, but it is not particularly nice to use
- 3rd party libraries were made to take care of this, such as `axios` and `superagent`
- Web browsers have began to implement a new native function for making HTTP requests - `fetch`

## fetch

Example code:
```js
// URL (required), options (optional)
fetch('https://url.com/some/url')
  .then(function(response) {
    // Successful response :)
  })
  .catch(function(err) {
    // Error :(
  });
```

- `fetch` returns a promise with the response from the request
- note the promise **won’t reject on HTTP error status**
- the response is just an HTTP response, not the actual JSON - use `Response.json()` method

Example to POST JSON-encoded data:
```js
fetch('https://example.com/profile', {
  method: 'POST', // or 'PUT'
  headers: {
    'Content-Type': 'application/json',
  },
  body: JSON.stringify(data),
})
.then(response => response.json())
.then(data => {
  console.log('Success:', data);
})
.catch((error) => {
  console.error('Error:', error);
});
```

## CORS

- `CORS` is Cross-Origin Resource Sharing
- For security reasons, browsers restrict HTTP requests to outside sources, but if required, the fix is simple for `fetch`:
```js
fetch('url.url.com/api', {
  mode: 'cors'
});
```