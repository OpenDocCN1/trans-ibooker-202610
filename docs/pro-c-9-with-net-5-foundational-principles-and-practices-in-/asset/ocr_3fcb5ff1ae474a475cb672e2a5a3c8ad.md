# CHAPTER 3

# Core C# Programming Constructs, Part 1

This chapter begins your formal investigation of the C# programming language by presenting a number of bite-sized, stand-alone topics you must be comfortable with as you explore the .NET Core Framework. The first order of business is to understand how to build your program’s application object and to examine the composition of an executable program’s entry point: the Main() method as well as a new C# 9.0 feature, top-level statements. Next, you will investigate the fundamental C# data types (and their equivalent types in the System namespace) including an examination of the System.String and System.Text.StringBuilder classes.

After you know the details of the fundamental .NET Core data types, you will then examine a number of data type conversion techniques, including narrowing operations, widening operations, and the use of the checked and unchecked keywords.

This chapter will also examine the role of the C# var keyword, which allows you to implicitly define a local variable. As you will see later in this book, implicit typing is extremely helpful, if not occasionally mandatory, when working with the LINQ technology set. You will wrap up this chapter by quickly examining the C# keywords and operators that allow you to control the flow of an application using various looping and decision constructs.

# Breaking Down a Simple C# Program

C# demands that all program logic be contained within a type definition (recall from Chapter 1 that type is a general term referring to a member of the set {class, interface, structure, enumeration, delegate}). Unlike many other languages, in C# it is not possible to create global functions or global points of data. Rather, all data members and all methods must be contained within a type definition. To get the ball rolling, create a new empty solution named Chapter3_AllProject.sln that contains a C# console application named SimpleCSharpApp.

From Visual Studio, select the Blank Solution template on the “Create a new project” screen. When the solution opens, right-click the solution in Solution Explorer and select Add ► New Project. Select “C# console app” from the templates, name it SimpleCSharpApp, and click Create. Remember to update the Target Framework to net5.0.

From the command line, execute the following:

```
dotnet new sln -n Chapter3_AllProjects

dotnet new console -lang c# -n SimpleCSharpApp -o .\SimpleCSharpApp -f net5.0
dotnet sln .\Chapter3_AllProjects.sln add .\SimpleCSharpApp
```