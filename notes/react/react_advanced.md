# PropTypes

```
npm install --save prop-types
```

PropTypes provides the possibility to declare types. It is a package that does runtime type checking for React props.

## Usage

```js
import React from 'react';
import PropTypes from 'prop-types';

const MyComponent = (props) => {
// react component here
};

MyComponent.propTypes = { // notice the lowerCamelCase here
	propertyName: PropTypes.string, // declare property with a type
}
```

# Styled Components

https://styled-components.com/docs

# Higher-order Components

Higher order components are components that consume another component and return a third component.

https://reactjs.org/docs/higher-order-components.html

# withRouter (history, match, children object)

An example of a higher-order component. By wrapping `withRouter` around one of your components, the component will get access to your history, match, and children objects, which provide some additional syntactic sugar. E.g. could use the history object to direct a user from one route to another

https://reactrouter.com/core/api/withRouter

# custom hooks (building your own)

https://reactjs.org/docs/hooks-custom.html