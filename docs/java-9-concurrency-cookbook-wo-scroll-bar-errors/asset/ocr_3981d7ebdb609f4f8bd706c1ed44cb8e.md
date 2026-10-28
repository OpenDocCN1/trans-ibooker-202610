# See also

+   - The *Waiting for multiple concurrent events* recipe in this chapter

# Running concurrent-phased tasks

One of the most complex and powerful functionalities offered by the Java concurrency API is the ability to execute concurrent-phased tasks using the `Phaser` class. This mechanism is useful when we have some concurrent tasks divided into steps. The `Phaser` class provides us with a mechanism to synchronize threads at the end of each step, so no thread will start with the second step until all the threads have finished the first one.

As with other synchronization utilities, we have to initialize the `Phaser` class with the number of tasks that participate in the synchronization operation, but we can dynamically modify this number by either increasing or decreasing it.

In this recipe, you will learn how to use the `Phaser` class to synchronize three concurrent tasks. The three tasks look for files with the extension `.log` modified in the last 24 hours in three different folders and their subfolders. This task is divided into three steps:

+   1. Get a list of the files with the extension `.log` in the assigned folder and its subfolders.
+   2. Filter the list created in the first step by deleting the files modified more than 24 hours ago.
+   3. Print the results in the console.

At the end of step 1 and step 2, we check whether the list has any elements or not. If it doesn't, the thread ends its execution and is eliminated from the `Phaser` class.

# Getting ready

The example of this recipe has been implemented using the Eclipse IDE. If you use Eclipse or a different IDE, such as NetBeans, open it and create a new Java project.