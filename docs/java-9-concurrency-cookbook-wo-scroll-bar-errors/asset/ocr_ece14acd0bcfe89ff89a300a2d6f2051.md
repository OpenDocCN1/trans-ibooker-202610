When you execute the program, the exception is thrown, but the program doesn't stop. In the Main class you have included a call to the isCompletedAbnormally() method of the ForkJoinTask class using the original task. This method returns true if that task, or one of its subtasks, has thrown an exception. You also used the getException() method of the same object to get the Exception object that it has thrown.

When you throw an unchecked exception in a task, it also affects its parent task (the task that sent it to the ForkJoinPool class) and the parent task of its parent task, and so on. If you revise the entire output of the program, you'll see that there aren't output messages for the finalization of some tasks. The starting messages of those tasks are as follows:

```
Task: Starting form 0 to 100
Task: Starting form 0 to 50
Task: Starting form 0 to 25
Task: Starting form 0 to 12
Task: Starting form 0 to 6
```

These tasks are the ones that threw the exception and its parent tasks. All of them have finished abnormally. Take this into account when you develop a program with the ForkJoinPool and ForkJoinTask objects that can throw exceptions if you don't want this behavior.

The following screenshot shows part of an execution of this example:

![](bbox=[0.1, 0.52, 0.9, 0.72])