
```
set idx(newValue) {
    this._idx = newValue;
}

handleDelete() {
    this.dispatchEvent(new CustomEvent('delEvent', {bubbles: true, detail:
    {idx: this.idx}}));
}
customElements.define('note-list-item-component', NoteListItemComponent);
```

We are going to add the template of this component and the styles, to get a nice item for the notes (see Listing 6-6).

## Listing 6-6. Adding the Template and Styles in NoteListItemComponent

```
class NoteListItemComponent extends HTMLElement {

    constructor() {
        super();

        this._note = JSON.parse(this.getAttribute('note')) || {};
        this.idx = this.getAttribute('idx') || -1;
        this.root = this.attachShadow({mode: 'open'});
        this.root.innerHTML = this.getTemplate();
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

set idx(newValue) {
    this._idx = newValue;
}

handleDelete() {
    this.dispatchEvent(new CustomEvent('delEvent', {bubbles: true, detail:
    {idx: this.idx}}));
}

getStyle() {
    return `
<style>
.note {
    background-color: #ffffcc;
    border-left: 6px solid #ffeb3b;
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
    <p><strong>${this._note.title}</strong> ${this._note.description}</p>
    <br/>
    <button type="button" id="deleteButton">Delete</button>
</div>`;
}
}
customElements.define('note-list-item-component', NoteListItemComponent);
```

With these enhancements, our <note-list-item-component> is going to look like Figure 6-6.

## Figure 6-6. note-list-item component in Google Chrome

![](img/e276a43da8123ca0974f10d5c85948b9_81_0.png)

Finally, we are going to add an event listener for the Delete button that triggers the delEvent event. As with NoteListComponent, we must add the listener in connectedCallback() and remove it in disconnectedCallback(), as shown in Listing 6-7.

## Listing 6-7. Adding an Event Listener in NoteListItemComponent

```
class NoteListItemComponent extends HTMLElement {
    constructor() {
        super();

        this._note = JSON.parse(this.getAttribute('note')) || {};
        this.idx = this.getAttribute('idx') || -1;
        this.root = this.attachShadow({mode: 'open'});
        this.root.innerHTML = this.getTemplate();
        this.handleDelete = this.handleDelete.bind(this);
    }

    connectedCallback() {
        this.delBtn.addEventListener('click', this.handleDelete);
    }

    disconnectedCallback () {
        this.delBtn.removeEventListener('click', this.handleDelete);
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

set idx(newValue) {
    this._idx = newValue;
}

handleDelete() {
    this.dispatchEvent(new CustomEvent('delEvent', {bubbles: true,
    detail: {idx: this.idx}}));
}

getStyle() {
    return `
<style>
.note {
    background-color: #ffffcc;
    border-left: 6px solid #ffeb3b;
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
    <button type="button" id="deleteButton">Delete</button>
</div>`;
}
}
customElements.define('note-list-item-component', NoteListItemComponent);
```

Now the NoteListItemComponent is going to send the custom event when a user clicks on the Delete button, and NoteListComponent will know what the item that must be eliminated from the list is.

## SimpleFormModalComponent

With NoteListComponent and NoteListItemComponent ready, we now require an easy way to add new notes in our app. That is why we are going to create the SimpleFormModalComponent, a form that is going to allow users to input a title and description. This component is going to communicate with app.js, with which we are going to use an open property, to know when to show or hide the modal and when the user inserts data in the form. We are going to pass that data with the custom event addEvent, as shown in Figure 6-7.

![](img/e276a43da8123ca0974f10d5c85948b9_83_0.png)

Figure 6-7. Identifying communication between simple-form-modal-component and app.js

We are going to start defining our component, adding setters and getters, as in Listing 6-8.

## Listing 6-8. Adding Getters and Setters in SimpleFormModalComponent

```
class SimpleFormModalComponent extends HTMLElement {
    constructor() {
        super();

        this.root = this.attachShadow({mode: 'open'});
        this.container = document.createElement('div');

this.container.innerHTML = this.getTemplate();
this.root.appendChild(this.container.cloneNode(true));

this._open = this.getAttribute('open') || false;
}
get open() {
    return this._open;
}

set open(newValue) {
    this._open = newValue;
}
}
customElements.define('simple-form-modal-component',
SimpleFormModalComponent);
```

Now we are going to add the template and styles required to show and hide the component (see Listing 6-9).

## Listing 6-9. Adding the Template and Styles in SimpleFormModalComponent

```
class SimpleFormModalComponent extends HTMLElement {

    constructor() {
        super();
        this.root = this.attachShadow({mode: 'open'});
        this.container = document.createElement('div');
        this.container.innerHTML = this.getTemplate();
        this.root.appendChild(this.container.cloneNode(true));

        this._open = this.getAttribute('open') || false;
    }
    get open() {
        return this._open;
    }
    set open(newValue) {
        this._open = newValue;
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
            <button type="button" id="addBtn">Add</button><button type=
            "button" id="closeBtn">Close</button>
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
.close {
  color: #aaaaaa;
  float: right;
  font-size: 28px;
  font-weight: bold;
}
.close:hover,
.close:focus {
  color: #000;
  text-decoration: none;
  cursor: pointer;
}
</style>`;
}
}
customElements.define('simple-form-modal-component',
SimpleFormModalComponent);
```

With this enhancement, we are going to show a form with two inputs and two buttons. The modal class will place the element on top of everything and show it in the middle of the window with a transparent gray background. Also, because the default display is "none", that means that we are going to hide this element and make it visible when we set that property to "block". To handle this behavior, we are going to create the method showModal() and trigger it in the setter of the open property, as in Listing 6-10.

## Listing 6-10. Adding showModal() in SimpleFormModalComponent

```
class SimpleFormModalComponent extends HTMLElement {
  constructor() {
    super();

    this.root = this.attachShadow({mode: 'open'});
    this.container = document.createElement('div');

this.container.innerHTML = this.getTemplate();
this.root.appendChild(this.container.cloneNode(true));

this._open = this.getAttribute('open') || false;
this.modal = this.root.getElementById("myModal");
}
get open() {
    return this._open;
}
set open(newValue) {
    this._open = newValue;
    this.showModal(this._open);
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
            <button type="button" id="addBtn">Add</button><button
            type="button" id="closeBtn">Close</button>
        </form>
    </div>
</div>`;
}
```
