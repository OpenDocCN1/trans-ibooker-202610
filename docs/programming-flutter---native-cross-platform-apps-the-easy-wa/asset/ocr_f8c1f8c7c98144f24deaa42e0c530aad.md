```
Firestore.instance.collection("Messages").add(
  {
    "from": "Anonymous",
    "when": DateTime.now().toUtc(),
    "msg": "Hi There From The App",
  }
);
```

In a real app we need to authenticate users, so we’ll start by discovering how we can authenticate users to our Flutter apps using Firebase Authentication.

# Deleting or Changing Data from the Cloud Firestore

Given a DocumentReference (the object we get from Firestore.instance.collection(collection).document(document)), we can, in addition to getting the data using the DocumentReference.get as we said earllier, perform some actions on the document:

+   - Delete it using DocumentReference.delete.
+   - Change some fields by passing a Map to DocumentReference.updateData.
+   - Set the contents of the document, creating it if it doesn’t exist, by passing a Map to DocumentReference.setData.

All of these methods run asynchronously but don’t return anything.

# AUTHENTICATION

Authentication requires its own firebase_auth package[49] and can be configured in Flutter’s console by going to Develop -> Authentication -> Sign-in method. In our example we’ll authenticate users using their email address and password, which is made very easy by Firebase and is representative of the majority of apps. After selecting it, you’ll be warned that more platform-specific setup is required: specifically, we need to add:

```
implementation 'com.google.firebase:firebase-auth:16.2.1'
```

to the app-level build.gradle dependencies for Android, and:

```
pod 'Firebase/Auth'
```