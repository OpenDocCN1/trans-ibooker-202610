![](bbox=[0.35, 0.1, 0.69, 0.38])

Figure 9-21. mwc-dialog in Dashboard.vue

Also, we are going to add the library uuid. With this library, we can generate new unique ids for the new notes. Run the following:

```
$npm install -save uuid
```

In the template, we are going to add mwc-dialog, as in Listing 9-31.

# Listing 9-31. Adding mwc-dialog

```
<!-- eslint-disable vue/no-deprecated-slot-attribute -->
<template>
  <div>
    <mwc-list v-for="(note) in notes" :key="note.id" multi>
      <mwc-list-item twoline hasMeta>
        <span>{{note.title}}</span>
        <span slot="meta" class="material-icons" @click="handleDelete(note.id)">delete</span>
        <span slot="secondary">{{note.description}}</span>
      </mwc-list-item>
      <li divider padded role="separator"></li>
    </mwc-list>
  </div>
</template>
```