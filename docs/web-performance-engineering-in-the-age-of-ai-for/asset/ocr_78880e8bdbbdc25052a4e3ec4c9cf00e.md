suppose you ask for a component that filters a list of items based on user input, and the AI integrates that logic:

```
function FilteredList({ items, query }) {
  const filteredItems = items.filter((item) => matchesQuery(item, query));
  // ... then maps filteredItems to JSX ...
}
```

This looks straightforward, but if items is large and this component rerenders frequently (say query is bound to a text input where the user types), it could become a performance nightmare. With every keystroke, the entire list (maybe thousands of items) is filtered on the main thread. This could cause noticeable input lag or even freeze if the list is big enough. A human developer might immediately think to debounce the filtering or move heavy computation to a web worker, but the AI-provided code won’t do that unless explicitly told. Similarly, an AI might sort data in the render path, perform deep copies of large objects, or update state in a loop—all patterns that can lead to long tasks that block the UI.

The result of such patterns is often a high total blocking time (TBT) in lab tests and poor interaction to next paint (INP) in the field. Users experience this as the UI stuttering or being unresponsive to clicks and keypresses. In extreme cases, the browser might even throw a “Page Unresponsive” warning if a JavaScript function runs for too long uninterrupted. Watch out for any AI-generated code that performs heavy synchronous work on the main thread.

Profiling the runtime performance is key here. Use the Chrome DevTools Performance panel to record a timeline while you interact with your page. The flame chart will show you if there are long tasks, like a 300-ms or 500-ms JavaScript function blocking the main thread. If you see a big chunk of activity corresponding to an AI-generated function, that’s a red flag.

So how do we address this issue? Optimize by splitting that work, using techniques like requestAnimationFrame to chunk it across multiple frames, or offloading it, perhaps using web workers for heavy computations