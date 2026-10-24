# Chapter 6

# Testing and Debugging
Flutter Apps

Up until now we have discussed how a Flutter app is built, with all the different UI, I/O, and state management features the framework offers. That’s all very nice when everything goes according to plan and the code does exactly what you think it will do, but that’s not always the case and testing can simplify the development process, especially on larger projects, by making sure each piece of code does what it’s supposed to.

It’s not the only thing we’re going to talk about in this chapter: when things go wrong in your app and there’s nothing the method you’re writing can do about it, exceptions are there to make sure you can notify the rest of your app that something has gone wrong, letting a calling function make a decision about what to do about the issue. This is especially necessary when apps make I/O operations that, as they operate on things outside the control of the user and/or the developer, are bound to fail at some point if the required conditions to execute the operation aren’t met.