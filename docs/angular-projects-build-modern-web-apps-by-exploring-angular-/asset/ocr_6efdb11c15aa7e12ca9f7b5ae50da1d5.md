6. Firebase will generate a configuration that we will use later in the mobile application:

```
```javascript
var firebaseConfig = {
    apiKey: "<Your API key>",
    authDomain: "<Your project auth domain>",
    projectId: "<Your project ID>",
    storageBucket: "<Your storage bucket>",
    messagingSenderId: "<Your messaging sender ID>",
    appId: "<Your application ID>"
};
```
```

Take a note of the `firebaseConfig` object and click the **Continue to console** button.

> **Tip**
> 
> The Firebase configuration can also be accessed later at `https://console.firebase.google.com/project/<project-id>/settings/general` where project-id is the ID of your Firebase project.

7. Back in the dashboard console, select the **Cloud Firestore** option to enable Cloud Firestore in your application:

![](bbox=[0.257, 0.548, 0.762, 0.821])

Figure 6.15 – Enable Cloud Firestore