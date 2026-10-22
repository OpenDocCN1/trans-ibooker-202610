inputs to a given function or method returns an expected result. Boundary conditions are often tested as well, to ensure the function or method can handle these types of conditions appropriately.

# NOTE

A unit of code can technically be a single line, multiple lines of code, or even an entire code library. For our purposes, a unit refers to one or more lines of code, or an entire code library, that can be accessed through a single interface that is available on the local system - that is, a function or a method which encapsulates the unit’s functionality and can be called from your test application. This functionality can be, for example, a sorting algorithm, a calculation, or even an entry point to one or more additional functions or methods.

I use JUnit for unit testing Java code (included with Eclipse), and PyUnit for unit testing Python code (part of the standard Python interpreter, and available within PyDev). You don’t have to install any additional components to write and execute unit tests within your IDE if you’re using Eclipse and PyDev.