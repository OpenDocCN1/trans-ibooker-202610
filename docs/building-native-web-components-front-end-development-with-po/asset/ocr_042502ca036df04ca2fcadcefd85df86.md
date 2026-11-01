```
<button type="button" id="deleteButton">Delete</button>
</div>`;
}
}
customElements.define('note-list-item-component', NoteListItemComponent);
```

The process to migrate this component is similar to the steps that we followed before. Therefore, I'll skip the explanation and just show you the complete component in LitElement (Listing 8-29).

# Listing 8-29. NoteListItemComponent with LitElement

```
import { LitElement, html, css } from "lit-element";

/**
 * A Note List Item Component
 */
export class NoteListItemComponent extends LitElement {
  static get properties() {
    return {
      /**
       * The attribute is an Object.
       */
      note: {
        type: Object,
        attribute: true,
        reflect: true,
      },
      /**
       * The attribute is a number.
       */
      idx: {
        type: Number,
        attribute: true,
        reflect: true,
      }
    };
  }
}
```