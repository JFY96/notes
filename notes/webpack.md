# Webpack

- Tool for bundling modules
- combines many JavaScript files into a single bundle file

## features 

- Webpack can minify and ugligy your code
- Loading Images - Webpack can manage images and compress/optimize them for use on the web
- Loading CSS - supporting `sass`, `less` etc, using `style-loader` and `css-loader`
- Loading Data - e.g. `JSON`, similar to nodejs, `import Data from './data.json` will work by default using webpack

## Basic setup / example

- After creating your project and `npm init -y`:
```
npm install webpack webpack-cli --save-dev
```
- create folder `src` for source code with `index.js` as **entry** point (or if something else the entry point can be changed in `webpack.config.js` file)
- create configuration file `webpack.config.js` in same root directory as `package.json` (not required but recommended as alternative is typing long commands in terminal)
    - this file can also be generated using `webpack init`
```js
const path = require('path');

module.exports = {
  entry: './src/index.js',
  output: {
    filename: 'main.js',
    path: path.resolve(__dirname, 'dist'),
  },
};
```
- add script to `package.json`:
```
"scripts": {
    "build": "webpack"
},
```
then `npm run build` command can be used, equivalent to `npx webpack`.
- These can also be used (production will minify, development does not):
```
"scripts": {
    "dev": "webpack --mode development",
    "build": "webpack --mode production"
}
```
- The above setup will take all dependencies from `./src/index.js`, bundle all the code, and **output** into `./dist/main.js`
    - the `src` folder will be where we write all of the code that webpack will bundle up for us
    - when webpack runs, it will look for any `import` statements and compiles all of that into that single file

## Webpack with Babel

```
npm install @babel/core babel-loader
```

Add this to `webpack.config.js`:
```js
module: {
	rules: [
		{
			test: /\.?js$/,
			exclude: /node_modules/,
			use: {
				loader: "babel-loader",
			}
		},
	]
},
```

may need predefined configs (presets) of what to transpile:
```
npm install @babel/preset-env @babel/preset-react
```
which will require changes to `webpack.config.js`
```js
use: {
	loader: "babel-loader",
	options: {
		presets: ['@babel/preset-env', '@babel/preset-react']
	}
}
```

## With CSS Loader

```
npm install css-loader
```

Add to `webpack.config.js`:
```js
module.exports = {
  module: {
    rules: [
      {
        test: /\.css$/i,
        use: ["style-loader", "css-loader"],
      },
    ],
  },
};
```

### with sass Loader

```
npm install sass sass-loader
```


Add to `webpack.config.js`:
```js
module.exports = {
  module: {
    rules: [
      {
			test: /\.(s[ac]ss)$/i,
			use: [
				// Creates `style` nodes from JS strings
				"style-loader",
				// Translates CSS into CommonJS
				"css-loader",
				// Compiles Sass to CSS
				"sass-loader",
			],
      },
    ],
  },
};
```

## with TypeScript

```
npm install --save-dev typescript
```

if using webpack (else install `ts-loader`):
```
npm install @babel/preset-typescript
```

`@babel/preset-typescript` should replace `@babel/preset-env`.

configure `tsconfig.json`.

add to `webpack.config.js`:
```js
{
	resolve: {
	extensions: ['.ts', '.tsx', '.js']
	},
	module: {
		rules: [
			{
				test: /\.(tsx|ts|jsx|js)?$/,
				exclude: /node_modules/,
				// use: 'ts-loader',
				use: {
					loader: "babel-loader",
					options: {
						presets: ['@babel/preset-typescript', '@babel/preset-react']
					},
				}
			}
		]
	}
}
```

- remember to replace `.js` and `.jsx` files with `.ts` and `.tsx` files (also in `package.json`, `webpack.config.js` etc)
- when using third party libraries, it is important to install the typing definition for that library. e.g. `npm install --save-dev @types/lodash`

# Environment Variables

`package.json`:
```json
{
	"scripts": {
		"build-dev": "webpack --mode development --env BUILD_ENV=dev"
	}
}
```

`webpack.config.js`:
```js
const webpack = require('webpack');

module.exports = (env) => {
	console.log(env.BUILD_ENV);
	return {
		...
		plugins: [
			new webpack.DefinePlugin({
				'process.env.BUILD_ENV': JSON.stringify(env.BUILD_ENV || 'production')
			}),
		]
	}
}
```

## using dotenv

```
npm install dotenv-webpack --save-dev
```

create file `.env` and others env files such as `.env.development`:
```
BUILD_ENV=production
```

update `webpack.config.js`:
```js
const Dotenv = require('dotenv-webpack');

module.exports = (env) => {
	console.log(env.BUILD_ENV);
	return {
		...
		plugins: [
			new Dotenv({
				path: `.env${env.ENVIRONMENT ? `.${env.ENVIRONMENT}` : ''}`
			}),
		]
	}
}
```

update `package.json` scripts:
```json
{
	"scripts": {
		"build-dev": "webpack --mode development --env ENVIRONMENT=development"
	}
}
```

Then on build, webpack will replace `process.env` variables with the values in the `env` file.

# Hot Module Replacement (HMR)

Feature to inject updated modules while an application is running without a full reload (can be used as LiveReload replacement). For most purposes, `webpack-dev-server` is a good fit to get started with HMR quickly
