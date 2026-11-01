# Listing 8-25. Adding a Template in NoteListComponent with LitElement

```
import {LitElement, html, css} from 'lit-element';

export class NoteListComponent extends LitElement {
static get properties() {
    return {
      notes: {
        type: Array,
        attribute: true,
        reflect: true,
      }
    };
  }

  constructor() {
    super();
    this.notes = this.getAttribute("notes") || [];
  }

  render() {
    return html`
      ${this.notes.map((note, idx) => {
        return html` <note-list-item-component
          note="${JSON.stringify(note)}"
          idx="${idx}"
        ></note-list-item-component>`;
      })}
    `;
  }
}
```

Here, we are using map() to iterate in notes and returning all as an HTML tag literal, to make our template work correctly.

We must update notes each time that the value changes, but to do this, we must do something special, because when passed as a reference, the hasChanged() option doesn’t detect when a change in the array is occurring. To fix this, we are going to use attributeChangedCallback(), as in Listing 8-26.