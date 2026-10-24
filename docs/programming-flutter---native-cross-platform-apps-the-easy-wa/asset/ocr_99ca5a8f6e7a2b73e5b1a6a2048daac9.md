to the iOS `Podfile` you should have from the CocoaPods configuration that was needed to connect the iOS app to Firebase.

**Using the `firebase_auth` Package**

Let's import the `firebase_auth` package, which provides an intuitive Flutter interface to the Firebase Authentication tools:

```
firebase/chatonfire/pubspec.yaml

firebase_auth: `^0.6.2`
```

As always, we need to import the `firebase_auth` package in `main.dart`:

```
firebase/chatonfire/lib/main.dart

`import` 'package:firebase_auth/firebase_auth.dart';
```

# The `FirebaseAuth`

Firebase authentication is achieved using an instance of the `FirebaseAuth` class, obtained using:

```
`var` auth = FirebaseAuth.instance;
```

After this, we can use the `auth` object to handle most authentication-related actions.

Among them, we find:

createUserWithEmailAndPassword(email, password)

which creates a user with the given email and password, and:

signInWithEmailAndPassword(email, password)

which signs in the user with the given email and password.

> **The Many Authentication Methods Available in Flutter**
> 
> There are many authentication methods available to use with Firebase. Here is a list of the most common ones: