# Test Driven Development

- Main idea is simply that you start working on your code by writing automated tests *before* writing the code that is being tested
- TDD means less bugs, higher quality software, and a razor focus, but it can slow development down and the test suites can be tedious to manage and maintain. However it is recommended as the pros outweigh the cons:
- Popular test-running systems available in JavaScript are Mocha, Jasmine, Tape, Jest

## Why write tests before the code

- To prevent bias and slack on the tests if the code is written first
- The test becomes the first client/user of that code (the first client of that API) 

## Benefits

- Reduces **bugs** in new features and existing features
- Reduces the **cost of change**
    - Refactoring is easy as the tests will make sure your new code is working
- Improves **productivity** by
    - minimizing time spent **debugging**
    - reduces need for **manual (monkey) checking** by developers and tester
    - helping developers maintain **focus**
    - reduces **wastage**: hand overs
- Improves **communication**
    - The tests double as specific executable documentation on how the feature/function should work
- Creating living, up-to-date specification
- Communicate **design decisions** - improves design
- **Learning**: listen to your code
- **Baby steps**: slow down and think
- Improves **confidence** / reduces **fear**
- **Testable** code by **design** + safety net against other programmers
- **Loosely-coupled** design
- Encourages **Refactoring**


# Writing Tests

## Approaches to take when writing test first

- Small steps
    - First write some basic tests
    - Write function that satisfy your tests
    - Run Tests
    - Add to / build on tests
    - Refactor function to satisfy your tests
    - Run Tests
    - Repeat until done
    - "red green red green red green"

- Triangulation
- Obvious implementation

## Isolation

- Test one method at a time
- tests for one function should not depend upon an external function behaving correctly (esp if that function is being tested elsewhere)
- Reason is when your tests fail, you want to be able to narrow down the cause of failure as quickly as possible
    - If test that depends on several functions, it can be hard to pinpoint the issue

## Unit tests

- Unit tests exist to test individual units of software functionality
- A unit is a module, component, or function
- Narrow focus down
- Split up complexity by having tests check single specific cases
    - When multiple things break/dont work as intended, certain test cases that fail make it easier to understand the problem
- Unit tests should be:
    - Thorough
    - Stable
    - Fast
    - Few (none unnecessary)
- Focus on messages

## Tightly Coupled Code

- Tightly coupled code is hard to test, which makes one of biggest benefits to TDD being it helps you write better code
- TDD encourages better program architecture because it encourages you to write *pure functions*
- Tightly coupled code has many causes:
    - Mutation vs immutability
    - Side-effects vs purity/isolated side-effects
    - Responsibility overload vs Do One Thing (DOT)
    - Procedural instructions vs describing structure
    - Class Inheritance vs composition

### Pure Functions

- Pure functions:
    - Given the same input, always return the same output
    - Produce no side-effects
- Pure functions reduce coupling by:
    - **Immutability**: Pure functions don't mutate existing values. They return new ones instead.
    - **No side effects** : No interference with the operation of other functions that may be observing external state such as screen, DOM, console, standard out, network, disk
    - **Do one thing**: Map some input to some corresponding output, avoiding responsibility overload that tends to plague object/class-based code
    - **Structure, not instructiongs**: Pure functions can be safely memoized (so if the system had infinite memory, any pure function could be replaced with a lookup table). Pure functions describe structural relationships between data, not instructions for the computer to follow.

### Dealing with problematic coupling

- **Use pure functions** as the atomic unit of composition, instead of classes, imperative procedures, or mutating functions
- **Isolate side-effects** from the rest of program logic. So don't mix logic with I/O (network, rendering UI, logging etc)
    - Don't unit test I/O - use integration tests instead (fine to mocking for this)
- **Remove dependent logic** from imperative compositions so that they can become declarative compositions which don't need their own unit tests. If there's no logic then there is nothing meaningful to test

## Integration tests

## Mocking (for integration tests)

- Mock functions allow you to test links between code by erasing the actual implementation of a function, instead capturing calls to the function (and parameters passed in those calls)
- writing "fake" versions of a function that always behaves exactly how you want
    - e.g. if testing a function that retrieves information from a DOM input, just create a fake version of the input-grabbing function that always returns a specific value and use that in your test
- E.g. Tests should be quick so many network fetch requests / asynchronous actions may not be a good idea
    - With mocks we can add a fake "fetch" function to pass to the function being tested
        - Called **Dependency Injection**

# Jest

Installing
```
npm i jest --save-dev
```

Useful scripts in `package.json`:
```json
"test": "jest",
"watch": "jest --watch *.js"
```

Basic example:
```js
test('double 1 is 2', () => {
    expect(double(1)).toBe(2);
});
```

## Matchers

`expect` returns an "expectation" object, which we can call matchers on them:
- `toBe` tests exact equality
- `toEqual` recursively checks every field of an object
- If you need to distinguish between falsy values or match on truthiness:
    - `toBeNull` matches only `null`
    - `toBeUndefined` matches only `undefined`
    - `toBeDefined` opposite of `toBeUndefined`
    - `toBeTruthy`
    - `toBeFalsy`
- Number comparisons:
    - `toBeGreaterThan`, `toBeGreaterThanOrEqual`, `toBeLessThan`, `toBeLessThanOrEqual`
    - `toBeCloseTo` for floating point equality
- Regular expressions for strings - `toMatch` e.g. `toMatch(/i/)`
- `toContain` - check array/iterable contains an item
- `toThrow` to check if function throws an error
- `not` e.g. `not.toBe` can be used to check the opposite of a matcher
- `toBeCalledWith` - 

## "TODO" tests

- `test` without a second parameter (so just the label)
    - these are skipped

## Async

- If your test involves async code / promises, either `return` a promise or use `done` parameter

## Globals

- These methods and objects are added in the global environment by Jest so do not need to require/import to use them:
    - `test` or `it`
    - `test.only` - only run this test (good for when a test is failing so we focus on that one only until fixed)

## Repeating setup

`beforeEach` and `afterEach` can be used if there is work that needs to be done repeatedly for many tests
```js
beforeEach(() => {
  initializeTable();
});

afterEach(() => {
  clearTable();
});
```
- These also support async code like tests
- Similarly, `beforeAll` and `afterAll` if setup only needs to be done once, at the beginning of a file
- These usually apply to every test in a file but when inside a `describe` block, they only apply within that block
    - Note top-level ones will still be executed for tests inside the `describe` block
- Jest executes all describe handlers in a test file before it executes any of the actual tests

## Mock Functions

- Two ways to mock functions
    - create a mock function to use in test code
    - write a `manual mock` to override a module dependency

```js

```

