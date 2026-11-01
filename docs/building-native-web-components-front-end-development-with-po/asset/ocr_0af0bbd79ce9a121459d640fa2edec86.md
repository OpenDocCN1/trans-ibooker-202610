```
```js
handleEvent() {
  console.log('It is Loaded');
}
}
```
```

You can read more about events at https://lit-element.polymer-project.org/guide/events.

# Life Cycle

LitElement inherits the default life cycle callbacks from the Web Components standard and some additional methods that can be used to add logic in our components.

connectedCallback: This is invoked when a component is added to the document’s DOM (Listing 8-11).

# Listing 8-11. connectedCallback

```
```js
import { LitElement, html } from 'lit-element';

class MyComponent extends LitElement {
    connectedCallback() {
        super.connectedCallback();
        console.log('added');
    }
}
```
```

disconnectedCallback: This is invoked when a component is removed from the document’s DOM (Listing 8-12).

# Listing 8-12. disconnectedCallback

```
```js
import { LitElement, html } from 'lit-element';

class MyComponent extends LitElement {
    disconnectedCallback() {
        super.disconnectedCallback();
```
```