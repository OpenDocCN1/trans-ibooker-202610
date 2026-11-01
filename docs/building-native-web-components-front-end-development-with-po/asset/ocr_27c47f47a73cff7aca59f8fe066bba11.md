Here, we are using a route meta field in /Dashboard to enforce the authentication in this route, and we are checking this in the guard. Also, we are checking for the getCurrentUser() method from fireApp. We must add this method in firebase.js (Listing 10-5).

# Listing 10-5. Adding the getCurrentUser() Method in firebase.js

```
import Firebase from 'firebase';

let config = {
  apiKey: "xxx",
  authDomain: "xxx",
  databaseURL: "xxx",
  projectId: "xxx",
  storageBucket: "xxx",
  messagingSenderId: "xxx",
  appId: "xxx"
};

Firebase.initializeApp(config)

Firebase.getCurrentUser = () => {
  return new Promise((resolve, reject) => {
    const unsubscribe = Firebase.auth().onAuthStateChanged(user => {
      unsubscribe();
      resolve(user);
    }, reject);
  })
};
export const fireApp = Firebase;
```

The getCurrentUser method gives us information about the current user or a rejection. Finally, we are going to add a logout link at the top, for users to close their sessions.

In App.vue, we are going to add a link in <mwc-top-app-bar> (Listing 10-6).