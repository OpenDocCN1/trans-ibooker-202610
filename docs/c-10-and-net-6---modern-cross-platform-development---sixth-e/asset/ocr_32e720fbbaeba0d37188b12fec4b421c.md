When in break mode, there is also a useful window at the bottom of the edit area:

+   - **DEBUG CONSOLE** or **Immediate Window** enables live interaction with your code. You can interrogate the program state, for example, by entering the name of a variable. For example, you can ask a question such as, "What is 1+2?" by typing 1+2 and pressing Enter, as shown in Figure 4.9:

![](bbox=[0.125, 0.165, 0.891, 0.193])

Figure 4.9: Interrogating the program state

# Stepping through code

Let's explore some ways to step through the code using either Visual Studio or Visual Studio Code:

+   1. Navigate to **Run/Debug | Step Into**, or click on the **Step Into** button in the toolbar, or press *F11*. The yellow highlight steps forward one line.
+   2. Navigate to **Run/Debug | Step Over**, or click on the **Step Over** button in the toolbar, or press *F10*. The yellow highlight steps forward one line. At the moment, you can see that there is no difference between using **Step Into** or **Step Over**.
+   3. You should now be on the line that calls the Add method, as shown in Figure 4.10:

![](bbox=[0.125, 0.454, 0.891, 0.652])

Figure 4.10: Stepping into and over code

The difference between **Step Into** and **Step Over** can be seen when you are about to execute a method call:

+   - If you click on **Step Into**, the debugger steps *into* the method so that you can step through every line in that method.
+   - If you click on **Step Over**, the whole method is executed in one go; it does not skip over the method without executing it.