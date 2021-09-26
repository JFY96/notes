# Prototypal inheritance

- In JavaScript, objects have a speical hidden property `[[Prototype]]` 
    - either `null` or references another `object` called "a prototype"
        - only these two types, others are ignored
    - if we read a property from an object and it is missing, JavaScript will take it from the prototype. This is called `prototypal inheritance`.
- `__proto__` is a getter and setter for `[[Prototype]]`
    - e.g. `rabbit.__proto__ = animal;`
        - "animal is the prototype of rabbit"
        - "rabbit prototypically inherits from animal"
```js
let animal = {
  eats: true,
  walk() {
    alert("Animal walk");
  }
};

let rabbit = {
  jumps: true,
  __proto__: animal
};

// walk is taken from the prototype
rabbit.walk(); // Animal walk

// but we can still assign its own walk method and it will not use the prototype:
rabbit.walk = function() {
  alert("Rabbit bounce");
};

rabbit.walk(); // Rabbit bounce
```
- Prototype chain can be as long as required
    - however the references cannot go in circles
- The prototype is only used for reading properties - writing/deleting properties will work directly with the object. Accessor properties are an exception as they are handled by a setter function
```js
let user = {
  name: "John",
  surname: "Smith",

  set fullName(value) {
    [this.name, this.surname] = value.split(" ");
  },

  get fullName() {
    return `${this.name} ${this.surname}`;
  }
};

let admin = {
  __proto__: user,
  isAdmin: true
};

alert(admin.fullName); // John Smith (*)

// setter triggers!
admin.fullName = "Alice Cooper"; // (**)

alert(admin.fullName); // Alice Cooper, state of admin modified
alert(user.fullName); // John Smith, state of user protected
```
- In a method call, `this` is always the object itself, no matter if the method is found in the object or its prototype
- The `for..in` loop iterates over inherited properties too
    - to exclude inherited properties, the built in method `obj.hasOwnProperty(key)` returns `true` if it has its own (not inherited) property `key`
        - note this method `Object.prototype.hasOwnProperty` is inherited too, but it is not `enumerable` so not listed in `for..in`
- Almost all other key/value-getting methods likje `Object.keys`, `Object.values` ignore inheritied properties

## F.Prototype

- if `F.prototype` is an object, then `new` operator uses it to set `[[Prototype]]` for the new object
```js
let animal = {
  eats: true
};

function Rabbit(name) {
  this.name = name;
}

Rabbit.prototype = animal;

let rabbit = new Rabbit("White Rabbit"); //  rabbit.__proto__ == animal
```
- `F.prototype` property is only used when `new F` is called, it assigns `[[Prototype]]` of the new object
    - if after creation, `F.property` changes, then new objects created by `new F` will use the new one but existing `F` objects keep the old one
- The default "prototype" for a function is an object with the only property `constructor` that points back to the function itself
```js
function Rabbit() {}
// by default: Rabbit.prototype = { constructor: Rabbit }
alert(Rabbit.prototype.constructor == Rabbit); // true
let rabbit = new Rabbit();
alert(rabbit.constructor == Rabbit); // true
```
- able to use `constructor` property to create a new object based on existing one:
```js
let rabbit2 = new rabbit.constructor();
```
- Note if we replace the default prototype as a whole, then there may be no `constructor` in it. To prevent overwriting it, we can choose to add/renmove properties to the default prototype:
```js
function Rabbit() {}
Rabbit.prototype.jumps = true;
// default Rabbit.prototype.constructor is preserved
```

## Native prototypes

- All built-in objects such as `Array`, `Function` store methods in prototypes
  - The object itself stores only the data
- By specification, all of the built-in prototypes have `Object.prototype` on the top. "everything inherits from objects"
```js
let arr = [1, 2, 3];
// an Array inherits from Array.prototype
alert( arr.__proto__ === Array.prototype ); // true
// then from Object.prototype
alert( arr.__proto__.__proto__ === Object.prototype ); // true
```
- Native prototypes can be modified (e.g. adding a method to `String.prototype` so it becomes available to all strings), but it is generally considered a bad idea as prototypes are global
- Two libraries could add the same method and there would be a conflict for example.
- The only case where this is approved in modern programming is **polyfilling**
  - Polyfilling is making a substitute for a method that exists in JavaScript Spec, but not yet supported by a particular engine
  - A method is implemented manually then the built-in prototype is populated with it
  ```js
  if (!String.prototype.repeat) { // no such method
    String.prototype.repeat = function(n) {} // polyfill
  }
  ```

## Primitives

- With strings, numbers, booleans, these are not objects, but they have built-in methods as: 
  - Temporary wrapper objects are created using built-in `String`, `Number`, `Boolean` to provide the methods then disappear after
  - These objects are created invisibly and most engines optimize them out
  - Methods of these objects are from prototypes `String.prototype`, `Number.prototype`, and `Boolean.prototype`
- `null` and `undefined` have no object wrappers or prototype, so no methods/propterties are available for them

## Modern methods / practices

`__proto__` is considered outdated / somewhat deprecated so these methods are recommended:

- `Object.create(proto, [descriptors])` - creates empty object with given `proto` as `[[Prototype]]`
- `Object.getPrototypeOf(obj)`
- `Object.setPrototypeOf(obj, proto)`

- `__proto__` getter/setter is unsafe if we want user-generated keys in an object as `__proto__` is possible as a key, which may cause an error. Instead:
  - use `Object.create(null)` to create an object without a prototype, which will have no issues with `__proto__` as the key
  - use a `Map`

- easy way to shallow-copy an object with all descriptors:
```js
let clone = Object.create(Object.getPrototypeOf(obj), Object.getOwnPropertyDescriptors(obj));
```

- If using constructors, best to define functions on the `prototype` of that object, so that a single instance of each function will be shared between all those objects. 
  - This is better for performance as the function will not be duplicated every time a new one of those objects are created.
- Don't change `[[Prototype]]` after object creation as this will break internal optimizations that JavaScript engines have, which will affect speed.

# Factory Functions

- In JavaScript, any function can return a new object. When it’s not a constructor function or class, it’s called a factory function.
- similar to contructors but without the need to use `new`
```js
const personFactory = (name, age) => {
  const sayHello = () => console.log('hello!');
  return { name, age, sayHello };
};

const jeff = personFactory('jeff', 27);
```

# Scope and Context

- Scope refers to *variable access*
- Context refers to `this`

## Scope

### var

- **Global/Root scope** - scope before you write any javascript in a new file. If a variable is declared, its defined globally (not inside a function)
  - Be careful / avoid using global scope directly unless required as there could be namespace classes
- **Local scope** refers to any scope defined past the global scope. 
  -Each function has its own nested local scope. 
  - Any function defined within another function has a local scope which is linked to the outer function
  - Any localy scoped items are not visible in the global scope
- `var` variables are **function scopeed** (only available inside the function created in)
  - they are not created for `for` or `while` loops, or statements like `if`, `switch`
- **Lexical Scope** - For functions inside another function, the inner function has access to the scope of the outer function
  - Any variables/objects/functions defined in its parent scope are available in the scope chain
  - Does not work backwards
- **Scope Chain** for a given function - the link between the scope of the functions inside that function
  - When resolving a variable/object/function, JavaScript starts at the innermost scope and searches outwards until it finds it
```js
// Scope A: Global scope
var myFunction = function () {
  // Scope B: Local scope
  var name = "Bob";
  var myOtherFunction = function () {
    // Scope C
    console.log(`name is ${name}`);
    var myFunctionInsideMyOtherFunction = function () {
      // Scope D
    };
  };
  myOtherFunction(); // "name is Bob"
};
console.log(name); // Uncaught ReferenceError: name is not defined
```

### let/const

- `let` and `const` are **block scoped**, meaning it will be scoped to the closest set of curly brackets `{}`

## Closures

- Anonymous closures are functions that wrap our code and create an enclosed scope around it
- A closure is what happens when you make a functions private variables (or embedded functions) available to outside code

- Example:
```js
const counterCreator = () => {
  let count = 0;
  return () => {
    console.log(count);
    count++;
  };
};

const counter = counterCreator();

counter(); // 0
counter(); // 1
counter(); // 2
counter(); // 3
```
  - `counterCreator` initializes a local variable `count` and then returns a function
  - To use the function, we have to assign a variable (`counter`)
  - Every time we run the function, it `console.log`s `count` and increments it
  - the function `counter` is a closure - it has access to the variable `count` and can print/increment it, but there is no other way for our program to access that variable

- Concept of closure: Functions retain their scope even if they are passed around and called outside of that scope
- For factory functions, closures allow us to create **private** variables and functions. Private functions are functions that are used in our objects but not intended to be used elsewhere.
  - So we split out functions up in the object and only export that functions that the rest of the program is going to use
  - e.g:
```js
const FactoryFunction = string => {
  const capitalizeString = () => string.toUpperCase();
  const printString = () => console.log(`----${capitalizeString()}----`);
  return { printString };
};
const taco = FactoryFunction('taco');
taco.capitalizeString(); // ERROR!!
taco.printString(); // this prints "----TACO----"
```

# Inheritance with factory functions

- Building on from the concept of prototypes and inheritance, there are a few ways to accomplish this using factory functions:
```js
const Person = (name) => {
  const sayName = () => console.log(`my name is ${name}`)
  return {sayName}
}

// This pattern allows you to pick and choose which functions to include in the new object
const Nerd = (name) => {
  // simply create a person and pull out the sayName function with destructuring assignment syntax!
  const {sayName} = Person(name)
  const doSomethingNerdy = () => console.log('nerd stuff')
  return {sayName, doSomethingNerdy}
}

// If you want everything from the other object, Object.assign
const Nerd = (name) => {
  const prototype = Person(name)
  const doSomethingNerdy = () => console.log('nerd stuff')
  return Object.assign({}, prototype, {doSomethingNerdy})
}
```

# IFFEs
- Immediately Invoked Function Expression
- An IFFE is when you construct a function expression (that contains some private variables/functions) and is executed immediately
```js
(function () {
    // logic here
})();
```
- Primary reason to use an IIFE is to obtain data privacy and avoid global variable polution
- Meaningful IIFEs usually create a closure but they don't have to

# Module Pattern

- One of the most common design patterns in JavaScript
- easy to use and creates encapsulation of our code
- Benefit to revealing module pattern is that we can look at the bottom of the module and quickly see what is publicly available to use

## Creating module and exporting module

- Module is created as an IFFE 
- assign the module to a variable that we can use to call module methods to "export"
```js
var myModule = (function() {
  'use strict';

})();
```
## Private methods and properties

- Using closures we can create private methods and private state:

```js
var myModule = (function() {
  'use strict';

  var _privateProperty = 'Hello World';
  var publicProperty = 'I am a public property';

  function _privateMethod() {
    console.log(_privateProperty);
  }

  function publicMethod() {
    _privateMethod();
  }

  return {
    publicMethod: publicMethod,
    publicProperty: publicProperty
  };
})();

myModule.publicMethod(); // outputs 'Hello World'
console.log(myModule.publicProperty); // outputs 'I am a public property'
console.log(myModule._privateProperty); // is undefined protected by the module closure
myModule._privateMethod(); // is TypeError protected by the module closure
```

- Because our private properties are not returned, they are not available outside of the module
- Our public method gives access to our private methods, allowing us to create private state and encapsulation 
- note `_` prefix is usually used for private methods/properties as JavaScript does not have private keyword for functions

# Namespacing

- A useful side effect of encapsulating our code of our programs into objects is namespacing
- Namespacing is a technique that is used to avoid naming collisions in our programs
- e.g. if we had `add` method multiple times, it wouldn't be a problem if all of them were nicely encapsulated inside of an object

## General rule of thumb regarding factories/modules

- if you only ever need ONE of something, use a module
- if you need multiple of something, create them with factories