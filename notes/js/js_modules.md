# Modules

## History

- `AMD` (Asynchronous module definition) - initially implemented by library `require.js`
- `CommonJS` - module system created for `Node.js` server
- `UMD` (Universal module definition) - suggested as universal to replace (compatible with) AMD and CommonJS

## Modern JS Modules

- One script is one module
- Modules load each other using `export` and `import`
- `export` keyword labels functions and variables that can be accessed outside of the current module
- `import` includes the functionality from other modules
- Modules have their own local top-level scope
- Modules always `use strict`
- Module code is executed only once - exports are created once and shared between imports
- Browsers need `<script type="module">` to make import/export work. These scripts are deferred by default.
- Bundlers such as Webpack are often used to bundle modules together for performance/other reasons