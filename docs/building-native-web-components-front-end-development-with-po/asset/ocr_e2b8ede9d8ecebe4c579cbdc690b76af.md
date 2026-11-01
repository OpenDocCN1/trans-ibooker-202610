```
</style>
    <div class="error">
        <slot>Error component</slot>
    </div>
</template>
<template class="none-type">
    <style>
    .none {
        background-color: gray;
        padding: 15px;
        color: black;
    }
    </style>
    <div class="none">
        <slot>Error component</slot>
    </div>
</template>
    `;
}
}
customElements.define('error-component', ErrorComponent);
</script>
</body>
</html>
```

In our markup, we have three <template> blocks—one for each kind of message that we can receive: error, warning, and none. In addition, we are adding <slot> in each of these, which will take the value that we pass between our tags, as in Listing 3-6.

# Listing 3-6. Passing Error Messages with Slots

```
<error-component>Value that the slot going to take</error-component>
```

Finally, we are going to use the life cycle hook connectedCallback() to handle the logic for choosing which template to use, as in Listing 3-7.