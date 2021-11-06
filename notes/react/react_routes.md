# Router

- For applications with multiple pages, there should be a reliable routing system
- The package `react-router-dom` can be used for this purpose

## Basic usage

`Routes.js`:
```js
import React from 'react';
import { BrowserRouter, Switch, Route } from 'react-router-dom';
import App from './App';
import About from './About';

const Routes = () => {
  return (
    <BrowserRouter>
      <Switch>
        <Route path="/" component={App} />
        <Route path="/about" component={About} />
      </Switch>
    </BrowserRouter>
  );
};

export default Routes;
```

- `Route` - our routes with a path, which equals the url path, and a component that should be rendered when we navigate to this url
- `BrowserRouter` - router which keeps your UI in sync with the URL
- `Switch` - renders the first child route that matches the location, and all others will be ignored.
	- Note `"/"` as well as `"/about"` contain at first a `/` in the path, so going to `"/about"` will show the `App` render as it is the first path athat matches the url
		- To solve, either change the order your routes, or use the `exact` keyword
			- the `exact` keyword specifies that the routes path has to match the URL path exactly, as opposite to finding the first string of characters that matches the URL path
			```js
			<Route exact path="/" component={App} />
			<Route exact path="/profile" component={Profile} />
			```
		- without a `Switch` it will render everything that matches the URL path

- Also remember to use `Routes` in `index.js` rather than your `App`

## Adding a nav

`Routes.js`:
```js
import React from 'react';
import { BrowserRouter, Switch, Route } from 'react-router-dom';
import Nav from './Nav';
import App from './App';
import About from './About';

const Routes = () => {
  return (
    <BrowserRouter>
	 	<Nav />
      <Switch>
        <Route path="/" component={App} />
        <Route path="/about" component={About} />
      </Switch>
    </BrowserRouter>
  );
};

export default Routes;
```

- `<Nav />` will be rendered on every page since it does not limited by any `Route`

`Nav.js`:

```js
import React from 'react';
import { Link } from 'react-router-dom';

const Nav = () => {
  return (
		<nav>
			<h3>Logo</h3>
			<ul className="nav-links">
				<Link to="/">
					<li>Home</li>
				</Link>
				<Link to="/about">
					<li>About</li>
				</Link>
			</ul>
		</nav>
  );
};

export default Nav;
```

- `Link` - anchors for your routes - use `<Link to="">` instead of `<a href="">`


## Using with Webpack

add to `webpack.config.js`:
```js
module.exports = {
	output: {
		publicPath: '/'
	},
	devServer: {
		historyApiFallback: true,
	},
}
```
# Other Sources/Links

https://css-tricks.com/learning-react-router/