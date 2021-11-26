# Redux

Redux is a pattern and library for managing and updating application state by using "actions" (events).

It serves as a centralized store used across the entire application, with rules in place to ensure any state updates are predictable.

## Advantages

- Redux patterns and tools make it easier to understand when, where, why, and how the application state is begin updated, and how your application logic will behave when state changes occur.
- Guides you towards writing predictable and testable code, providing confidence

Redux is more useful for larger or more complex applications, especially when
- large amounts of application state are needed in many places
- app state is updated frequently
- logic for updating is complex
- codebase is large, or is worked on by many people

## Ideas

In a typical react application, if components need to share and use the same state, this is done by "lifting state up" to parent components. However, this may not always help in complex applications.

Redux solves this by separating the concepts involved in state management and enforcing rules that maintain independence betwen views and states, giving the code more structure and maintainability.

It provides a single centralized place to contain the global state in your application, and specific patterns to follow when updating that state to make the code predictable.

# Redux concepts

## Immutability



# Redux Toolkit

Redux Toolkit package was originally created to help address common concerns about Redux:
- Too much boilerplate code
- Configuring a store is complex
- Many packages required

```bash
npm install @reduxjs/toolkit
```

## Included APIs

- `configureStore()`
- `createReducer()`
- `createAction()`
- `createSlice()`
- `createAsyncThunk`
- `createEntityAdapter`
- `createSelector`

## RTK Query

Optional addon used for data fetching and caching. It allows you to define an API interface layer for your app, to simplify common cases for data loading by handling fetching and caching logic for you.

```js
import { createApi } from '@reduxjs/toolkit/query'

/* React-specific entry point that automatically generates hooks */
import { createApi } from '@reduxjs/toolkit/query/react'
```

APIs included:
- `createAPI()`
- `fetchBaseQuery()`
- `<ApiProvider />`
- `setupListeners()`

# React Redux

Official React UI bindings layer for Redux. It lets react components read data from a Redux store, and dispatch actions to the store to update state.

If you are using Redux and React together, you should use React Redux to bind these two libraries.

```bash
npm install react-redux
```

## Advantages

- Designed and intended for use with React, and maintained directly by the Redux team
- React Redux implements many performance optimizations internally, so that your own component only re-renders when it actually needs to

## Included APIs

- `<Provider />` component which makes the Redux store available to the rest of your app
- Custom React Hooks that allow your React components to interact with the Redux store.
	- `useSelector` - reads a value from the store state and subscribes to updates
	- `useDispatch` - returns the stores `dispatch` method to let you dispatch actions

## TypeScript

React Redux is written in TypeScript, so TS type definitions are built in. If required, type definitions are in package `@types/react-redux`.

`react-redux` package has a dependency on `@types/react-redux`, so these will automatically be installed with the library.

# Quick Start

```bash
npm install @reduxjs/toolkit react-redux
```

## Create a Redux Store

## Provide the store to React

## Create Redux State Slice

## Add Slice Reducers to store

## Use Redux State and Actions in React Components