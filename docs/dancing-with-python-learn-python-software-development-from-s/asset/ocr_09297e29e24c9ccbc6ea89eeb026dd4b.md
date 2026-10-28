# 1.8 Exceptions

In many science fiction stories, novels, movies, and television series, characters use “teleportation” to move people and objects from one place to another. For example, the protagonist could teleport from Toronto to Tokyo instantly, without going through the time-consuming hassle of air flight.

It is also used in such plots to allow characters to escape from dangerous and unexpected conditions. Perhaps a monster might have the team cornered in the back of a cave, and there is no way to fight their way to safety. A hurried call by the leader on a communications device might demand “get us out of here!” or “beam us up now!” The STAR TREK® series and movies often featured the latter exclamation.

So, characters use teleportation for regular transportation, but also for exceptional situations that are usually life-threatening. Many programming languages also have the concept and mechanism of exceptions.

In a loop or a well-written function, you can see how your code’s execution moves from one step to another. With an exception, you say, “try this, and if it doesn’t work out, come back here.” Or, you might be thinking, “I’m going about my business, but something may happen infrequently, and I need to handle it if it does.” The place in your code where you handle the exception is called its catch point.

Suppose you want to delete a file. You have its name, and you call a function to remove the file. The only problem is that the file does not exist. How do you handle this? One way is to use a condition: if the file exists, then delete it. Another way is to use an exception: try to delete the file, and if something goes wrong (for example, it doesn’t exist), raise an exception, and go back and decide what you should do about it.

Another good example of why you might raise an exception is division by zero. Something is seriously wrong in your code if it tries to do this, and the solution may not be obvious to you. Using an exception can help you find processing or code design errors.

An exception usually includes information that explains why it was raised. Perhaps we could not delete the file because it wasn’t present. Alternatively, maybe we didn’t have permission to delete it. When we catch the exception, we can see what happened and proceed accordingly.