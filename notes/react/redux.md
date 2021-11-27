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

Create empty Redux store and export:

(`app/store.ts`)
```ts
import { configureStore } from '@reduxjs/toolkit';

export const store = configureStore({
  reducer: {
		// posts: postsReducer,
		// comments: commentsReducer,
		// users: usersReducer,
  },
});

// Infer the `RootState` and `AppDispatch` types from the store itself
export type RootState = ReturnType<typeof store.getState>;
// Inferred type for commented out example would be:
// {posts: PostsState, comments: CommentsState, users: UsersState}
export type AppDispatch = typeof store.dispatch;
```

For TypeScript, `configureStore` should not need additional typings but `RootState` and `Dispatch` types should be extracted so they can be referenced as needed. Inferring these types from the store itself means they update correctly as more slices are added or middleware settings are modified.

## Provide the store to React

Wrap application with `<Provider>` to make the store available to React components:

(`index.tsx`)
```tsx
import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
import App from './App';
import { store } from './app/store';
import { Provider } from 'react-redux';

ReactDOM.render(
  <Provider store={store}>
    <App />
  </Provider>,
  document.getElementById('root')
);
```

## Create Redux State Slice

Create a slice, providing a string name to identify the slice, an initial state value, and reducer functions to define how state can be updated.

Then export the generated Redux action creators and the reducer function for the whole slice.

Redux Toolkit's `createSlice` and `createReducer` APIs use `Immer` inside to allow writing mutating update logic that becomes correct immutable updates (different to Redux's requirement of writing state updates immutably by making copies to update).

(`features/counter/counterSlice.ts`)
```ts
import { createSlice, PayloadAction } from '@reduxjs/toolkit';

// Define a type for the slice state
export interface CounterState {
  value: number
}

// Define the initial state using that type
const initialState: CounterState = {
  value: 0,
};

export const counterSlice = createSlice({
  name: 'counter',
  // `createSlice` will infer the state type from the `initialState` argument
  initialState,
  reducers: {
    increment: (state) => {
      // Redux Toolkit allows us to write "mutating" logic in reducers. It
      // doesn't actually mutate the state because it uses the Immer library,
      // which detects changes to a "draft state" and produces a brand new
      // immutable state based off those changes
      state.value += 1
    },
    decrement: (state) => {
      state.value -= 1
    },
	 // Use the PayloadAction type to declare the contents of `action.payload`
    incrementByAmount: (state, action: PayloadAction<number>) => {
      state.value += action.payload
    },
  },
});

// Action creators are generated for each case reducer function
export const { increment, decrement, incrementByAmount } = counterSlice.actions;

export default counterSlice.reducer;
```

For TypeScript, each slice file should define a type for its initial state value, so `createSlice` can correctly infer the type of `state` in each case reducer.

Generated actions should be defined using the `PayloadAction<T>` type which takes the type of the `action.payload` field as its generic argument. The generated action creators will be correctly typed to accept a `payload` argument based on this type provided (e.g. `incrementByAmount` requires a `number`).

The `RootState` type from the store file can be imported (circular import but TS compiler can correctly handle that for types) if needed for something like selector functions:

```ts
import type { RootState } from '../../app/store';

// ... above code

// Other code such as selectors can use the imported `RootState` type
export const selectCount = (state: RootState) => state.counter.value;

export default counterSlice.reducer;
```



## Add Slice Reducers to store

Import reducer function from slice and add to store.

By defining a field inside the `reducer` parameter, we tell the stoer to use this slice reducer function to handle all updates to that state.

(`app/store.ts`)
```ts
import { configureStore } from '@reduxjs/toolkit';
import counterReducer from '../features/counter/counterSlice';

export default configureStore({
  reducer: {
    counter: counterReducer,
  },
})
```

## Define Typed Hooks

It is recommended to create typed versions of `useDispatch` and `useSelector` hooks because:
- For `useSelector`, it saves the need to type `(state: RootState)` every time
- For `useDispatch`, the default `Disppatch` type does not know about thunks. In order to correctly dispatch thunks, you need to use the specific customized `AppDispatch` type from the store that includes the thunk middleware types. Adding a pre-typed `useDispatch` hook keeps you from forgetting to import `AppDispatch` where it's needed.

(`app/hooks.ts`)
```ts
import { TypedUseSelectorHook, useDispatch, useSelector } from 'react-redux';
import type { RootState, AppDispatch } from './store';

// Use throughout your app instead of plain `useDispatch` and `useSelector`
export const useAppDispatch = () => useDispatch<AppDispatch>();
export const useAppSelector: TypedUseSelectorHook<RootState> = useSelector;
```

## Use Redux State and Actions in React Components

Use React-Redux hooks to let React components inside `<App>` interact with the Redux store.

Read data from the store with `useSelector`, and dispatch actions using `useDispatch`. Here we use the pre-typed hooks instead of the standard hooks from React-Redux.

In the example when the increment/decrement buttons are clicked:
- the Redux action will be dispatched to the store
- slice reducer will see the actions and update its state
- the component will see the new state value from the store and re-render itself

(`features/counter/Counter.tsx)`
```tsx
import React from 'react';
import { RootState } from '../../app/store';
// import { useSelector, useDispatch } from 'react-redux';
import { useAppSelector, useAppDispatch } from 'app/hooks';
import { decrement, increment } from './counterSlice';

export function Counter() {
//   const count = useSelector((state: RootState) => state.counter.value);
//   const dispatch = useDispatch();

// The `state` arg is correctly typed as `RootState` already
  const count = useAppSelector((state) => state.counter.value);
  const dispatch = useAppDispatch();

  return (
    <div>
      <div>
        <button
          aria-label="Increment value"
          onClick={() => dispatch(increment())}
        >
          Increment
        </button>
        <span>{count}</span>
        <button
          aria-label="Decrement value"
          onClick={() => dispatch(decrement())}
        >
          Decrement
        </button>
      </div>
    </div>
  );
};
```