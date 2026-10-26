
Where existing code is required for context, rather than repeat all of it, … will be displayed:

```
function animate() {
    ...
    new_variable = "Hello";
}
```

Some lines of code should be entered on one line, but we’ve had to wrap them because of page constraints. An ➜ indicates a line break that exists for formatting purposes only, and should be ignored:

```
URL.open("http://www.sitepoint.com/responsive-web-design-real-user-testing/?responsive1");
```

You’ll notice that we’ve used certain layout styles throughout this book to signify different types of information. Look out for the following items.

## TIPS, NOTES, AND WARNINGS

> **Hey, You!**
>
> Tips provide helpful little pointers.

> **Ahem, Excuse Me ...**
>
> Notes are useful asides that are related—but not critical—to the topic at hand. Think of them as extra tidbits of information.

> **Make Sure You Always ...**
>
> ... pay attention to these important points.

![](img/eaf78d0df5a5560ef972f68b7d6e48cc_14_0.png)

In the example above, the `v-model` directive is being used to bind the `text` property to the `<input>` element. When Vue initially renders the template, the input will be pre-filled with the content of `text`. Changing the input field will cause the `text` property to be updated and the output inside the `<p>` element to be re-rendered.

## v-on

We can use the `v-on:<event>` syntax for attaching listeners to events that are emitted by elements and components:

```
<button v-on:click="heading = 'Hello World!'">Click Me</button>
```

In the example above, the `heading` property on our instance data would be set to “Hello, World!” when the button is clicked. As with interpolations, the values you pass to directives can include any valid JavaScript expression, and can directly reference instance data properties as if they were local variables.

Vue allows you to use a shorthand for binding event handlers, prefixing them with `@` rather than the more verbose `v-on:`. We’ll be using this shorter syntax throughout the rest of the book.

As well as modifying data properties, you can also call custom methods on your Vue instance. The method will receive the event object as the first argument.

## JavaScript

```javascript
new Vue({
  el: "#main",
  data: {
    status: ""
  },
  methods: {
    updateStatus(event) {
```

```
},
"extends": [
"plugin:vue/essential",
"eslint:recommended"
],
"rules": {},
"parserOptions": {
"parser": "babel-eslint"
}
},
"postcss": {
"plugins": {
"autoprefixer": {}
}
},
"browserslist": [
"> 1%",
"last 2 versions",
"not ie <= 8"
]
}
```

As you can see, ESLint has been set up with the recommended ruleset, and the vue/essential plugin (which you can read more about in the ESLint plugin’s repo). Babel has also been configured to support all the browsers covered by the browserslist options.

While the default preset is a good starting point for learning about build tooling in Vue apps, and for building quick prototypes, for proper applications you’re probably going to want to customize the project by selecting the features you want.

## Webpack Configuration

Vue CLI is built on top of webpack but abstracts away all the configuration to make it easier to get up and running. You can see the webpack configuration at any time by running the `vue inspect` command.

Running Vue CLI and choosing to manually select features presents you with the following screen:

self-closing form unless we want it to wrap any child elements (we’ll look at this later).

## LIFECYCLE METHODS

If you’ve used React before, you’re probably familiar with the concept of **component lifecycle methods**. The idea behind this is that there are various events that happen in the lifetime of a component—from when it’s created, through to when it’s destroyed. The lifecycle methods are called at these points, allowing you to execute code—for example, to perform an Ajax request when the component is initialized.

The Vue documentation includes a useful chart of all of the various methods and when in the lifecycle they’re called.

The methods you’ll probably find yourself using most often are:

- **created**. This is the place to kick off any Ajax requests for fetching data.
- **mounted**. When this hook is fired, the component has been rendered and inserted into the DOM. This is where you can manipulate the DOM elements—for example, if you’re wrapping a non-Vue library of some sort.
- **beforeDestroy**. Just before the component is removed from the DOM, this method is called. You can do any cleanup here.

If we wanted to load in the staff data for our component from an API, we could do something like the following.

### src/StaffDirectory.vue

```
<script>
  export default {
    name: 'StaffDirectory',
    data() {
      return {
        heading: "Staff Directory",
        sortBy: 'department',
        employees: []
```

## Chapter 4: Routing

If you’ve built server-side web apps before, you’ll be familiar with putting some thought into the URL structure of your application.

Using a client-side router will give your SPAs the advantages of a server-side web app—namely, the ability to use the browser’s back and forward buttons to move through your app’s screens, and the ability for the user to bookmark specific screens and/or states of your app.

Vue.js has the advantage of having a library that’s not only extremely capable and well documented, but which is also the official routing solution for Vue, and is guaranteed to be maintained and kept in sync with the development of the core library.

### Installing Vue Router

The router can be easily added to your app via npm (`npm install vue-router`), or from the Vue CLI.

For the purposes of this chapter, let’s install the router ourselves via npm and run through the steps needed to get a basic example up and running. As we go, I’ll briefly explain each of the necessary parts, which we’ll revisit in more depth later in the chapter.

If you haven’t already, use the Vue CLI to create a new project based on the “default” preset. After it has finished installing, change directory into the project’s root folder and run the following command:

```
npm install vue-router
```

Once the router library is installed, we need to configure it and add some routes to map URLs to components. Create a file called `router.js` inside the `src` folder and add the following.

passing an array of route configuration objects to the router when we instantiate it.

As a minimum, we need to supply a path (that is, a URL) and a component for each route we want to define:

```javascript
import HelloWorld from './components/HelloWorld';

const routes = [
  {
    path: '/hello-world',
    component: HelloWorld
  }
];
```

The above route will display the `HelloWorld` component when the user navigates to the `/hello-world` URL.

There are other useful options you might want to set when creating routes.

- **name.** You can supply a name for your route. This is optional, but I’d recommend it for all but the simplest of apps. Using named routes allows your URL structure to change without breaking your app, and can make it easier to navigate to programmatically when dealing with dynamic routes.
- **redirect.** By entering the path of another route here, you can create redirects within your app.
- **beforeEnter.** You can provide a function that will be called before the route change, allowing you to perform additional logic before continuing or canceling the navigation. Later on, we’ll look at how this can be used to make certain routes available to authenticated users only.
- **meta.** You can use this option to provide an object of custom properties to be made available to the route.

> ### Additional Options
>
> As with the router options, there are more options you can set on routes that I haven’t gone into here. I’d recommend _browsing_ the API documentation if you want to know more.
