# Language

- Weakly Typed Language
    - No explicit type assignment
    - Data types can be switched dynamically
- Object-Oriented Language
    - Data can be organized in logical objects
    - Primitive and reference types
- Versatile Language
    - Runs in browser & directly on a PC / Server
    - Can perform a broad variety of tasks
        - Browser: manipulate DOM, event listening etc
        - Node: Work with files, databases etc

# Variable Declaration

- `let` - modern variable declaration
- `const` - constants (`let` but variable cannot be changed after)
- `var` - old-school variable decalaration

## differences

- `var` has lexical scoping (function/globally) while `let` and `const` have block scoping
- While all are hoisted to the top of their scope, `var` is initialized with `undefined` while the others are not initialized
- `const` must be initialized during declaration

## Naming

- name can only contain alpha, numbers, `$` or `_`
- first character must not be a digit
- case matters
- keep in mind reserved names (e.g. `let`, `class`, `delete`, `yield` etc)

Recommended practices:
- use camelCase 
- uppercase contants for values difficult to remember that are known prior to execution (e.g. `const COLOR_RED = "#F00"`)

# Data types

Primitive types:
- `Number` (both integers and floating point numbers)
  - `NaN`, `Infinity` and `-Infinity` are also of this type
  - Range is `-(2⁵³-1)` to `(2⁵³-1)`
- `BigInt` - represent integers of arbitrary length (if `Number` is not large/small enough)
  - created by appending `n` to end of an integer e.g. `123123131231n`
- `String` - wrapped in `"` (Double quotes), `'` (Single quotes), or `\`` (Backticks)
- `Boolean` - `true` or `false`
- `null` - special value that represents "nothing", "empty", "unknown"
- `undefined` - special value that means "value is not assigned"

Non-primitive:
- `Object` - used to store collections of data and more complex entries
- `symbol` - used to create unique identifiers for objects

`typeof` operator can be used to check the data type of a variable
# Operators

## Arithmetic

- `+` addition
- `-` subtraction
- `*` multiplication
- `/` division
- `%` remainder (modulo)
- `**` exponent (e.g. `5 ** 2` is `25`)
- `++` increment, `--` decrement
- `+=`, `-=`, `*=`, `/=` addition/subtraction/multiplication/division assignment

Notes:
- Be careful with data types e.g. `"74" + 6` is `"746"` not `80` - use `Number()` to convert string to number
  - `+` is the only operator that supports strings in such a way. Other arithmetic operators work only with numbers so always convert their operands to numbers

## Unary Operators

- `-` negation e.g. `x = -x`
- `+` numeric conversion - converts non-numbers
  - equivalent to `Number()`
  - e.g. `+true` is `1`, `+""` is `0`
  - e.g. `const str1 = "2"; const str2 = "3"; const num = +str1 + +str2; // 5`

## Comparison Operators

- `==` equality
- `!=` non-equality
- `===` strict equality (no type conversion)
- `!==` strict non-equality
- `<` less than
- `>` greater than
- `<=` less than or equal to
- `>=` greater than or equal to

- When comparing values of different types, values are usually converted to numbers
- `0 == "0"` but `Boolean(0) != Boolean("0")` (false/true)
- `null == undefined` is a special rule, they dont `==` other "falsey" values
- when doing comparisions with `< > <= >=`, `null` becomes `0` and `undefined` becomes `NaN`
  - `null >= 0` is true but `null > 0` and `null == 0` are false
- `NaN` returns false for all comparisons e.g. `NaN == NaN` is false

## Bitwise

Bitwise operators treat arguments as 32-bit integer numbers, rather than 64-bit in other languages (may not be enough for some problems)

- `&` AND
- `|` OR
- `^` XOR
- `~` NOT
- `<<` LEFT SHIFT
- `>>` RIGHT SHIFT
- `>>>` ZERO-FILL RIGHT SHIFT

## Comma

rare operator `,` allows us to evaluate several expressions (seperated by comma), and the result of the last one is returned

e.g. `let a = (1 + 2, 3 + 4);` has a result of `7`

## Nullish coalescing operator (??)

-  logical operator that returns its right-hand side operand when its left-hand side operand is `null` or `undefined`, and otherwise returns its left-hand side operand
## Logical OR (||)

- Similar to Nullish coalescing operator but returns the right-hand operand if the left operand is any `falsy` value (e.g. `0` or `''`)

# Strings

- Strings are immutable: Strings cannot be changed, only replaced
- `\` to escape a character e.g. `'I\'ve'`
- special characters:
  - `\n`, `\r`, `\t` new line, carriage return, tab
  - `\'`, `\"`, `\\` quotes, backslash
  - `\xXX` hexadecimal unicode e.g. `\x7A` is `z`
  - `\uXXXX` unicode symbol with hex code XXXX
  - `\u{X...XXXXXX}` unicode symbol with given UTF-32 encoding (1-6 hex characters) e.g. `\u{1F60D}` is 😍
- concatenate strings using `+` (or `concat()` method)
- `length` property (not method) returns legnth of string
- `indexOf()`  and `lastIndexOf()` methods returns the index of first / last occurrence of specified text/character
- one of `slice`, `substring`, `substr` can be used to get a substring
- other common methods are `replace`, `toUpperCase`, `toLowerCase`, `trim`, `padStart`, `padEnd`, `charAt`, `charCodeAt`, `split`, `includes`, `startsWith`, `endsWith`, `repeat`
- Can use property access `[ ]` on strings
  - e.g. for `const str = "HELLO"`, `str[0]` is `H`
  - similar to `charAt`, but returns undefined if not found rather than empty string
  - read only, cannot do assignment 
- All strings are encoded using UTF-16 (each character has a corresponding numeric code)
  - `str.codePointAt(pos)` to get code for character at `pos` e.g. `"z".codePointAt(0)` is `122`
  - `String.fromCodePoint(code)` to get character from code e.g. `String.fromCodePoint(122)` is `"z"`



## Template literals

String literals allowing embedded expressions, using the backtick (``) instead of double or single quotes.

```js
const classes = `header ${ isReady ? 'green' : 'red'}`;
```

Tagged templates
```js
function myTag(strings, ...values) { // myTag(strings, name, gender)
    let str0 = strings[0]; // "Hi "
    let str1 = strings[1]; // ", how are you today "
    let str2 = strings[2]; // "?"

    let gender = values[1] || 'M';
    let honorific = (gender === 'M') ? 'Sir' : 'Madame';

    let name = values[0] || honorific;

    // does not have to return a string, could be function for example
    return `${str0}${name}${str1}${honorific}${str2}`; 
}
let string = myTag`Hi ${name}, how are you today ${gender}?`;
// same as myTag(["Hi ", ", how are you today ", "?"], name, gender)
```

Raw strings
```js
let str = String.raw`Answer is: \n${2+3}`; // "Answer is: \n5"
let str2 = `Answer is: \n${2+3}`;   // "Answer is: 
                                    //  5"
```
### String comparisons

- JavaScript uses Unicode order to compare characters
  - e.g. `"Z" > "A"` is true, `"a" > "A"` is true
- The strings are compared from the first character - the string with the greater first character is greater. If they are the same then compare the second character etc
  - e.g. `"Nom" < "Non"`

# Functions

- Programmers call `functions` that are part of objects `methods`
- `anonymous functions` are ones that have no name

## Scope

- Values passed to a function as parameters are copied to its local variables
- Variables and other things defined inside a function are inside their own separate `scope` (only visible inside that function)
- The top level outside all your functions is called the `global scope`
  - Global variables are visible from any function, unless shadowed by local one
  - It is a good practice to minimize the use of global variables

## Parameters

-  Function `Parameters` are sometimes called arguments, properties, or attributes
  - A `parameter` is the variable listed inside the parentheses in the function declaration (it’s a `declaration time term`)
  - An `argument` is the value that is passed to the function when it is called (it’s a `call time term`)
- "Default" values for a parameter can be declared using `=`

## Returning values

- A function without return or with an empty return will return `undefined`

## Naming functions

- Widespread practice to start a function name with a verbal prefix which vaguely describes the action e.g.
  - `"show…"` - show something
  - `"get…"` – return a value
  - `"calc…"` – calculate something
  - `"create…"` – create something
  - `"check…"` – check something and return a boolean, etc
- One function – one action
  - Two independent actions usually deserve two functions, even if they are usually called together (in which case we can make a third function that does that)
- Functions == Comments
  - for larger functions, it may be worth to split it into a few smaller ones
  - function names should be self-describing

## Default Values

- default argument is evaluated at call time - this applies to functions and objects
- earlier parameters are available to later default parameters
```js
function greet(name, greeting, message = greeting + ' ' + name) { return [name, greeting, message] }
```
- default parameter values exist in their own scope

## arguments object

- `arguments` is an Array-like object inside functions that contains values of the arguments passed
  - "Array-like" as it has `length` property and is indexed from zero, but does not have Arrays built in methods
```js
function func(a, b, c) {
  console.log(arguments[1]);
}
func(1,2,3); // prints 2
```
## rest parameters ()
*preferred over arguments object if writing ES6 compatible code*

- used to allow a function to accept an indefinite number of arguments as an array
- if a functions *last* parameter is prefixed with "`...`", all remaining paramters will be placed inside a standard JavaScript array (unlike arguments)
  - a function definition can only have one `...`restParameter and it must be last
```js
function sum(...args) {
  return args.reduce((accum, val) => accum + val);
}
```

## Declaration

`Function Declaration`:
```js
function sayHi() {
  alert( "Hello" );
}
```
`Function Expression`:
```js
let sayHi = function() {
  alert( "Hello" );
};
```
A difference between these is when they are created by the JavaScript engine
  - A `Function Expression` is created when the execution reaches it and is usable only from that moment.
  - A `Function Declaration` can be called earlier than it is defined (JavaScript will first look through the script for global function delcarations and create the functions - "initialization stage")
Another special feature of `Function Declarations` is their `block scope`
  - In strict mode, when a `Function Declaration` is within a code block, it’s visible everywhere inside that block. But not outside of it.
  ```js
  if (age < 18) {
    welcome();               // \   (runs)
                            //  |
    function welcome() {     //  |
      alert("Hello!");       //  |  Function Declaration is available
    }                        //  |  everywhere in the block where it's declared
                            //  |
    welcome();               // /   (runs)
  }
  welcome(); // Error: welcome is not defined
  ```
  - In such use case, `Function Expression` should be used

## Arrow functions

- very simple and concise syntax for creating functions, that’s often better than `Function Expressions` (can be used in the same way as Function Expressions)
```js
let func = (arg1, arg2, ..., argN) => expression
```
`func` takes arguments `arg1...argN`, evaluates `expression` and returns its result

- If we only have one argument, parentheses around parameters can be omited `arg => expression`
- If there are no arguments, parentheses will be empty `() => expression`
- Multiline arrow functions - if we need something more complex, the right side should be exclosed in curly braces, then use a normal `return` within them.
```js
let func = (arg1, arg2, ..., argN) => {
  // code
  return result;
}
```

## First Class Functions

## Higher order functions
- map, filter, reduce

# this keyword
- bind, call, apply
- arrow notation



# Document Object Model (DOM)

# Browser Object Model (BOM)

# Event Loop

- macrotasks
- microtasks

# JavaScript Engine
- Declartion
- Initialization

# Scoping
- Lexical scoping - var
- Block scoping - const/let

# Hoisting
var, function (not anomalous) 

# Global Object
- window (browser)
- global (node)

# Types
- Type Casting
- Coercion

# Objects
- used to store keyed collections of various data and more complex entities
- created with `{}` and an optional list of properties. A property is a `key: value` pair, `key` a string and `value` anything.

- by Reference
    - Variable stores pointer
- Object mutation
## Notation

```js
// creating empty object
let user = new Object(); // "object constructor" syntax
let user2 = {}; // "object literal" syntax
```
```js
// object initializer (comma-delimited list of zero or more pairs of property name and values)
let user = {
  name: "John",  // key can be identifier
  "user age": 30,     // or a string
  1: false,      // or a number (becomes a string when used as property key)
};
// note the trialing comma in the last property - this is not required but makes it easier when adding/moving properties around

// access values using dot notation
console.log(user.name);
// or square brackets (use for multiword properties, or numeric, or if the name is in a variable)
console.log(user["user age"]);
let propName = 1;
console.log(user[propName]);

// remove a property using delete
delete user.name;

// accessing a property that does not exist returns undefined
console.log(user.gender); // undefined
```
```js
// Shorthand property names (ES2015)
let a = 'foo', b = 42, c = {};
let o = {a, b, c} // {a: a, b: b, c: c}

// Shorthand method names (ES2015)
let o = {
  property(parameters) {} // property: function (parameters) {},
}

// Computed property names (ES2015)
let prop = 'foo';
let o = {
  [prop]: 'hey',
  ['b' + 'ar']: 'there'
}
```

## in operator

- used to check if object has a property with name specified
```js
let user = { age: 30, name: undefined };
console.log("age" in user); // true
console.log("name" in user); // true
console.log("gender" in user); // false
```
- to loop over all keys of an object, use the `for..in` loop
```js
for (let key in user) {
  console.log(key); // age, name
  console.log(user[key]); // 30, undefined
}
```

## object property order

- integer properties are sorted, and others appear in creation order after the integer properties
  - integer properties are those that can be converted to-and-from an integer without change
```js
let codes = {
  "+49": "Germany",
  "+41": "Switzerland",
  86:  "China",
  "44": "Great Britain",
  "+1": "USA",
};

for (let code in codes) {
  console.log( +code ); // 44, 86, 49, 41, 1
}
```
## Immutability
- E.g. Don't change object directly, create a new object with changes. Less likely for bugs this way.

## Copying an array/object
- array: .slice()
- object: Object.assign()
- Shallow copy, deep copy
- Spread Operator
```js
const copy = [...arr];
```
- Rest Operator
```js
const toArray = (...args) => {
    return args;
}
```

## Object Destructuring

```js
const person = {
    name: 'Jin',
    age: 24
};

const {name, age} = person;
console.log(name); // 'Jin'

const printData = ({ name, age }) => {
    console.log(name, age);
}

const food = ['Pho', 'Ramen'];
const [food1, food2] = food;
console.log(food1); // 'Pho'
```

## Getters and Setters

- `get` syntax binds an object property to a function that is called when that property is looked up 
- `set` syntax binds an object property to a function that is called when there is an attempt to set that property

```js
let o = { 
  property: function (parameters) {},
  get property() {},
  set property(value) {}
};
```

```js
const language = {
  set current(name) {
    this.log.push(name);
  },
  get latest() {
    if (this.log.length === 0) return undefined;
    return this.log[this.log.length - 1];
  },
  log: []
};

console.log(language.latest); // undefined

language.current = 'EN';
language.current = 'FA';

console.log(language.latest); // "FA"
console.log(language.log); // Array ["EN", "FA"]
```

# Arrays

- creating empty array:
```js
let arr = new Array();
let arr = [];
```
- to create an array with a given length: 
```js
let arr = new Array(2); // length 2
  ```
- Getting, replacing, adding elements to array:
```js
let fruits = ["Apple", "Orange", "Plum"];
console.log(fruits[0]); // Apple
fruits[2] = "Pear" // now ["Apple", "Orange", "Pear"]
fruits[3] = "Lemon" // now ["Apple", "Orange", "Pear", "Lemon"]
```
- Arrays can store elements of any type
- Arrays in JavaScript can work both as a queue and as a stack (this type of data structure is called deque, or double ended queue)
  - Queue (First-In-First-Out)
    - `pop` - removes last element and returns it
    - `push` appends element to end of array
  - Stack (Last-In-First-Out)
    - `shift` - removes first element and returns it
    - `unshift` - adds element to start of array

- `toString` implementation for arrays returns a comma-separated list of elements
```js
console.log(String([1, 2, 3])); // "1,2,3"
console.log([1, 2] + 1) // "1,21"
```


## Internal Representation
- Array is actually a special kind of `object` (basic data type) so behaves like an object
  - e.g. it is copied by reference
- The JavaScript engine will try to store elements of arrays in a contiguous memory area, one after another, along with other optimizations, to make them work really quick. However, if we quit working with arrays as an "ordered collection", and instead like a regular object, those optimizations will be turned off. 
- Ways to misuse an array:
  - Add a non-numeric property e.g. `arr.test = 5`
  - make holes, e.g. `arr[0]` then `arr[1000]` (and nothing in between)
  - Fill the array in reverse order (e.g. `arr[1000]` then `arr[999]`)
- Arrays should be thought about as special structures to work with *ordered data*. Arrays are carefully tuned inside JavaScript engines to work with contiguous ordered data.

## Performance

- `push`/`pop` run quick, while `shift`/`unshift` run slow
  - this is because all elements must be shifted to the left/right for `shift`/`unshift`
    - the more elements in the array, the more in-memory operations it takes
  - `push`/`pop` do not need to move anything as other elements keep their indexes, so these actions are very quick

## Looping

- standard for loop: `for (let i = 0, n = arr.length; i < n; i++) {}`
- `for..of` loops:  `for (let element of arr) {}`
- Since arrays are objects, it is possible to use `for..in` loops, but it is generally a bad idea as:
  - `for..in` iterates over all properties, not only numeric ones - e.g.  `length` 
    - there are other "array-like" objects which may have other non numeric properties and methods
  - `for..in` loop is optimized for generic objects and not arrays so it would be slower

## length

- use `length` property to get total count of elements in array
- `length` actually returns the greatest numeric index plus one, so if adding a single element with a large index (shouldn't use arrays like that), then the length wouldn't be the total number of elements
- `length` property is writable
  - simplest way to clear an array is `arr.length = 0`
```js
let arr = [1, 2, 3, 4, 5];
arr.length = 2; // truncate to 2 elements
console.log(arr); // [1, 2]
arr.length = 5; // return length back
console.log(arr[3]); // undefined: the values do not return
```

# Enum

# Loops

- `while` and `do...while` loops: `while (condition) {}`, `do {} while (condition);`
- `for` loops: `for (begin; condition; step) {}`
- e.g.
```js
for (let i = 0; i < 3; i++) { // i is called an "inline" variable - ony visible inside the loop
  console.log(i); // 0, 1, 2
}
```
- `break` forces an exit of the loop
- `continue` stops the current iteration and forces the loop to start the next one (if condition allows)
- note do not use `break` and `continue` with the ternary operator `?`
- `break`/`continue` support labels before the loop in order to escape a nested loop to go to an outer one. A label is an identifier with a colon before a loop.
```js
outer: for (let i = 0; i < 3; i++) {
  for (let j = 0; j < 3; j++) {
    let input = getInput();
    // if invalid input then break out of both loops
    if (!input) break outer; // (*)
    // ... other code
  }
}
// (*) goes to here
```

# Optional Chaining Operator

- provides a way to simplify accessing values through connected objects when it is possible that a reference or function may be undefined or null

these are equivalent:
```js
let nestedProp = obj.first?.second;
```
```js
let temp = obj.first;
let nestedProp = ((temp === null || temp === undefined) ? undefined : temp.second);
```

```js
obj.val?.prop
obj.val?.[expr]
obj.arr?.[index]
obj.func?.(args) // attempt to call function which may not exist
```

# Debugging

https://nodejs.org/en/docs/guides/debugging-getting-started/

https://code.visualstudio.com/docs/nodejs/nodejs-debugging

https://developer.chrome.com/docs/devtools/javascript/

# Console

- `console.log()`
- `console.time("labelName");` - starts timer
- `console.timeEnd("labelName");` - stops timer and logs elapsed time in ms
- `console.trace()` - outputs a stack trace
- `console.table()` - displays tabular data as a table

## String Substitutions

- for console methods that accept a string (e.g. `log()`), these substitution strings may be used:
    - `%o` or `%O` - object
    - `%s` - string
    - `%d` or `%i` - integer
        - Number formatting supported 
            - `console.log("Value %.2d", 1.1)` outputs `Value 01` (2 significant figures)
    - `%f` - floating point value
        - Number formatting supported
            - `console.log("Value %.2f", 1.1)` outputs `Value 1.10` (2 decimal places)

## Styling console output

- can apply CSS styles using the `%c` directive
    - can be used multiple times, and the text before the directive will not be affected
```js
console.log("This is %cMy stylish message%c And just a basic orange message", "color: yellow; font-style: italic; background-color: blue;padding: 2px", "color: orange", "with some more regular text on the end");
```

# Errors

## Common Errors

- Syntax Error
- Reference Error
- Type Error

# JSON

JSON (JavaScript Object Notation) is a standardized format for structuring data, which is heavily based on the syntax for JS objects.
- built-in JavaScript methods `JSON.parse()` and `JSON.stringify()` are the most often used when dealing with JSON