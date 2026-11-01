Here, we are using the option hasChanged() in 'open' to check when an update is triggered, and with the updated() method, we use the method showModal() to show/hide the modal. Now, our <simple-form-modal-component> is ready.

You can access the code for this book (https://github.com/carlosrojaso/apress-book-web-components) with

```
$git checkout chap-8.
```

I created an example in the dev/ folder, documentation in the docs-src/ folder, and some basic tests in the test/ folder, then you can run the following:

```
$ npm install
```

```
$ npm run serve
```

To see the example running in localhost:8080/dev, run

```
$ npm run docs
```

```
$ npm run docs:serve
```

We will continue with the <note-list-component> that we wrote in the previous chapters, as in Listing 8-23.

# Listing 8-23. NoteListComponent in VanillaJS

```
export class NoteListComponent extends HTMLElement {
static get observedAttributes() { return ['notes']; }

constructor() {
super();

this._notes = JSON.parse(this.getAttribute('notes')) || [];
this.root = this.attachShadow({mode: 'open'});
this.root.innerHTML = this.render();

this.handleDelEvent = this.handleDelEvent.bind(this);
}

attributeChangedCallback(name, oldValue, newValue) {
switch(name) {
case 'notes':
```