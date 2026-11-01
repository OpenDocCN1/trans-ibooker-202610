
## CHAPTER 6 COMPONENT ARCHITECTURE

Now we can take this initial mock-up and think about how we can split the elements into small pieces that will make our development easier and better oriented to components that we can use in the future.

In Figure 6-2, you can see that we have three main components (`simple-form-modal-component`, `note-list-component`, and `note-list-item-component`) that we can use inside the logic of `app.js` to achieve our goal of having a list of notes, the ability to remove notes, and allowance of the addition of new notes.

![](img/e276a43da8123ca0974f10d5c85948b9_71_0.png)

Figure 6-2. Identifying components in our app

With our components defined, we can think of a hierarchy of components (as in Figure 6-3), to see what the relationship among them is.

![](img/e276a43da8123ca0974f10d5c85948b9_72_0.png)

Figure 6-3. Identifying the hierarchy of components in our app

We can see that the main element is `app.js`. To it, we are going to add two sibling components, `simple-form-modal-component` and `note-list-component`, and inside of `note-list-component`, we will have several `note-list-item-component` elements that are children of `note-list-component`. Having a clear understanding of this relationship will help us with the other architectural decisions that we will be required to take in the next steps.

### Communicating Between Web Components

When we are working with components, usually we need a way to send and receive data between parents and children (as in Figure 6-4), to update or send notification changes that a user or other component made at some moment in the business logic of our app. To achieve this, we use the properties to get data and events to send data to the other components.

![](img/e276a43da8123ca0974f10d5c85948b9_73_0.png)

Figure 6-4. Identifying communication between components

In Figure 6-5, you can see that we are going to define passing data from the note-list-component using the idx and note attributes and receive data using the delEvent event.

![](img/e276a43da8123ca0974f10d5c85948b9_73_1.png)

Figure 6-5. Designing a communication mechanism between note-list-component and note-list-item-component

This means that in our NoteListComponent, we are going to create the <note-list-item-component> elements that we require and pass an object note and a number idx, as you can see in Listing 6-1.

### Listing 6-1. Defining NoteListComponent

```
class NoteListComponent extends HTMLElement {
    constructor() {
    }

    render() {
        return `
            <note-list-item-component note='${JSON.stringify(note)}'
            idx='${idx}'></note-list-item-component> ;
    }
}
customElements.define('note-list-component', NoteListComponent);
```

We are going to use JSON.stringify() to pass the object note correctly and JSON.parse() to receive that data in our child component. You can learn more about these methods at https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify and https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON/parse.

Now, in our NoteListItemComponent, we are going to create a custom event that NoteListComponent can listen to and that knows when the user wants to remove a note from the list, as shown in Listing 6-2.

### Listing 6-2. Adding a Custom Event in NoteListItemComponent

```
class NoteListItemComponent extends HTMLElement {
    constructor() {
    }

    handleDelete() {
    this.dispatchEvent(new CustomEvent('delEvent', {bubbles: true,
    detail: {idx: this.idx}}));
    }
}
customElements.define('note-list-item-component', NoteListItemComponent);
```

Here, we are creating a handleDelete() method that is using CustomEvent() to create an event and sending the data that we need in detail. In that way, the parent is going to know what item needs to be removed from the list. You can learn more about CustomEvent in https://developer.mozilla.org/en-US/docs/Web/API/CustomEvent/CustomEvent.

Usually, with properties and events, we can handle the communication between components in most of the scenarios on a small and medium scale. Still, if your app is more sophisticated and has complex interactions between components, you will require an event bus, which you can find at www.npmjs.com/package/js-event-bus, or a pattern such as Redux, which you can find at https://github.com/reduxjs/redux.

### NoteListComponent

In the preceding section, you learned how to make our web components communicate. Now we are going to build these components. First, the more natural way to handle our notes is to receive an array of notes and create one item for each element. Then, we are going to get the attribute notes and use the method JSON.parse() to convert this into an object. We are using _notes, to avoid conflicts with the setter in the component and render all the notes again, each time that they are updated (see Listing 6-3).

*Listing 6-3. getter and setter in NoteListComponent*

```js
class NoteListComponent extends HTMLElement {
  constructor() {
    super();

    this._notes = JSON.parse(this.getAttribute('notes')) || [];
    this.root = this.attachShadow({mode: 'open'});
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

set notes(newValue) {
    this._notes = newValue;
    this.root.innerHTML = this.render();
}
}
customElements.define('note-list-component', NoteListComponent);
```

In addition, we are using a map() in the render method, to iterate in the object notes and create each item that we need, and updating the Shadow DOM with the list of items. Remember that we are receiving a delEvent event each time that we need to remove an item from the list. Therefore, we must handle this behavior here, adding a listener for that event and eliminating the element from the list, as in Listing 6-4.

### Listing 6-4. Adding a Listener in NoteListComponent

```
class NoteListComponent extends HTMLElement {
    constructor() {
        super();
        this._notes = JSON.parse(this.getAttribute('notes')) || [];
        this.root = this.attachShadow({mode: 'open'});
        this.root.innerHTML = this.render();
        this.handleDelEvent = this.handleDelEvent.bind(this);
    }

connectedCallback() {
    this.root.addEventListener('delEvent', this.handleDelEvent);
}

disconnectedCallback () {
    this.root.removeEventListener('delEvent', this.handleDelEvent);
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

set notes(newValue) {
    this._notes = newValue;
    this.root.innerHTML = this.render();
}
}
customElements.define('note-list-component', NoteListComponent);
```

Here, we are adding the listener in connectedCallback() and removing the listener in disconnectedCallback(), to avoid unnecessary listeners when the component is removed. Also, we use bind() in the constructor, to indicate in the constructor that we are defining the handleDelEvent() and ensure that it doesn’t get undefined when we want to pass the method in our component. With these modifications, our component is complete.

### NoteListItemComponent

Now the NoteListComponent creates a list of <note-list-item-component> elements and passes a note object and an idx number that is equivalent to the position in the array of notes. We are going to add the getters and setters in NoteListItemComponent and initialize these properties in the constructor(). Remember that a note is an object, and with JSON.parse(), we are going to convert this data into an object (see Listing 6-5).

### Listing 6-5. Adding Getters and Setters in NoteListItemComponent

```
class NoteListItemComponent extends HTMLElement {
    constructor() {
        super();

        this._note = JSON.parse(this.getAttribute('note')) || {};
        this.idx = this.getAttribute('idx') || -1;
        this.root = this.attachShadow({mode: 'open'});
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
}
```
