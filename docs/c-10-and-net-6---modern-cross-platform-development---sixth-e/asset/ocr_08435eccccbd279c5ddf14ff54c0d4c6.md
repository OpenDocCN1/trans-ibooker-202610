# Unit testing

Fixing bugs in code is expensive. The earlier that a bug is discovered in the development process, the less expensive it will be to fix.

Unit testing is a good way to find bugs early in the development process. Some developers even follow the principle that programmers should create unit tests before they write code, and this is called **Test-Driven Development (TDD)**.

Microsoft has a proprietary unit testing framework known as **MS Test**. There is also a framework named **NUnit**. However, we will use the free and open-source third-party framework **xUnit.net**. xUnit was created by the same team that built NUnit but they fixed the mistakes they felt they made previously. xUnit is more extensible and has better community support.

# Understanding types of testing

Unit testing is just one of many types of testing, as described in the following table:

| Type of testing | Description |
|---|---|
| Unit | Tests the smallest unit of code, typically a method or function. Unit testing is performed on a unit of code isolated from its dependencies by mocking them if needed. Each unit should have multiple tests: some with typical inputs and expected outputs, some with extreme input values to test boundaries, and some with deliberately wrong inputs to test exception handling. |
| Integration | Tests if the smaller units and larger components work together as a single piece of software. Sometimes involves integrating with external components that you do not have source code for. |
| System | Tests the whole system environment in which your software will run. |
| Performance | Tests the performance of your software; for example, your code must return a web page full of data to a visitor in under 20 milliseconds. |
| Load | Tests how many requests your software can handle simultaneously while maintaining required performance, for example, 10,000 concurrent visitors to a website. |
| User Acceptance | Tests if users can happily complete their work using your software. |

# Creating a class library that needs testing

First, we will create a function that needs testing. We will create it in a class library project. A class library is a package of code that can be distributed and referenced by other .NET applications:

+   1. Use your preferred coding tool to add a new **Class Library** to the Chapter04 workspace/solution named CalculatorLib. The `dotnet new` template is named `classlib`.
+   2. Rename the file named `Class1.cs` to `Calculator.cs`.