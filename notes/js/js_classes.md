# Classes

- JavaScript does not have classes in the same sense as other OOP languages like Java, but ES6 introduced the `class` keyword
- It is a new syntax that does that same thing as object constructors

## Example

- basic syntax:
```js
class User {
    constructor(name) {
        this.name = name;
    }

    logName() {
        console.log(this.name);
    }
}
```
- when `new User("John")` is called
    - A new object is created
    - the `constructor` runs with given argument(s)
- what `class User {...}` actually does:
    - Creates a function named `User` with the function code taken from the `constructor`
    - Stores class methods in `User.prototype`

```js
// class is a function
alert(typeof User); // function

// ...or, more precisely, the constructor method
alert(User === User.prototype.constructor); // true

// The methods are in User.prototype, e.g:
alert(User.prototype.logName);
```

## More than syntactic sugar

There are a few differences between classes and writing them using pure functions:

- a function created by `class` is labelled by `[[IsClassConstructor]]: true`, a special internal property
    - this property is checked in a few places meaning a few differences
        - class constructor must be called with `new` (not doing this for function version does not cause an error)
        - String representation in most engines start with "class" (i.e for alert/console.log)
- class methods are non-enumerable (class definition sets `enumurable` flag to `false` for all `prototype` methods)
- Classes always `use strict`

## Getters/Setters

- Classes may include getters/setters 
- example:
```js
class User {

  constructor(name) {
    // invokes the setter
    this.name = name;
  }

  get name() {
    return this._name;
  }

  set name(value) {
    if (value.length < 4) {
      alert("Name is too short.");
      return;
    }
    this._name = value;
  }
}
```

## Computed names [...]

- e.g. computed method name:
```js
class User {
    ["log" + "Name"]() {
    }
}
```

## Class fields

- classes allow us to add any properties
```js
class User {
    name = "John";
}
```
- these are set on individual objects, not the `prototype`

## this

- note if an object method is passed around and called in another context, `this` may not be a reference to its object anymore. 
- An elegant syntax to solve this is to use arrow functions for methods inside the class

## Inheritance

