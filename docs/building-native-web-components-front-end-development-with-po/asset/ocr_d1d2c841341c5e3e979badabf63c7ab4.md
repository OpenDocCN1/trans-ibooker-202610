![](bbox=[0.07, 0.1, 0.89, 0.52])

# Figure 9-32. Firebase security rules

Select Start in test mode. In this mode, we can write data in our database without having authentication. This is handy in development but unsafe in production. Now we can go back to our app.

In the terminal run the following:

```
$npm install firebase --save
```

Create a new firebase.js file and paste the data from your Firebase Project, as in Listing 9-33.

# Listing 9-33. Adding Firebase

```
import Firebase from 'firebase';

let config = {
    apiKey: "xxx",
```