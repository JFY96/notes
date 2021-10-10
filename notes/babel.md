# Babel

- JavaScript compiler to convert newer ECMAScript code into a backwards compatible version
- Features:
    - Transform syntax
    - Polyfill missing features in target environment
    - Source code transformations/mods

# Using a preset for a single part of project (No compiling of node_modules)

- e.g. for use with `Jest`

```
npm i -D @babel/preset-env
```

Add this to `package.json` (or create `.babelrc` file)
```json
"babel": {
    "presets": ["@babel/preset-env"]
}
```
