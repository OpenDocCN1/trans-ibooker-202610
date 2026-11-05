6. You would have had to add the async keyword to the Main method and change its return type to Task. With .NET 6 and later, the console app project template uses the top-level program feature to automatically define the Program class with an asynchronous Main method for you.

7. Run the code and view the result, which is likely to have a different number of bytes since Apple changes its home page frequently, as shown in the following output:

```
Apple's home page has 40,252 bytes.
```

# Improving responsiveness for GUI apps

So far in this book, we have only built console applications. Life for a programmer gets more complicated when building web applications, web services, and apps with GUIs such as Windows desktop and mobile apps.

One reason for this is that for a GUI app, there is a special thread: the user interface (UI) thread.

There are two rules for working in GUIs:

+   - Do not perform long-running tasks on the UI thread.
+   - Do not access UI elements on any thread except the UI thread.

To handle these rules, programmers used to have to write complex code to ensure that long-running tasks were executed by a non-UI thread, but once complete, the results of the task were safely passed to the UI thread to present to the user. It could quickly get messy!

Luckily, with C# 5 and later, you have the use of async and await. They allow you to continue to write your code as if it is synchronous, which keeps your code clean and easy to understand, but underneath, the C# compiler creates a complex state machine and keeps track of running threads. It's kind of magical!

Let's see an example. We will build a Windows desktop app using WPF that gets employees from the Northwind database in an SQL Server database using low-level types like SqlConnection, SqlCommand, and SqlDataReader. You will only be able to complete this task if you have Windows and the Northwind database stored in SQL Server. This is the only section in this book that is not cross-platform and modern (WPF is 16 years old!).

At this point, we are focusing on making a GUI app responsive. You will learn about XAML and building cross-platform GUI apps in Chapter 19, Building Mobile and Desktop Apps Using .NET MAUI. Since this book does not cover WPF elsewhere, I thought this task would be a good opportunity to at least see an example app built using WPF even if we do not look at it in detail.

Let's go!

+   1. If you are using Visual Studio 2022 for Windows, add a new WPF Application [C#] project named WpfResponsive to the Chapter12 solution. If you are using Visual Studio Code, use the following command: dotnet new wpf.