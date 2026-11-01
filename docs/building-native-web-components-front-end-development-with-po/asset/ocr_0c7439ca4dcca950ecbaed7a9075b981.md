```
<li divider padded role="separator"></li>
<mwc-list-item twoline>
  <span>Item 3</span>
  <span slot="secondary">Secondary line</span>
</mwc-list-item>
</mwc-list>
<mwc-fab class="floatButton" mini icon="add"></mwc-fab>
</div>
</template>
<script>
import '@material/mwc-list/mwc-list';
import '@material/mwc-list/mwc-list-item';
import '@material/mwc-fab';

export default {
  name: 'Dashboard'
}
</script>
<style scoped>
.floatButton {
  position: fixed;
  bottom: 20px;
  right: 20px;
}
</style>
```

We are adding mwc-list-items to show the notes and an mwc-fab to have a floating button that we are going to use to add new notes.

We are also adding views/About.vue, as shown in Listing 9-27.

# Listing 9-27. Adding Routes in About.vue

```
<template>
  <div class="about">
    <h1>Building Native Web Components</h1>
    <h2><i>Front-End Development with Polymer and Vue.js</i></h2>
    <p>
```