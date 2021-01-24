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

# ECMAScript

# Document Object Model (DOM)

# Browser Object Model (BOM)

# Event Loop

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
- by Reference
    - Variable stores pointer
- Object mutation

## Immutability
- E.g. Don't change object directly, create a new object with changes. Less likely for bugs this way.

## Copying an array/object
- array: .slice()
- object: Object.assign()
- Shallow copy, deep copy
- Spread Operator
```
const copy = [...arr];
```
- Rest Operator
```
const toArray = (...args) => {
    return args;
}
```

## Destructuring

Object Destructuring

```
const person = {
    name: 'Jin Fei',
    age: 24
};

const {name, age} = person;
console.log(name); // 'Jin Fei'

const printData = ({ name, age }) => {
    console.log(name, age);
}

const food = ['Pho', 'Ramen'];
const [food1, food2] = food;
console.log(food1); // 'Pho'
```

# Prototypes
- Protypal Inheritence

# Enum

# this keyword
- bind, call, apply
- arrow notation

# Closures & IFFEs
Immediately Invoked Function Expressions

# First Class Functions

# Higher order functions
- map, filter, reduce

# Synchronous
JavaScript is a Single-Threaded language

# Asynchronous JavaScript
- Execution Stack
- Browser APIs
- Function Queue
- Event Loop

# Callbacks

# Promises
```
const promise = new Promise((resolve, reject) => {});
```

# Async / Await

# Classses

# Template literals

String literals allowing embedded expressions, using the backtick (``) instead of double or single quotes.

```
const classes = `header ${ isReady ? 'green' : 'red'}`;
```

Tagged templates
```
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
```
let str = String.raw`Answer is: \n${2+3}`; // "Answer is: \n5"
let str2 = `Answer is: \n${2+3}`;   // "Answer is: 
                                    //  5"
```


# Debugging

https://nodejs.org/en/docs/guides/debugging-getting-started/

https://code.visualstudio.com/docs/nodejs/nodejs-debugging