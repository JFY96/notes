# Linting

- Linters are tools that scan your code with a set of rules to report any errors they find
- Reasons why to use linting:
    - It prevents certain types of bugs
    - Code styling is important
        - A consistent set of rules makes it more maintainable and easier to read
    - Finds syntax errors before execution
        - Saves you time


## ESLint

- Open source, JavaScript linting utility

- Installation:
```
npm install eslint --save-dev
```
then set up a configuration file (given `package.json` already exists):
```
npx eslint --init
```
as part of the setup, it will ask a few questions about the project, after which the `.eslintrc.js` file should look something like this:
```js
module.exports = {
  env: {
    browser: true,
    es2021: true,
  },
  extends: ["airbnb-base"],
  parserOptions: {
    ecmaVersion: 12,
    sourceType: "module",
  },
  rules: {},
};
```
- `airbnb style` is one of the most popular JavaScript style guides

## Prettier

- Similar to a linter, but has a different purpose
- Takes JS code and automatically format it according to a set of rules
    - Specifically targetting layout of code like spaces, indentation levels and line-breaks
- Installation with ESLint:
```
npm install --save-dev --save-exact prettier
npm install --save-dev eslint-config-prettier
```
- Then adding to `.eslintrc.js`:
```js
module.exports = {
  env: {
    browser: true,
    es2021: true,
  },
  extends: ["airbnb-base", "prettier"],
  parserOptions: {
    ecmaVersion: 12,
    sourceType: "module",
  },
  plugins: ["prettier"],
  rules: {
    "prettier/prettier": "error",
  },
};
```

### With React

- Note there may be problems with using `react` style guide and `prettier` when setting up ESLint.
    - Both of them will try format and lint your code
- if using `eslint-config-airbnb`, since that has rules and guidelines for React, just remove the `react` plugin from `.eslintrc.js` (so the setup should look like the above)

## VSCode tips

- Install prettier and ESLint extensions
- If you want it to automatically format and fix all fixable errors in your code, in `setting.json` add these lines:
```json
"editor.formatOnSave": true,
"editor.defaultFormatter": "esbenp.prettier-vscode",
"editor.codeActionsOnSave": {
    "source.fixAll.eslint": true
}
```
- or without prettier, just eslint:
```json
"editor.codeActionsOnSave": {
  "source.fixAll.eslint": true
},
"eslint.validate": [
  "javascript"
]
```
- In `package.json` add a lint and format script:
```json
"scripts": {
    "lint": "eslint --fix main.js",
    "format": "prettier -w ."
}
```

