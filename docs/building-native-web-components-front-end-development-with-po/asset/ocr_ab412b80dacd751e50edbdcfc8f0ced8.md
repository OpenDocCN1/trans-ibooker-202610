```
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

set notes(newValue) {
    this._notes = newValue;
    this.root.innerHTML = this.render();
}
}
customElements.define('note-list-component', NoteListComponent);
```

In addition, we are using a map() in the render method, to iterate in the object notes and create each item that we need, and updating the Shadow DOM with the list of items. Remember that we are receiving a delEvent event each time that we need to remove an item from the list. Therefore, we must handle this behavior here, adding a listener for that event and eliminating the element from the list, as in Listing 6-4.

# Listing 6-4. Adding a Listener in NoteListComponent

```
class NoteListComponent extends HTMLElement {
    constructor() {
        super();
        this._notes = JSON.parse(this.getAttribute('notes')) || [];
        this.root = this.attachShadow({mode: 'open'});
        this.root.innerHTML = this.render();
        this.handleDelEvent = this.handleDelEvent.bind(this);
    }
}
```