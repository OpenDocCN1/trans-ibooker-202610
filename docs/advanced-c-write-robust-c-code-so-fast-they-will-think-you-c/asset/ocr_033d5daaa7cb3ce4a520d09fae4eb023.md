+   4. To start the debugger, find the toolbar entry that looks like an insect (bug) and click on the dropdown next to it. Select **CxxTemplate** to debug the main application. If it asks you to switch to the debug perspective, accept. Now, this is what Eclipse will look like:

![](bbox=[0.067, 0.181, 0.867, 0.643])

Figure 1.47: Eclipse debug screen

At the moment, our code froze at the very first line of our **main()** function, which is shown with the green highlight and the arrow in the center code view. On the left, we see the running threads, of which there is only one. On the right, we see the variables that are accessible in this context. On the bottom, we see the **gdb** output that Eclipse uses behind the scenes to actually debug the executable. Now, there is not much to be debugged with our main function.

+   5. Click **Step Over** either under the **Run** menu or in the toolbar a couple of times and see that the application will terminate soon. In the end, you will see the **libc-start.c** library, which is the caller of the **main** function. You can close it and switch to your source files when done. When you do not see the red stop button anymore, you know that the program execution is over.