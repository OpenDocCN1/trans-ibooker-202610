# Runtime environments

Cloud Functions are executed in a fully managed environment. The infrastructure and software that's needed to run the function are handled for you. Each function is single-threaded and is run in an isolated environment with the intended context. You don't need to take care of any updates for that environment. They are auto-updated for you and scaled as needed.

Currently, several runtimes are supported by Cloud Functions, namely the following:

+   - Node.js 10, 12, and 14
+   - Python 3.7, 3.8, and 3.9
+   - Go 1.11 and 1.13
+   - Java 11
+   - .NET Core 3.1
+   - Ruby 2.6 and 2.7
+   - PHP 7.4

When you define a function, you can also define the requirements or dependencies file in which you state which modules or libraries your function is dependent on. However, remember that those libraries will be loaded when your function is executed. This causes delays in terms of execution. We will talk about this in more detail in the Cold start section of this chapter.

In the next section, we will look at types of Cloud Functions.

# Types of Cloud Functions

There are two types of Cloud Functions: HTTP functions and background functions. They differ in the way they are triggered. Let's have a look at each.