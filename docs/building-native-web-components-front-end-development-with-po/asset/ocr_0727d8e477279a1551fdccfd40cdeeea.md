# Listing 9-7. Using mounted in a Vue Component

```
Vue.createApp({
  mounted: function () {
    console.log('The component is mounted');
  }
})
```

# beforeUpdate

The beforeUpdate hook runs after data changes on your component and the update cycle begins, right before the DOM is patched and re-rendered. You can use this method as in Listing 9-8.

# Listing 9-8. Using beforeUpdate in a Vue Component

```
Vue.createApp({
  beforeUpdate: function () {
    console.log('The component is going to be updated');
  }
})
```

# updated

The updated hook runs after data changes on your component and the DOM re-renders. You can use this method as in Listing 9-9.

# Listing 9-9. Using updated in a Vue Component

```
Vue.createApp({
  updated: function () {
    console.log('The component is updated');
  }
})
```