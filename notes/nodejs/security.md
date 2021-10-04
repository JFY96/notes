# Security Configuration

Sensitive information such as `express.sessions` secret, mongoDB uri, and any API keys you might use should stay hidden. Details such as these should never be published or pushed to a git repo.

npm packages `dotenv` or `nconf` can be used to hide secrets. 

## dotenv

Module that loads environment variables from a `.env` file into `process.env`.

To use it, simply create a file called `.env` in your project directory and fill it with variables that represent things you need to keep secret. In `.env`, add environment-specific variables on new lines in the form of `NAME=VALUE`. For example `DB_HOST=localhost`.

Make sure to add it to `.gitignore`.

To use it in your application:
```js
const dotenv_result = require('dotenv').config();
if (dotenv_result.error) throw dotenv_result.error;
console.log(dotenv_result.parsed);
```
`config()` will read your `.env` file, parse the contents, assign it to `process.env`.

## nconf

A more robust option as it allows you to define configuration files in multople ways for ultimate flexibility. E.g. have a `config.js` file to keep all your secrets, but also add the ability to override one with a command line argument.

This can be especially useful when app configuration needs to a little more involved. It makes it easy to configure things such as separate production and development databases, logging and debugging options or anything else.