In this example we create a dog named Willie and a dog named Lucy. Each dog is a separate instance with its own set of attributes, capable of the same set of actions:

```
My dog's name is Willie.
My dog is 6 years old.
Willie is now sitting.

Your dog's name is Lucy.
Your dog is 3 years old.
Lucy is now sitting.
```

Even if we used the same name and age for the second dog, Python would still create a separate instance from the Dog class. You can make as many instances from one class as you need, as long as you give each instance a unique variable name or it occupies a unique spot in a list or dictionary.

# TRY IT YOURSELF

+   **9-1. Restaurant:** Make a class called `Restaurant`. The `__init__()` method for `Restaurant` should store two attributes: a `restaurant_name` and a `cuisine_type`. Make a method called `describe_restaurant()` that prints these two pieces of information, and a method called `open_restaurant()` that prints a message indicating that the restaurant is open.
+   
+   Make an instance called `restaurant` from your class. Print the two attributes individually, and then call both methods.
+   
+   **9-2. Three Restaurants:** Start with your class from Exercise 9-1. Create three different instances from the class, and call `describe_restaurant()` for each instance.
+   
+   **9-3. Users:** Make a class called `User`. Create two attributes called `first_name` and `last_name`, and then create several other attributes that are typically stored in a user profile. Make a method called `describe_user()` that prints a summary of the user’s information. Make another method called `greet_user()` that prints a personalized greeting to the user.
+   
+   Create several instances representing different users, and call both methods for each user.