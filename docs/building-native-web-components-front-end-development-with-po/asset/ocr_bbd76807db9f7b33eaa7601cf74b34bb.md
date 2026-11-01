```
```js
render() {
    return html`
        <div>Content in Blue!</div>
    `;
}
```
```

You can read more about styles at https://lit-element.polymer-project.org/guide/styles.

# Events

You can add events listeners directly in your template, as in Listing 8-9.

# Listing 8-9. Adding an Event in the Template

```
```js
import { LitElement, html } from 'lit-element';
class MyComponent extends LitElement {
    render() {
        return html`<button @click="${this.handleEvent}">click</button>`;
    }
    handleEvent(e) {
        console.log(e);
    }
}
```
```

You can also add the listener directly in the component, in some method of the life cycle, as in Listing 8-10.

# Listing 8-10. Adding an Event in the Component

```
```js
import { LitElement, html } from 'lit-element';
class MyComponent extends LitElement {
    constructor() {
        super();
        this.addEventListener('DOMContentLoaded', this.handleEvent);
    }
}
```
```