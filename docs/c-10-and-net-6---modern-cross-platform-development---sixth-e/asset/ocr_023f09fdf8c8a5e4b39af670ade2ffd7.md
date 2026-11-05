# Nested and child tasks

As well as defining dependencies between tasks, you can define nested and child tasks. A **nested task** is a task that is created inside another task. A **child task** is a nested task that must finish before its parent task is allowed to finish.

Let's explore how these types of tasks work:

+   1. Use your preferred code editor to add a new console app to the Chapter12 solution/workspace named NestedAndChildTasks.
+   2. In Visual Studio Code, select NestedAndChildTasks as the active OmniSharp project.
+   3. In Program.cs, delete the existing statements, statically import Console, and then add two methods, one of which starts a task to run the other, as shown in the following code:
+   4. Above the methods, add statements to start a task to run the outer method and wait for it to finish before stopping, as shown in the following code:
+   5. Run the code and view the result, as shown in the following output:

```
static void OuterMethod()
{
    WriteLine("Outer method starting...");
    Task innerTask = Task.Factory.StartNew(InnerMethod);
    WriteLine("Outer method finished.");
}

static void InnerMethod()
{
    WriteLine("Inner method starting...");
    Thread.Sleep(2000);
    WriteLine("Inner method finished.");
}
```

```
Task outerTask = Task.Factory.StartNew(OuterMethod);
outerTask.Wait();
WriteLine("Console app is stopping.");
```

```
Outer method starting...
Inner method starting...
Outer method finished.
Console app is stopping.
```

Note that, although we wait for the outer task to finish, its inner task does not have to finish as well. In fact, the outer task might finish, and the console app could end, before the inner task even starts!