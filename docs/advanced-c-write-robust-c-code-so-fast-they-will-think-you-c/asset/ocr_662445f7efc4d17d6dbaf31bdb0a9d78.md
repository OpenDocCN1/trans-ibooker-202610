Our code has transferred program control to the catch() statement in the main() function and has executed the destructors for all the variables that had been constructed on the stack (from the time the call was made in the try clause).

# Stack Unwinding

The process of destroying all local function variables, as guaranteed by the C++ language is known as Stack Unwinding. As the stack unwinds in the presence of an exception, C++ uses its well-defined rules to destroy all the objects in the scope.

When an exception occurs, the function call stack starts searching linearly from the current function back to the function that called it, to the one that called that, and so on, until an exception handler (expressed by a catch block) that matches the exception is found.

If an exception handler is found, then the stack unwinding occurs, destroying all the local variables for all the functions in the stack. The objects are destroyed in the reverse order that they were created. If no handler is found to deal with the thrown exception, then the program terminates (usually without warning the user).

# Exercise 1: Implementing exceptions in Fraction and Stack

In this exercise, we will return to two classes that we worked on in Chapter 2A, No Ducks Allowed – Types and Deduction and Chapter 2B, No Ducks Allowed – Templates and Deduction – Fraction and Stack, both of which can experience runtime anomalies. We are going to update their code so that they can raise exceptions whenever any problem is detected. Follow these steps to implement this exercise:

+   1. Open Eclipse and create a new project named Lesson3 using the files found in the Lesson3 examples folder.
+   2. As this is a CMake-based project, change the current builder to be CMake Build (portable).
+   3. Go to the Project | Build All menu to build all the exercises. By default, the console at the bottom of the screen will display the CMake Console [Lesson3].
+   4. Configure a new Launch Configuration, L3Exercise1 to run with the name Exercise1.
+   5. Also, configure a new C/C++ Unit Run Configuration, L3Ex1Tests, to run L3Ex1tests. Set the Google Tests Runner.