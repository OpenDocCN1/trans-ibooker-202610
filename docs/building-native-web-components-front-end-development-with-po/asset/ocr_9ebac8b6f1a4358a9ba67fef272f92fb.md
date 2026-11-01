# Listing 9-4. Using beforeCreate in a Vue Component

```
```js
Vue.createApp({
  beforeCreate: function () {
    console.log('Initialization is happening');
  }
})
```
```

# created

The created hook runs when your component is initialized. You will be able to access reactive data, and events are active. You can use this method as in Listing 9-5.

# Listing 9-5. Using created in a Vue Component

```
```js
Vue.createApp({
  created: function () {
    console.log('The Component is created');
  }
})
```
```

# beforeMount

The beforeMount hook runs right before the initial render occurs and after the template or render functions have been compiled. You can use this method as in Listing 9-6.

# Listing 9-6. Using beforeMount in a Vue Component

```
```js
Vue.createApp({
  beforeMount: function () {
    console.log('The component is going to be Mounted');
  }
})
```
```

# mounted

The mounted hook provides full access to the reactive component, templates, and rendered DOM (via. this.$el). You can use this method as in Listing 9-7.