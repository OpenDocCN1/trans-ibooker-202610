
```
id="text-description"
outlined
minlength="3"
label="Description"
required>
</mwc-textfield>
</div>
<div>
<mwc-button
id="primary-action-button"
slot="primaryAction"
@click="handleAddNote">
Add
</mwc-button>
<mwc-button
slot="secondaryAction"
dialogAction="close"
@click="handleClose">
Cancel
</mwc-button>
</div>
</mwc-dialog>
</div>
</template>
<script>
import '@material/mwc-list/mwc-list';
import '@material/mwc-list/mwc-list-item';
import '@material/mwc-fab';
import '@material/mwc-button';
import '@material/mwc-dialog';
import '@material/mwc-textfield';
import { notesData } from '../utils/DummyData';
import { v4 as uuidv4 } from 'uuid';
export default {
    name: 'Dashboard',
    data() {
        return {
            notes: notesData
        }
    },
    methods: {
        handleDelete(id) {
            const noteToDelete = this.notes.findIndex((item) => (item.id === id));
            this.notes.splice(noteToDelete, 1);
        },
        handleAdd() {
            const formDialog = this.$el.querySelector('#dialog');
            formDialog.show();
        },
        handleAddNote() {
            const formDialog = this.$el.querySelector('#dialog');
            let txtTitle = this.$el.querySelector('#text-title');
            let txtDescription = this.$el.querySelector('#text-description');
            const isValid = txtTitle.checkValidity() && txtDescription.
            checkValidity();

            if(isValid) {
                const newIndex = uuidv4();
                this.notes.push({id: newIndex, title: txtTitle.value, description:
                txtDescription.value});

                txtTitle.value ='';
                txtDescription.value = '';
                formDialog.close();
            }
        },
    }
handleClose() {
  let txtTitle = this.$el.querySelector('#text-title');
  let txtDescription = this.$el.querySelector('#text-description');
  const formDialog = this.$el.querySelector('#dialog');

  txtTitle.value = '';
  txtDescription.value = '';
  formDialog.close();
}
},
}
</script>
<style scoped>
.floatButton {
  position: fixed;
  bottom: 20px;
  right: 20px;
}

.formFields {
  margin: 15px;
}
</style>
```

Here, we are using this.$el.querySelector() to select the web components and use their methods to execute logic, such as, for example, formDialog.show() and formDialog.close(), to show and hide the mwc-dialog. Also, in handleAddNote(), we are using the uuidv4() method to generate a new index with the data that the user enters in the form and, with this.notes.push(), add the new note in the local array that we are using to keep all our notes. With these modifications, you can add notes (see Figure 9-22).

![](img/e276a43da8123ca0974f10d5c85948b9_188_0.png)

## Figure 9-22. Adding new notes

You can access the relevant code from the GitHub repository at `$git checkout v1.0.6`.

## CHALLENGE EXERCISE

Add an action button and the functionality in our app to edit a note.

- Add a new button, Edit, similar to Delete.
- When a user clicks Edit, the app is going to open a dialog window similar to New Note but with the information from the note selected.
- The user can make changes and save the data again, and the note should be updated.

## Adding Firebase

If you try to reload the app, you can see that we lost all our notes. We need an external persistence system that keeps our data and syncs this between all our clients.

The Firebase database gives us a perfect solution for keeping sync with our data in real time for all our clients, and we can save the data easily with Firebase’s JavaScript software development kit (SDK).

To get started, go to firebase.google.com and use your Google account to log in. Next, go to the console (Figure 9-23).

![](img/e276a43da8123ca0974f10d5c85948b9_189_0.png)

Figure 9-23. Firebase console link

Create a new project (Figure 9-24).

![](img/e276a43da8123ca0974f10d5c85948b9_190_0.png)

Figure 9-24. Adding the Project button

Next, you are going to choose the name of your project (Figure 9-25).

![](img/e276a43da8123ca0974f10d5c85948b9_190_1.png)

Figure 9-25. Creating a new project in Firebase

Choose whether you want to use Google Analytics integration in your project (Figure 9-26).

![](img/e276a43da8123ca0974f10d5c85948b9_191_0.png)

## Figure 9-26. Adding Google Analytics to the new Firebase project

In just minutes, you can start to use your new project.

When your project is ready in the Firebase console, you are going to require some information that allows you to connect our app and Firebase.

For this, go to Project Overview ► Project settings. (See Figure 9-27.)

![](img/e276a43da8123ca0974f10d5c85948b9_192_0.png)

## Figure 9-27. Firebase Project Overview

Now click the web app icon (Figure 9-28).

![](img/e276a43da8123ca0974f10d5c85948b9_192_1.png)

## Figure 9-28. Firebase project settings view

Firebase is going to start a wizard (Figure 9-29).

![](img/e276a43da8123ca0974f10d5c85948b9_193_0.png)

## Figure 9-29. Firebase web app wizard

At the end, you will see our firebaseConfig summary (Figure 9-30).

![](img/e276a43da8123ca0974f10d5c85948b9_193_1.png)

## Figure 9-30. Firebase configuration summary

Copy this information. You will need it when we create the firebase.js file for our project.

The last thing that we need to make in the console is a new database and the security rules to access it without authentication. (We do this to keep our app simple.) Go to Database (Figure 9-31).

![](img/e276a43da8123ca0974f10d5c85948b9_194_0.png)

Figure 9-31. Firebase Database link

Select Create Realtime Database (Figure 9-32).

![](img/e276a43da8123ca0974f10d5c85948b9_195_0.png)

## Figure 9-32. Firebase security rules

Select Start in test mode. In this mode, we can write data in our database without having authentication. This is handy in development but unsafe in production. Now we can go back to our app.

In the terminal run the following:

```
$npm install firebase --save
```

Create a new firebase.js file and paste the data from your Firebase Project, as in Listing 9-33.

## Listing 9-33. Adding Firebase

```
import Firebase from 'firebase';

let config = {
    apiKey: "xxx",
```
```
authDomain: "xxx",
databaseURL: "xxx",
projectId: "xxx",
storageBucket: "xxx",
messagingSenderId: "xxx",
appId: "xxx"
};
```
```
Firebase.initializeApp(config)
```
```
export const fireApp = Firebase;
```

Import main.js, as shown in Listing 9-34.

## Listing 9-34. Importing Firebase in main.js

```
import Firebase from 'firebase';

import './firebase';
import { createApp } from 'vue'
import App from './App.vue'
import router from './router'

createApp(App).use(router).mount('#app')...
```

You can access the relevant code from the GitHub repository at $git checkout v1.0.7.

Now, in Dashboard.vue, we must use the life cycle mounted() method and recover all the notes in our real-time database. We must also update saveNote() and deleteNote(), to update the new notes in Firebase.

With the following, we import Fireapp from firebase.js to keep a reference in our app:

```
const db = fireApp.database().ref();
```

Now, with db.push, we add data to Firebase, and with remove(), we can delete data from Firebase (Listing 9-35). For additional information, you can access the relevant documents at https://firebase.google.com/docs/reference/js/firebase.database.

## Listing 9-35. Adding Firebase in Dashboard.vue

```
<!-- eslint-disable vue/no-deprecated-slot-attribute -->
<template>
    <div>
        <mwc-list v-for="(note) in notes" :key="note.id" multi>
            <mwc-list-item twoline hasMeta>
                <span>{{note.title}}</span>
                <span slot="meta" class="material-icons" @click="handleDelete
                (note.id)">delete</span>
                <span slot="secondary">{{note.description}}</span>
            </mwc-list-item>
            <li divider padded role="separator"></li>
        </mwc-list>
        <mwc-fab class="floatButton" @click="handleAdd" mini icon="add">
        </mwc-fab>
        <mwc-dialog id="dialog" heading="Add Note">
            <div class="formFields">
                <mwc-textfield
                    id="text-title"
                    outlined
                    minlength="3"
                    label="Title"
                    required>
                </mwc-textfield>
            </div>
            <div class="formFields">
                <mwc-textfield
                    id="text-description"
                    outlined
                    minlength="3"
                    label="Description"
                    required>
                </mwc-textfield>
            </div>
        </mwc-dialog>
```
