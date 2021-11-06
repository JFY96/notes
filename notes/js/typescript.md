# Introduction

- TypeScript is an open source language and is a superset of JavaScript
- It offers additional features to JavaScript
	- Main feature is the additional layer on top: TypeScript's type system
- Using types is completely *optional*
- Can be used for front-end JS as  well as backend with Node.js
- It is a strongly typed language which builds on JavaScript. TS code compiles down to regular JS which will run in any environment that JS runs in.
- The main benefit is that it can highlight unexpected behaviour in your code, with a tighter integration with your IDE. More errors will be caught early meaning the chance of bugs is also lower.

From typescriptlang.org:
```
The goal of TypeScript is to be a static typechecker for JavaScript programs - in other words, a tool that runs before your code runs (static) and ensures that the types of the program are correct (typechecked).
```

# Dynamic vs Static typing

In *dynamically typed languages*, the types are associated with run-time values and not named explicitly in your code (majority of its type checking is performed at run-time as opposed to at compile-time).

In statically typed languages, you explicitly assing types to variables, function parameters, return values, etc.

JavaScript only truly provides *dynamic* typing. A *static* type system describes the shapes and behaviours of what our values will be when we run our programs. A type-checker like TypeScript uses that information and tells us when things might be going off the rails.

# Pros & Cons

Pros
- More Robust
- Easily spot bugs
- Predictability
- Readability
- Popular

Cons
- More code to write
- More to learn
- Required compilation
- Not true static typing

# Compiling TypeScript

- TypeScript uses `.ts` and `.tsx` (corresponds to `jsx`) extensions
- `TSC` (TypeScript Compiler) is used to compile `.ts` files down to JS
- Can watch files and report errors at compile time
- Many tools include TS compilation by default
- Most IDEs have great support for TS
- `tsconfig.json` file is used to configre how TypeScript works

# Tooling

TypeScript can be leveraged for editing code - the core type-checker can provide error messages and code completion as you type in the editor.

An editor that supports TypeScript can deliver "quick fixes" to automatically fix errors, refactorings to re-organize code, and useful navigation features to jump to variable definitions, or find all references to a given variable.

# TypeScript Compiler

Install the TypeScript compiler `tsc` globally:
```bash
npm install -g typescript
```

Check version:
```bash
tsc -v
```

For a TypeScript file `hello.ts`, running this will *compile* it into a plain `js` file:
```bash
tsc hello.ts
```
It will output `hello.js` unless there are errors which will be shown on the command line.

## Compile all

This will compile all `.ts` files it can find in the project:
```bash
tsc
```

## Watch files

To start watch mode for a file `hello.ts` (automatically compile on file changes):
```bash
tsc --watch hello
```

or for all files:
```bash
tsc --watch
```

# Config File

Create `tsconfig.json` file:
```bash
tsc --init
```

Here you can configure compiler options such as:
- `target` - ECMAScript version to compile to
	- By default when compiiing TS, TypeScript targets ES5. This is called *downleveling*.
- `outDir` - output directory e.g. `"./dist"`
- `rootDir` - root directory of input files e.g. `"./src"`

## noEmitOnError

If you are migrating JS code over to TS and there are type-checking errors (but the original JS code was working), until cleaning up the code for the type-checker, you can use `noEmitOnError` flag:
```bash
tsc --noEmitOnError hello.ts
```
or set this in `tsconfig.json`

## Strictness

TypeScript has several type-checking strictness flags that can be turned on or off. The `strict` flag in the CLI, or `"strict": true` in a `tsconfig.json` toggles them all on simultaneously, but we can opt out of them individually.

Two important ones are:
- `noImplicitAny` - turning it on will issue an error on any variables whose type is implicitly inferred as `any`
- `strictNullChecks` makes handling `null` and `undefined` more explicit, and spares us from worrying about whether we forgot to handle `null` and `undefined`

# Type annotations on Variables

Add a type annotation to explicity specify the type of a variable when declaring:
```ts
const myName: string = "Alice";
```

Wherever possible, TypeScript tries to automatically infer the types in your code, so in most cases, this isn't needed.

For example, the type of a variable is inferred based on the type of its initializer.

# Common Types

Primitives
- `string`
- `number`
- `boolean`

`any`
- special type that you can use whenever you don't want a particular value to cause typechecking errors
- When a value is of type `any`, you can access any properties of it (which will in turn be of type `any`), call it like a function, assign it to (or from) a value of any type, or pretty much anything else that’s syntactically legal

Arrays
- `number[]`, `string[]`, `any[]` etc for array of numbers, strings
- `Array<number>` etc means the same thing

Object Types
```ts
const user: { id: number, name: string } = {
	id: 1,
	name: 'Jin',
	age?: number, // optional
};

// or using a type alias

type User = {
	id: number,
	name: string,
};

const user: User = {
	id: 1,
	name: 'Jin',
};
```

Union types
- formed from two or more types, representing values that may be *any one* of those types
```ts
let pid: string | number;
pid = '22'; // ok
pid = 22; // ok
pid = {id: 22} // error
```
- TypeScript will only allow you to do things with the union if that thing is valid for every member of the union (e.g. cannot use `string` specific methods in above example as it can also be a `number`)
	- The solution is to *narrow* the union with code i.e. using `typeof x === 'string'` or `Array.isArray(x)` etc
	- if every member of the union has a common method, then it can be used as normal

Tuple
```ts
let person: [number, string, boolean] = [1, 'Jin', true];
```

Tuple Array
```ts
let employee: [number, string][];

employee = [
	[1, 'Tom'],
	[2, 'John],
];
```

Enum
- set of named constants
```ts
enum Direction1 {
	Up,
	Down,
	Left,
	Right,
}
console.log(Direction1.Up); // 0
console.log(Direction1.Down); // 1
enum Direction2 {
	Up = 1,
	Down,
	Left,
	Right,
}
console.log(Direction1.Down); // 2
enum Direction1 {
	Up = 'Up',
	Down = 'Down',
	Left = 'Left',
	Right = 'Right',
}
```

# Functions basics

TypeScript allows you to specify the types of both the input and output values of functions

```ts
function greet(person: string, date: Date) {
  console.log(`Hello ${person}, today is ${date.toDateString()}!`);
}
```

`person` and `date` have *type annotations* added to describe what types of values the function should be called with.

Return type annotations appear after the parameter list:
```ts
function addNum(x: number, y: number): number {
  return x + y;
}
```
Usually these are not needed as TS will infer the functions return type based on its `return` statements. Some codebases will explicitly specify a return type for documentation purposes, to prevent accidental changes, or just for personal preference.

`void` type should be used if not returning a value:
```ts
function log(message: string | number): void {
	console.log(message);
}
```

Anonymous functions - When a function appears in a place where TypeScript can determine how it’s going to be called, the parameters of that function are automatically given types. 
```ts
const names = ["Alice", "Bob", "Eve"];
// Contextual typing for function
names.forEach(function (s) {
  console.log(s.toUppercase());
// Property 'toUppercase' does not exist on type 'string'. Did you mean 'toUpperCase'?
});
```

# Type Aliases

- A name for a type so it can be reused
- can be used to any primitive types, a union of types, or object type
```ts
type Point = {
	x: number;
	y: number;
};

type ID = number | string;
```

# Interfaces

- An *interface declaration* is another way to name an object type
```ts
interface Point {
  x: number;
  y: number;
}
```

```ts
interface UserInterface {
	readonly id: number, // readonly property
	name: string,
	age?: number, // optional
};

const user: UserInterface = {
	id: 1,
	name: 'Jin',
};

interface MathFunc {
	(x: number, y: number): number
}

const add: MathFunc = (x: number, y: number): number => x + y;
const sub: MathFunc = (x: number, y: number): number => x - y;
```

## Interfaces vs Type Aliases

The key distinction is a type cannot be re-opened to add new properties vs an interface which is always extendable

```ts
// Extending an interface
interface Animal {
  name: string
}

interface Bear extends Animal {
  honey: boolean
}

// Extending a type via intersections
type Animal = {
  name: string
}

type Bear = Animal & { 
  honey: boolean 
}

// Adding new fields to an existing interface
interface Window {
  title: string
}

interface Window {
  ts: TypeScriptAPI
}

// A type cannot be changed after being created
type Window = {
  title: string
}

type Window = {
  ts: TypeScriptAPI
}
// Error: Duplicate identifier 'Window'
```

Other differences:
- Type aliases may not participate in *declaration merging*, but interfaces can
- Interfaces may only be used to declare shapes of objects, not rename primitives
- Interfaces will always be named in error messages. Type aliases names may appear in error messages (proior to TS v4.2)
- Interface names will always appear in their original form in error messages, but only when they are used by name

# Type Assertions

*type assertion* can be used to specify a more specific type:

```ts
let cid: any = 1;

let customerId = <number>cid;
// or
let customerId = cid as number;
```

It is useful for a type of a value that TypeScript can't know about:
```ts
const myCanvas = <HTMLCanvasElement>document.getElementById("main_canvas");
```

Note TypeScript only allows type assertions which convert to a more specific or less specific version of a type, so e.g. a conversion from a string to number directly is not possible. If something like this is required then first convert to type `any` then to the desired type:
```ts
const a = (expr as any) as T;
```

# Literal Types

By themselves, literal types aren’t very valuable:

```ts
let x: "hello" = "hello";
// OK
x = "hello";
// Error
x = "howdy";
```

But by combining literals into unions, it becomes a much more useful concept:
```ts
function printText(s: string, alignment: "left" | "right" | "center") {
  // ...
}

function compare(a: string, b: string): -1 | 0 | 1 {
  return a === b ? 0 : a > b ? 1 : -1;
}

interface Options {
  width: number;
}
function configure(x: Options | "auto") {
  // ...
}
configure({ width: 100 });
configure("auto");
```

boolean literals - the type `boolean` itself is actually just an alias for the union `true | false`

## Literal Inference

When you initialize a variable with an object, TypeScript assumes that the properties of that object might change values later. This means variables are generally inferred with general types `number`, `string` etc.

At times this can cause an error:
```ts
const req = { url: "https://example.com", method: "GET" };
handleRequest(req.url, req.method);
// error Argument of type 'string' is not assignable to parameter of type '"GET" | "POST"'.
```
`handleRequest` expects `"GET" | "POST"` but `req.method` is inferred to be a `string`, not `"GET"`. Ways to work around this:
- change the inference by adding a type assertion in either location:
```ts
// Change 1:
const req = { url: "https://example.com", method: "GET" as "GET" };
// Change 2
handleRequest(req.url, req.method as "GET");
```
- use `as const` to convert the entire object to be type literals:
```ts
const req = { url: "https://example.com", method: "GET" } as const;
```
The as `const` suffix acts like `const` but for the type system, ensuring that all properties are assigned the literal type instead of a more general version like `string` or `number`.

# null and undefined

## with strictNullChecks off

Values that might be `null` or `undefined` can still be accessed normally, and the values `null` and `undefined` can be assigned to a property of any type.

This is similar to how languages without null checks (e.g. C#, Java) behave. 

The lack of checking for these values tends to be a major source of bugs so it is always recommend to turn strictNullChecks on if it’s practical to do.

## with strictNullChecks on

When a value is `null` or `undefined`, you will need to test for those values before using methods or properties on that value.

Just like checking for undefined before using an optional property, we can use *narrowing* to check for values that might be null:

```ts
function doSomething(x: string | null) {
  if (x === null) {
    // do nothing
  } else {
    console.log("Hello, " + x.toUpperCase());
  }
}
```

## Non-null Assertion Operator

TypeScript has a special syntax for removing `null` and `undefined` from a type without doing any explicit checking.
Writing `!` after any expression is effectively a type assertion that the value isn’t `null` or `undefined`:

```ts
function liveDangerously(x?: number | null) {
  // No error
  console.log(x!.toFixed());
}
```

Note like other type assertions, this doesn’t change the runtime behavior of your code, so it’s important to only use `!` when you know that the value can’t be `null` or `undefined`.

# Narrowing

A special form of code called a *type guard* is a special check like `typeof padding === "number"`.

The process of refining types to more specific types than declared is called *narrowing*.

`typeof` type guards:
- `'string'`
- `'number'`
- `'bigint'`
- `'boolean'`
- `'symbol'`
- `'undefined'`
- `'object'`
- `'function'`

## Truthiness narrowing

TypeScript can also use truthiness checking to narrow types:

```ts
function multiplyAll(
  values: number[] | undefined,
  factor: number
): number[] | undefined {
  if (!values) {
    return values;
  } else {
    return values.map((x) => x * factor);
  }
}
```

## Equality narrowing

TS can use `switch` statements and equality checks like `===`, `!==`, `==`, `!=` to narrow types:

```ts
function example(x: string | number, y: string | boolean) {
	if (x === y) {
		// x and y must be 'string's here
	}
}

function printAll(strs: string | string[] | null) {
	if (strs !== null) {
		if (typeof strs === "object") {
			for (const s of strs) console.log(s);
		} else if (typeof strs === "string") {
			console.log(strs);
		}
	}
}

// works with looser equality checks
function multiplyValue(value: number | null | undefined, factor: number) {
	// Remove both 'null' and 'undefined' from the type.
	if (value != null) {
		// Now we can safely multiply 'value'.
		value *= factor;
	}
	return value;
}
```

## in operator narrowing

`in` operator is an operator for determining if an object has a property with a name - TS takes this into account.

```ts
type Fish = { swim: () => void };
type Bird = { fly: () => void };
 
function move(animal: Fish | Bird) {
	if ("swim" in animal) {
		return animal.swim();
	}
	return animal.fly();
}
```

## instanceof narrowing

`instanceof` is an operator for checking whether or not a value is an “instance” of another value, and it is also a type guard for TS:

```ts
function logValue(x: Date | string) {
  if (x instanceof Date) {
    console.log(x.toUTCString());
  } else {
    console.log(x.toUpperCase());
  }
}
```

## Type Predicates

For more direct control over how types change throughout your code, you can define a user-defined type guard. To do so, define a function whose return type is a *type predicate*.

A predicate takes the form `parameterName is Type`, where `parameterName` must be the name of a parameter from the current function signature.

```ts
function isFish(pet: Fish | Bird): pet is Fish {
  return (pet as Fish).swim !== undefined;
}

if (isFish(pet)) {
  pet.swim();
}
```
Any time `isFish` is called with some variable, TypeScript will *narrow* that variable to that specific type if the original type is compatible. `pet is Fish` is the type predicate in this example. 

The type guard can also be used to filter an array:
```ts
const zoo: (Fish | Bird)[] = [getSmallPet(), getSmallPet(), getSmallPet()];
const underWater1: Fish[] = zoo.filter(isFish);
// or, equivalently
const underWater2: Fish[] = zoo.filter(isFish) as Fish[];
 
// The predicate may need repeating for more complex examples
const underWater3: Fish[] = zoo.filter((pet): pet is Fish => {
	if (pet.name === "sharkey") return false;
	return isFish(pet);
});
```
## never type

When narrowing, you can reduce the number of options until you have removed all possibilities. In those cases, TS uses a `never` type to represent a state that should not exist.

The `never` type is assignable to every type; however, no type is assignable to `never`. This means you can use narrowing and rely on `never` turning up to do exhaustive checking in a switch statement.

```ts
type Shape = Circle | Square | Triangle;
 
function getArea(shape: Shape) {
	switch (shape.kind) {
		case "circle":
			return Math.PI * shape.radius ** 2;
		case "square":
			return shape.sideLength ** 2;
		default:
			const _exhaustiveCheck: never = shape;
			// Error - Type 'Triangle' is not assignable to type 'never'.
			return _exhaustiveCheck;
	}
}
```

# Functions
```ts
// Function Type Expressions
function greeter(fn: (a: string) => void) {
  fn("Hello, World");
}
 
function printToConsole(s: string) {
  console.log(s);
}
greeter(printToConsole);

// or with a type alias:
type GreetFunction = (a: string) => void;
function greeter(fn: GreetFunction) {
  // ...
}

// Call signatures - if function properties are needed
type DescribableFunction = {
  description: string;
  (someArg: number): boolean;
};

// Construct signatures - when a function needs to be invoked with the new operator (usually creates a new object)
type SomeConstructor = {
  new (s: string): SomeObject;
};
function fn(ctor: SomeConstructor) {
  return new ctor("hello");
}
```

## Generic functions

Generics are used to describe a correspondence between two values. This can be done by eclaring a *type parameter* in the function signature:

```ts
function firstElement<Type>(arr: Type[]): Type | undefined {
  return arr[0];
}

function map<Input, Output>(arr: Input[], func: (arg: Input) => Output): Output[] {
  return arr.map(func);
}

// Constraint to limit kind of types the type parameter can accept
function longest<Type extends { length: number }>(a: Type, b: Type) {
  if (a.length >= b.length) {
    return a;
  } else {
    return b;
  }
} // note the return value is inferred by TS

// another example
function combine<Type>(arr1: Type[], arr2: Type[]): Type[] {
  return arr1.concat(arr2);
}
// this would give an error as Type 'string' is not assignable to type 'number'.
const arr = combine([1, 2, 3], ["hello"]);
// but you could manually specify type:
const arr = combine<string | number>([1, 2, 3], ["hello"]);
```

### Writing good generic functions

- Push Type Parameters Down
	- When possible, use the type parameter itself rather than constraining it
- Use Fewer Type Parameters
	- Always use as few type parameters as possible
- Type Parameters Should Appear Twice
	- If a type parameter only appears in one location, strongly reconsider if you actually need it

## Optional Parameters

Marking a parameter as optional with `?`, or provide a parameter default:
```ts
function f(x?: number) { // x will have type number | undefined
  // ...
}

function f(x = 10) { // x will have type number because any undefined argument will be replaced with 10
  // ...
}
```

## Function Overloads

We can specify a function that can be called in different ways by writing *overload signatures*:
```ts
function makeDate(timestamp: number): Date;
function makeDate(m: number, d: number, y: number): Date;
function makeDate(mOrTimestamp: number, d?: number, y?: number): Date {
  if (d !== undefined && y !== undefined) {
    return new Date(y, mOrTimestamp, d);
  } else {
    return new Date(mOrTimestamp);
  }
}
```

These first two signatures are called the *overload signatures*. The third is a function implementation with a compatible signature, called *implementation signature*, but this signature can’t be called directly.

## Other types

these can be used everywhere, but these are especially relevant in the context of functions:
- `void` - the return value of functions which don’t return a value. 
	- Note `void` is not the same as `undefined` (`undefined` is returned implicitly if a function does not reutn any value).
	- function type with a `void` return type can return *any* other value, but it will be ignored
- `object` - refers to any value that isn't a primitive
	- different from *empty object type* `{ }` and global type `Object`
- `unknown` - represents any value. Similar to `any` but safer because it's not legal to do anything with an `unknown` value
	- Useful when describing function types that accept any value without having `any` values in function body
- `never` - represents values which are *never* observed. In a return type, this means that the function throws and exception or terminates execution of the program
	- also appears when TS determines theres nothing left in a union
- `Function` - global type that describes properties like `bind`, `call`, `apply` and others present on all function values in JS. Values of type `Function` can always be called, and these calls return `any`.

## Rest parameters

A rest parameter appears after all other parameters, and uses the `...` syntax:

```ts
function multiply(n: number, ...m: number[]) {
  return m.map((x) => n * x);
}
```

The type annotation given must be of the form `Array<T>` or `T[]`, or a tuple type.

## Parameter destructuring

```ts
function sum({ a, b, c }: { a: number; b: number; c: number }) {
  console.log(a + b + c);
}
```

Note that there is currently no way to place type annotations within destructuring patterns since syntax already means something different in JavaScript.

# Object types

## Property Modifiers

- Optional properties - add `?` to end of names
- `readonly` properties - add `readonly` before name. A `readonly` property cannot be written to during type-checking
	- note the property itself can't be re-written to, but its not totally immutable (objects etc)
- Index Signatures - if name of a property is not known ahead of time, but the shape is known:
```ts
interface StringArray {
  [index: number]: string;
}
```

## Extending Types

The `extends` keyword on an `interface` allows us to effectively copy members from other named types, and add whatever new members we want. `interface`s can also extend from multiple types.

```ts
interface Colorful {
  color: string;
}
 
interface Circle {
  radius: number;
}
 
interface ColorfulCircle extends Colorful, Circle {}
 
const cc: ColorfulCircle = {
  color: "red",
  radius: 42,
};
```

## Intersecion types

*Intersection types* can be used to combine existing object types. is defined using the `&` operator.

```ts
interface Colorful {
  color: string;
}
interface Circle {
  radius: number;
}
 
type ColorfulCircle = Colorful & Circle;
```

## Interfaces vs. Intersections

The principle difference between the two is how conflicts are handled, and that difference is typically one of the main reasons why you’d pick one over the other.

## Generic Object Types

```ts
// generic Box type which declares a type parameter
interface Box<Type> {
  contents: Type;
}

let box: Box<string>;

function setContents<Type>(box: Box<Type>, newContents: Type) {
  box.contents = newContents;
}

// with type alias:
type Box<Type> = {
  contents: Type;
};

type OrNull<Type> = Type | null;
 
type OneOrMany<Type> = Type | Type[];

type OneOrManyOrNull<Type> = OrNull<OneOrMany<Type>>;

type OneOrManyOrNullStrings = OneOrManyOrNull<string>;
```

`Array` itself is a generic type. Other data structures like `Map<K, V>`, `Set<T>`, `Promise<T>` are also generic.

`ReadonlyArray` is a special type that describes arrays that shouldn't be changed.

A *tuple type* is another sort of `Array` type that knows exactly how many elements it contains, and exactly which types.
```ts
type StringNumberPair = [string, number];

// with optional property
type Either2dOr3d = [number, number, number?];

// with rest elements
type StringNumberBooleans = [string, number, ...boolean[]];
type StringBooleansNumber = [string, ...boolean[], number];
type BooleansStringNumber = [...boolean[], string, number];

function readButtonInput(...args: [string, number, ...boolean[]]) {
  const [name, version, ...input] = args;
  // ...
}
// equivalent to
function readButtonInput(name: string, version: number, ...input: boolean[]) {
  // ...
}

// readonly
function doSomething(pair: readonly [string, number]) {
  // ...
}
```

# Classes

```ts
interface PersonInterface {
	id: number,
	name: string,
	register(): string,
};

class Person implements PersonInterface {
	private id: number
	name: string

	constructor(id: number, name: string) {
		this.id = id;
		this.name = name;
	}

	register() {
		return `${this.name} is now registered`;
	}
}

const jin = new Person(1, 'Jin');

// Subclass
class Employee extends Person {
	position: string

	constructor(id: number, name: string, position: string) {
		super(id, name);
		this.position = position;
	}
}

const emp = new Employee(2, 'Shawn', 'Boss');
```

# Generics

```ts
function getArray<T>(items: T[]): T[] {
	return new Array().concat(items);
}

let numArray = getArray<number>([1,2,3,4]);
let strArray = getArray<string>(['Jin', 'Shawn', 'Tom']);
```

# TypeScript with React

When using TypeScript with React, `PropTypes` won't need to be used.

```ts
export interface Props {
	title: string
	color?: string
}

const Header = (props: Props) => {
	return (
		<header>
			<h1 style{{color: props.color ?? 'blue' }}>{props.title}</h1>
		</header>
	);
}
```