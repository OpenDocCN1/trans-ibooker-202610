
```
this.note = JSON.parse(newValue);
this.root.innerHTML = this.render();
break;
}
}
connectedCallback() {
    this.root.addEventListener('del-event', this.handleDelEvent);
}
disconnectedCallback () {
    this.root.removeEventListener('del-event', this.handleDelEvent);
}
handleDelEvent(e) {
    this._notes.splice(e.detail.idx, 1);
    this.root.innerHTML = this.render();
}
render() {
    let noteElements = '';
    this._notes.map(
        (note, idx) => {
            noteElements += `
            <note-list-item-component note='${JSON.stringify(note)}'
            idx='${idx}'></note-list-item-component>`;
        }
    );
    return `
        ${noteElements}`;
}
get notes(){
    return this._notes;
}
```
```
set notes(newValue) {
    this._notes = newValue;
    this.root.innerHTML = this.render();
}
}
customElements.define('note-list-component', NoteListComponent);
```

We are going to add the properties for the component, per Listing 8-24.

## Listing 8-24. Adding Properties in NoteListComponent with LitElement

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
}
```

Here, we are defining our notes property as an array and adding the options attribute to make the property work correctly as an attribute. In the constructor(), we are initializing this.notes with the value in 'notes' or an empty array.

Now we are going to migrate the render() method, which is similar to a LitElement, as you can see in Listing 8-25.

## Listing 8-25. Adding a Template in NoteListComponent with LitElement

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

## Listing 8-26. Adding attributeChangedCallback in NoteListComponent with LitElement

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

  attributeChangedCallback() {
    this.notes = [...this.notes];
    super.attributeChangedCallback();
  }

  render() {
    return html`
      ${this.notes.map((note, idx) => {
        return html`<note-list-item-component
          note="${JSON.stringify(note)}"
          idx="${idx}"
        ></note-list-item-component>`;
      })}
    `;
  }
}
```

Here, in attributeChangedCallback(), we are detecting a change in the component. If it is occurring, we can update this.notes with a new array (not a reference), using the spread operator, and with this small fix, our component is going to work correctly again.

Finally, we are going to add the event listener for 'del-event' and the handleDelEvent() function, as in Listing 8-27. With these modifications, our component is ready.

## Listing 8-27. Adding the Event Listener in NoteListComponent with LitElement

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
    this.addEventListener("del-event", this.handleDelEvent);
}

attributeChangedCallback() {
    this.notes = [...this.notes];
    super.attributeChangedCallback();
}

handleDelEvent(e) {
    this.notes.splice(e.detail.idx, 1);
    this.requestUpdate();
}
}
```
```
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
```

You can access the code for this book (https://github.com/carlosrojaso/apress-book-web-components) at $git checkout chap-8-1.

I created an example in the dev/ folder, documentation in the docs-src/ folder, and some basic tests in the test/ folder, then you can run the following:

```
$ npm install

$ npm run serve
```

To see the example running in localhost:8080/dev, run the following:

```
$ npm run docs

$ npm run docs:serve
```

Now we are going to migrate our last component, <note-list-item-component>, which we wrote in the previous chapters (see Listing 8-28).

## Listing 8-28. NoteListComponent in VanillaJS

```
export class NoteListItemComponent extends HTMLElement {
    static get observedAttributes() { return ['note', 'idx']; }

    constructor() {
        super();

        this._note = JSON.parse(this.getAttribute('note')) || {};
        this.idx = this.getAttribute('idx') || -1;
```
```
this.root = this.attachShadow({mode: 'open'});
this.root.innerHTML = this.getTemplate();

this.delBtn = this.root.getElementById('deleteButton');
this.handleDelete = this.handleDelete.bind(this);
}

connectedCallback() {
    this.delBtn.addEventListener('click', this.handleDelete);
}

disconnectedCallback () {
    this.delBtn.removeEventListener('click', this.handleDelete);
}

attributeChangedCallback(name, oldValue, newValue) {
    switch(name) {
        case 'note':
            this.note = JSON.parse(newValue);
            this.handleUpdate();
            break;
        case 'idx':
            this.idx = newValue;
            this.handleUpdate();
            break;
    }
}

get note() {
    return this._note;
}

set note(newValue) {
    this._note = newValue;
}

get idx() {
    return this._idx;
}
```
```
set idx(newValue) {
    this._idx = newValue;
}

handleDelete() {
    this.dispatchEvent(new CustomEvent('del-event', {bubbles:
    true,  composed: true, detail: {idx: this.idx}}));
}

handleUpdate() {
    this.root.innerHTML = this.getTemplate();
    this.delBtn = this.root.getElementById('deleteButton');
    this.handleDelete = this.handleDelete.bind(this);
    this.delBtn.addEventListener('click', this.handleDelete);
}

getStyle() {
    return `
<style>
.note {
    background-color: #ffffcc;
    border-left: 6px solid #ffe3b;
}
div {
    margin: 5px 0px 5px;
    padding: 4px 12px;
}
</style>
`;
}

getTemplate() {
    return`
${this.getStyle()}
<div class="note">
    <p><strong>${this._note.title}</strong> ${this._note.description}
    </p><br/>
```
```
<button type="button" id="deleteButton">Delete</button>
</div>`;
}
}
customElements.define('note-list-item-component', NoteListItemComponent);
```

The process to migrate this component is similar to the steps that we followed before. Therefore, I'll skip the explanation and just show you the complete component in LitElement (Listing 8-29).

## Listing 8-29. NoteListItemComponent with LitElement

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
