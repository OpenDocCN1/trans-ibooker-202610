# Grouping threads and processing uncontrolled exceptions in a group of threads

An interesting functionality offered by the concurrency API of Java is the ability to group threads. This allows us to treat the threads of a group as a single unit and provide access to the thread objects that belong to a group in order to do an operation with them. For example, you have some threads doing the same task and you want to control them. You can, for example, interrupt all the threads of the group with a single call.

Java provides the ThreadGroup class to work with a groups of threads. A ThreadGroup object can be formed by thread objects and another ThreadGroup object, generating a tree structure of threads.

In the Controlling the interruption of a thread recipe, you learned how to use a generic method to process all the uncaught exceptions that are thrown in a thread object. In the Processing uncontrolled exceptions in a thread recipe, we wrote a handler to process the uncaught exceptions thrown by a thread. We can use a similar mechanism to process the uncaught exceptions thrown by a thread or a group of threads.

In this recipe, we will learn to work with ThreadGroup objects and how to implement and set the handler that would process uncaught exceptions in a group of threads. We'll do this using an example.

# Getting ready

The example for this recipe has been implemented using the Eclipse IDE. If you use Eclipse or a different IDE, such as NetBeans, open it and create a new Java project.