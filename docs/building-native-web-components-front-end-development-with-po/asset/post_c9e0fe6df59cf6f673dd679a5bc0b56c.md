
```js
handleEvent() {
  console.log('It is Loaded');
}
}
```

You can read more about events at https://lit-element.polymer-project.org/guide/events.

## Life Cycle

LitElement inherits the default life cycle callbacks from the Web Components standard and some additional methods that can be used to add logic in our components.

connectedCallback: This is invoked when a component is added to the document’s DOM (Listing 8-11).

### Listing 8-11. connectedCallback

```js
import { LitElement, html } from 'lit-element';

class MyComponent extends LitElement {
    connectedCallback() {
        super.connectedCallback();
        console.log('added');
    }
}
```

disconnectedCallback: This is invoked when a component is removed from the document’s DOM (Listing 8-12).

### Listing 8-12. disconnectedCallback

```js
import { LitElement, html } from 'lit-element';

class MyComponent extends LitElement {
    disconnectedCallback() {
        super.disconnectedCallback();
```
```
console.log('removed');
}
}
```

adoptedCallback: This is invoked when a component is moved to a new document (Listing 8-13).

### Listing 8-13. adoptedCallback

```js
import { LitElement, html } from 'lit-element';

class MyComponent extends LitElement {
  adoptedCallback() {
    super.disconnectedCallback();
    console.log('moved');
  }
}
```

attributeChangedCallback: This is invoked when a component attribute changes (Listing 8-14).

### Listing 8-14. attributeChangedCallback

```js
import { LitElement, html } from 'lit-element';

class MyComponent extends LitElement {
  attributeChangedCallback(name, oldVal, newVal) {
    super.attributeChangedCallback(name, oldVal, newVal);
    console.log('attribute change: ', name, newVal);
  }
}
```

firstUpdated: This is invoked after the first time your component has been updated and rendered (Listing 8-15).

### Listing 8-15. firstUpdated

```js
import { LitElement, html } from 'lit-element';

class MyComponent extends LitElement {
  firstUpdated(changedProperties) {
    console.log('first updated');
  }
}
```

updated: This is invoked when the element’s DOM has been updated and rendered (Listing 8-16).

### Listing 8-16. updated

```js
import { LitElement, html } from 'lit-element';

class MyComponent extends LitElement {
  updated(changedProperties) {
    changedProperties.forEach((oldValue, propName) => {
      console.log(`${propName} changed. oldValue: ${oldValue}`);
    });
  }
}
```

You can read more about the life cycle in https://lit-element.polymer-project.org/guide/lifecycle.

## Building with Polymer

In this section, we are going to build our <simple-form-modal-component>, <note-list-component>, and <note-list-item-component>, using the Polymer starter.

As you see in Listing 8-17, we have our SimpleFormModalComponent in VanillaJS.

### Listing 8-17. SimpleFormModalComponent in VanillaJS

```js
export class SimpleFormModalComponent extends HTMLElement {
    constructor() {
        super();

        this.root = this.attachShadow({mode: 'open'});
        this.container = document.createElement('div');
        this.container.innerHTML = this.getTemplate();
        this.root.appendChild(this.container.cloneNode(true));

        this._open = this.getAttribute('open') || false;

        this.modal = this.root.getElementById("myModal");
        this.addBtn = this.root.getElementById("addBtn");
        this.closeBtn = this.root.getElementById("closeBtn");

        this.handleAdd = this.handleAdd.bind(this);
        this.handleCancel = this.handleCancel.bind(this);
    }

    connectedCallback() {
        this.addBtn.addEventListener('click', this.handleAdd);
        this.closeBtn.addEventListener('click', this.handleCancel);
    }

    disconnectedCallback () {
        this.addBtn.removeEventListener('click', this.handleAdd);
        this.closeBtn.removeEventListener('click', this.handleCancel);
    }

    get open() {
        return this._open;
    }

    set open(newValue) {
        this._open = newValue;
        this.showModal(this._open);
    }
}
```
```js
handleAdd() {
    const fTitle = this.root.getElementById('ftitle');
    const fDesc = this.root.getElementById('fdesc');
    this.dispatchEvent(new CustomEvent('add-event', {bubbles: true,
    composed:true, detail: {title: fTitle.value, description: fDesc.value}}));

    fTitle.value = '';
    fDesc.value = '';
    this.open = false;
}

handleCancel() {
    this.open = false;
}

showModal(state) {
    if(state) {
        this.modal.style.display = "block";
    } else {
        this.modal.style.display = "none"
    }
}

getTemplate() {
    return `
    ${this.getStyle()}
    <div id="myModal" class="modal">
        <div class="modal-content">
            <form id="myForm">
                <label for="ftitle">Title:</label><br>
                <input type="text" id="ftitle" name="ftitle"><br>
                <label for="fdesc">Description:</label><br>
                <textarea id="fdesc" name="fdesc" rows="4" cols="50">
                </textarea><br/>
                <button type="button" id="addBtn">Add</button><button type="button" id="closeBtn">Close</button>
```
```
</form>
</div>
</div>`;
}
getStyle() {
    return `
<style>
.modal {
    display: none;
    position: fixed;
    z-index: 1;
    padding-top: 100px;
    left: 0;
    top: 0;
    width: 100%;
    height: 100%;
    overflow: auto;
    background-color: rgb(0,0,0);
    background-color: rgba(0,0,0,0.4);
}
.modal-content {
    background-color: #fefefe;
    margin: auto;
    padding: 20px;
    border: 1px solid #888;
    width: 50%;
}
</style>`;
}
}
customElements.define('simple-form-modal-component',
SimpleFormModalComponent);
```

We have a getTemplate() method that returns the HTML for our component, a getStyle() method that returns the styles rules that we are using inside the component, some methods that we are adding in the component to handle some logic in the component, and some setter and getters that update the properties in the component. We can use the same principles to create a LitElement quickly and make our code shorter, because LitElement handles some low-level things for us.

We can start with the properties using the LitElement syntax, as in Listing 8-18.

### Listing 8-18. Adding Properties in SimpleFormModalComponent with LitElement

```js
import {LitElement, html, css} from 'lit-element';

export class SimpleFormModalComponent extends LitElement {
static get properties() {
    return {
        open: {
            type: Boolean
        }
    };
}

    constructor() {
        super();
        this.open = false;
    }
}

window.customElements.define('simple-form-modal-component',
SimpleFormModalComponent);
```

Here, we are adding our 'open' property as a Boolean, and we are initializing this property as false in the constructor().

Next, we are going to take the method getTemplate() and migrate in LitElement, as in Listing 8-19.

### Listing 8-19. Adding render() in SimpleFormModalComponent with LitElement

```js
import {LitElement, html, css} from 'lit-element';

export class SimpleFormModalComponent extends LitElement {
static get properties() {
    return {
        open: {
            type: Boolean
        }
    };
}

constructor() {
    super();
    this.open = false;
}

render() {
    return html`
        <div id="myModal" class="modal">
            <div class="modal-content">
                <form id="myForm">
                    <label for="ftitle">Title:</label><br>
                    <input type="text" id="ftitle" name="ftitle"><br>
                    <label for="fdesc">Description:</label><br>
                    <textarea id="fdesc" name="fdesc" rows="4" cols="50">
                    </textarea><br/>
                    <button type="button" id="addBtn">Add</button>
                    <button type="button" id="closeBtn">Close</button>
                </form>
            </div>
        </div>
    `;
}
}

window.customElements.define('simple-form-modal-component',
SimpleFormModalComponent);
```
