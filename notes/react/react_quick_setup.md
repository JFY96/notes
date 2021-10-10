# Quick setup for a new react project

```bash
npm init
```

Create `.gitignore` and ignore these folders
```
node_modules
build
```

npm modules:

```bash
npm install react react-dom
```

```bash
npm install @babel/core @babel/preset-env @babel/preset-react
```

```bash
npm install webpack webpack-cli webpack-dev-server babel-loader css-loader style-loader html-webpack-plugin
```

Babel config in own config file:
```json
{
    "presets": [
        "@babel/preset-env",
        "@babel/preset-react"
    ]
}
```
or `package.json`:
```json
"babel": {
    "presets": [
        "@babel/preset-env",
        "@babel/preset-react"
    ]
}
```

Create `webpack.config.js` at root path:
```js
const path = require('path');
const HtmlWebPackPlugin = require('html-webpack-plugin');

module.exports = {
	entry: './src/index.js',
	output: {
		filename: 'main.js',
		path: path.resolve(__dirname, 'build'),
	},
	resolve: {
		modules: [path.join(__dirname, 'src'), 'node_modules'],
		alias: {
		  react: path.join(__dirname, 'node_modules', 'react'),
		},
	},
	module: {
		rules: [
			{
				test: /\.?js$/,
				exclude: /node_modules/,
				use: {
					loader: "babel-loader",
					options: {
						presets: ['@babel/preset-env', '@babel/preset-react']
					},
				}
			},
			{
				test: /\.s[ac]ss$/i,
				use: [
					// Creates `style` nodes from JS strings
					"style-loader",
					// Translates CSS into CommonJS
					"css-loader",
					// Compiles Sass to CSS
					"sass-loader",
				],
			},
		]
	},
	plugins: [
		new HtmlWebPackPlugin({
		  template: './src/index.html',
		}),
	]
};
```

Add scripts to `package.json`:

```json
"scripts": {
	"start": "webpack-dev-server --mode=development --open --hot",
	"build": "webpack --mode=production"
}
```

Add `index.html` in `src` folder (Standard html file with `<div id="root"></div>` in the body)

Add `app.js` in `src` folder:
```js
import React from 'react';

const App = () => {
  return (
      <h1>App</h1>
  );
};

export default App;
```

Add `index.js` file  in `src` folder:

```js
import React from 'react';
import { render } from 'react-dom';
import App from './App';

render(
  <React.StrictMode>
    <App />
  </React.StrictMode>,
  document.getElementById('root')
);
```

Now try the command `npm start` to test it

