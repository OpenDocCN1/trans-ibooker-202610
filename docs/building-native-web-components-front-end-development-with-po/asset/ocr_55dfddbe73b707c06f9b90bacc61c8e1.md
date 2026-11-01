# Listing 9-23. Adding Router in main.js

```
import { createApp } from 'vue'
import App from './App.vue'
import router from './router'

createApp(App).use(router).mount('#app')
```

Edit the router/index.js file and add the router for Dashboard and About, as shown in Listing 9-24.

# Listing 9-24. Adding Router in router/index.js

```
import { createRouter, createWebHistory } from 'vue-router'
import Dashboard from '../views/Dashboard.vue'
const routes = [
  {
    path: '/',
    name: 'Dashboard',
    component: Dashboard
  },
  {
    path: '/about',
    name: 'About',
    component: () => import(/* webpackChunkName: "about" */ '../views/About.vue')
  }
]
const router = createRouter({
  history: createWebHistory(process.env.BASE_URL),
  routes
})

export default router
```

If you look at the /about route, we are using import() to load the component. This is because we are loading the component when the user employs that view, instead of loading everything when starting the app. This is called lazy load, and it’s good for