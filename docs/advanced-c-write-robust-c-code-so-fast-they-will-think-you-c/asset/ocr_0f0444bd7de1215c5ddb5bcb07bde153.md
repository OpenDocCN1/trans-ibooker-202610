A debugger is a better tool to fight runtime errors. A debugger can let you run code line by line, continue running and pause on the lines that you want, investigate the values of memory, and pause on errors, among other things. This lets you watch what exactly is going on with memory as your program is running and identify the line of code that results in the unwanted behavior.

gdb is the canonical command-line debugger that can debug C++ programs. However, it may be difficult to use as debugging is inherently a visual task—you want to be able to look at lines of code, values of variables, and the output of the program at the same time. Luckily, Eclipse CDT includes a visual debugger that is easy to use.

# Exercise 9: Debugging with Eclipse CDT

You have been simply running your projects and viewing the output. Now you want to learn how to debug your code in detail. In this exercise, we will explore Eclipse CDT's debugging capabilities. Perform the following steps to complete the exercise:

+   1. Open the CMake project in Eclipse CDT.
+   2. To ensure that we have an existing run configuration, click Run | Run Configurations. There, you should see a CxxTemplate entry under C/C++ Application.
+   3. Close the dialog box to continue.

> Note
> Since we ran our project before, it should be there. If not, please go back and create it again.