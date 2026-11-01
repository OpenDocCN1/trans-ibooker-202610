# Slots

In addition to `<template>`, we can take advantage of `<slot>` inside our content. Slots allow you to define placeholders in your template, as shown in Figure 3-3. Combined with other Web Components specifications, slots can be very useful in inserting markup inside elements.

The basic way to use `<slot>` is outlined in Listing 3-3.

# Listing 3-3. Using <slot>

```
<p>
    <slot>This is a default message</slot>
</p>
```

![](bbox=[0.26, 0.4, 0.6, 0.78])

Figure 3-3. Filling a slot with a message in error-component