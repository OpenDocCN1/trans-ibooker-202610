In the index.html, we are adding a button that is going to show the modal when the user must add a new note, and we are going to create app.js a small logic for the index.html that is going to connect all the pieces (see Listing 6-14).

# Listing 6-14. Creating app.js

```
```js
import { notesDataApi } from './utils/notes-data-api.js';
const formModal = document.querySelector('simple-form-modal-component');
const noteList = document.querySelector('note-list-component');

notesDataApi.getTasks(1)
  .then((res) => res.json())
  .then((items) => {
    const allNotes = items.map((item)=>({title: item.title, description:
    item.body}));
    noteList.notes = allNotes;
  });

formModal.addEventListener('addEvent', function(e) {
  let notes = noteList.notes;

  notes.push({"title": e.detail.title, "description": e.detail.
  description});
  noteList.notes = notes;
});

const myBtn = document.getElementById('myBtn');
myBtn.addEventListener('click', function() {
  formModal.open = !formModal.open;
});
```
```

Here, we are adding the login for the button that we add in the index.html. This button is going to pass a state to the SimpleFormModalComponent, to show or hide the modal. We are adding a listener for the custom event addEvent, to get the data for the new note and pass this data to the NoteListComponent. We are using the note-data-api module to get all the notes that we have in the API and send this data to the NoteListComponent, to fill dummy notes by default. Finally, we are going to add a style file, to improve the appearance of our button in index.html, as in Listing 6-15.