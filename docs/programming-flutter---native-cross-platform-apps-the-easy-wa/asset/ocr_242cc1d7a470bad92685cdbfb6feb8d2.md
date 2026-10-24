and a way to contact another user of the chat app via email since we’re already asking for that information.

# Creating a Collection for User Data

Let’s create another collection! We’ll call it *Users*, and it’ll be structured the following way:

+   - The user’s uid will be the document’s ID, to make it easier to access the document corresponding to each user.
+   - The user’s displayName will be a string called displayName.
+   - The user’s email address will be another string called email.
+   - The user’s bio will be stored as a string called bio.

# Changing the Documents and the App

We need to create entries to the user data collection for each existing user of our app and change every entry in the collection of messages and substitute the display name with the user ID. Then, we’ll worry about making the app work with the changed messages in the database, make it display the display name fetched from the Firestore, and then we’ll work on building a profile page.

# Changing the Data

Creating the users’ collection has to be done manually and it’s a good idea generally to have a collection containing user data: that’s how it is in most “traditional” databases and it’s worth it to replicate it in Firebase, except for the fact we don’t need (and shouldn’t, and especially not in plain text) to store passwords in the database we can access.

Let’s start with dealing with the changing data: to change each message in the database and substitute the values is easy enough when there aren’t many messages since we can just change them manually in the Firebase console, but with bigger databases you’d have to write some code that (using the interfaces we learned about in this chapter) fetches the messages, finds the user with the given display name and substitutes the display name with the user ID.

> **Making Smooth Transitions**
> 
> During initial development (and in our case) an app has no users, so we can make changes to the database that stop a previous version of the app from