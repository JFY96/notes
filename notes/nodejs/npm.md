# NPM - Node Package Manager

- standard package manager for Node.js
- Command line tool that gives access to a gigantic repository of plugins, libraries, and tools
# npm scripts

## npm init

Creates and congiures a package.json file (Configuration file for project)

## npm start 

Shortcut for `npm run start`
Will run the "start" property of "scripts" object in the package. If property is not specified then it wil run `node server.js`

# Installing/Managing third party packages

`npm install SomePackageName` or `npm i SomePackageName`

`-g` - `-global` installs the package globally on the machine instead of locally (`--save-dev` or `--save`)

`--save-dev` will differentiate the installed package to appear in your `devDependencies`, instead of the default `dependencies` (production - default)

``--no-save`` installs but does not add the entry to `package.json` dependencies

`npm install` will install/update all dependencies in package.json

`npm update` will check and update all packages

`npm update <package-name>` can be used to update a single package

# package.json

This file:
- lists the packages your project depends on (dependencies)
- specifies the versions of a package that your project can use
- makes your build reproducible, so easier to share 

## fields

- `name` - required - lowercase and one word
- `version` - required - in the form `x.x.x` (see [semantic versioning](#semantic-versioning))
- `author` - optional - `Your Name <email@example.com> (http://example.com)`
- `description`
- `dependencies` and `devDependencies` list npm packages installed as dependencies
    - "^" character in version numbers means it wil use a later version if available when doing `npm install`
- `scripts`- defines a set of node scripts you can run
- `main` - entry point for the application
- `private` - if true prevents the app/package to be published on npm
- `engines` sets which versions of Node.js this package/app works on
- `browserslist` - tell which browsers (& versions) to support

## package-lock.json

Stores the exact version of dependencies that were installed, in order to get consistent installs across machines
# semantic versioning

- everytime significant updates are made, a new version number is recommended

- 1.0.0 - First release
- 1.0.1 - Patch release (Backwards compatible, bug fixes)
- 1.1.0 - Minor release (Backwards compatible, new features)
- 2.0.0 - Major release (Changes that break backwards compatibility)

# npx
```
npm install -g npx
```
- npx is a npm package runner
- Able to execute a package that wasn't previously installed (or a locally installed package)
