![](bbox=[0.22, 0.1, 0.78, 0.48])

# Figure 9-2. Hierarchy of components

Now, think that each component can be what we want—a list, an image, a button, text, or whatever we define. The fundamental way to define a simple Vue’s component is shown in Listing 9-1.

# Listing 9-1. Declaring a Vue’s Component

```
```js
const app = createApp({...})
app.component('my-button', {
  data: function () {
    return {
      counter: 0
    }
  },
  template: '<button v-on:click="counter++">Clicks {{ counter }}.</button>'
})
```
```

We can add this in our app as a new HTML tag, as in Listing 9-2.