# Reloading the Development Server

If you add a Vuex store to your Nuxt application, you'll need to restart the development server before you can access it from your components!

Defining the store is slightly different than in a non-Nuxt app: the store file needs to return a factory function that will return a new store when called.

# store/index.js

```
import Vuex from 'vuex'

export default () => {
  return new Vuex.Store({
    state: () => ({
      // ...
    }),
    mutations: {
      // ...
    }
  })
}
```

It’s also important to note that the state property must be a function that returns the base state object. This is to prevent problems with shared state when the site is rendered on the server.

The store can also define a special action, nuxtServerInit, which will be called whenever a page is rendered on the server:

```
actions: {
  nuxtServerInit ({ commit }, { req, res }) {
    // ...
  }
}
```

As well as the usual Vuex context object that actions receive, this method receives a Nuxt context object as its second argument.

This object contains quite a lot of keys (see the PI documentation for a full list), including the server request and response objects—req and res respectively. This