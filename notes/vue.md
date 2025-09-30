# Vue

- Frontend JavaScript framework for building websites and UIs
- Generally used to create SPAs (Single Page Applications) that run on the client, but can also be run on the server-side by using a SSR framework like Nuxt

## Why Vue? 

- Create dynamic frontend apps & websites
- Easy learning curve
- Easy to integrate with other projects
- Fast, lightweight
- Virtual DOM
- Extremely popular
- Great Community

## Basic Layout of Vue Component

- Components include a template for markup, logic including and state/data/methods as well as the styling for that component.
- You can pass "props" into a component when embedding it like `<Header title="My Header">`
- `scoped` on `<style>` limits the styling to this specific component
```vue
<template>
  <header>
    <h1>{{title}}</h1>
  </header>
</template>

<script>
export default {
  props: {
    title: String,
  },
}
</script>

<style scoped>
header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
}
</style>
```

## State / Data

- Components can have their own state which determine how a specific component behaves and what data is displayed
- Some state may be local to a specific component and some may be "global" or "app" level state that needs to be shared with multiple components
- `Vuex` and `Pinia` are state managers for global state in larger applications (similar to redux)

## Options API vs Composition API

- Composition API aims to address code reusability and readability, especially in larger applications

## Vue CLI

- Standard tooling for Vue.js development
- Has CLI for creating vue apps
- Has dev server and easy production build
- Optional GUI for managing Vue projects
- Integrated testing, TypeScript support, ESLint

## Basic App
```html
...
  <div id="app"></div>
...
```
```js
const app = Vue.createApp({
  template: '<h1>Hello {{name}}</h1>',
  data() {
    return {
      name: 'John',
    };
  }
});

app.mount('#app');
```
- instead of using the `template` property, can also add the html inside the `#app` div directly

## New project setup

```
npm init vue@latest
```
- then follow the options
- then run:
```
cd <new project folder>
npm install
npm run dev
```

### Structure

- `public`
- `index.html` is entry html file with the `app` div
- `src`
  - `main.js` is the entry point of the application
  - `App.vue` is main/root component
  - `components`

## Data Binding

- Use `v-bind` to assign dynamic values to attributes such as `src`, `alt` `class` and `style`
- e.g. `v-bind:src="picture"` (`picture` is from data)
- `:src` is shorthand for `v-bind:src`
- to compute/combine properties:
```
:alt="`${firstName} ${lastName}`"
```
## Events

- `v-on:click="refreshData()"`, where `refreshData` is in `methods`
  - `@click` is the same as `v-on:click`

# Forms

- `v-model` binds values to inputs

# Router

This is an option when first setting up the vue project, but to do it manually:
- `npm install vue-router@next`
- create `router` folder in `src`
- create `index.js` inside `router` folder
```js
import {createRouter, createWebHistory} from 'vue-router';
// Components / Pages from /views
import Home from '../views/Home.vue';
import About from '../views/About.vue';

const routes = [
  {
    path: '/',
    name: 'Home',
    component: Home,
  },
  {
    path: '/about',
    name: 'About',
    component: About,
  }
];

const router = createRouter({
  history: createWebHistory(import.meta.env.BASE_URL),
  routes,
});

export default router;
```
- import router into `main.js` and `use`
```js
// ...
import router from './router/index';
// ...
createApp(App)
  .use(router)
  .mount('#app')
```
- Links
```html
<router-link to="/about">About</router-link>
```
- Using  in `App.vue`:
```html
<template>
  <!-- Header? -->
  <router-view></router-view>
  <!-- Footer? -->
</template>
```
- can also pass props to views
```html
<router-view :showHeaderButtons="showHeaderButtons">
</router-view>
```
  - For views that don't use every attribute passed as either `props` or `emits`, then add `inheritAttrs: false` to the component
- `this.$route.path` can be used within code to check current path (e.g. `/` is home page)
- `this.$router` can be used within code to redirect etc

# Vite

## Proxy

```js
server: {
  proxy: {
    '^/api': {
      target: 'http://localhost:5000',
      changeOrigin: true,
      logLevel: 'debug',
      rewrite: (path) => path.replace(/^\/api/, ''),
    },
  }
}
```