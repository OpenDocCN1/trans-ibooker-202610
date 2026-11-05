# Importing a namespace to use a type

Let's explore how namespaces are related to assemblies and types:

1. In the AssembliesAndNamespaces project, in Program.cs, enter the following code:

```
XDocument doc = new();
```

2. Build the project and note the compiler error message, as shown in the following output:

```
The type or namespace name 'XDocument' could not be found (are you missing a using directive or an assembly reference?)
```

The XDocument type is not recognized because we have not told the compiler what the namespace of the type is. Although this project already has a reference to the assembly that contains the type, we also need to either prefix the type name with its namespace or import the namespace.

3. Click inside the XDocument class name. Your code editor displays a light bulb, showing that it recognizes the type and can automatically fix the problem for you.

4. Click the light bulb, and select using System.Xml.Linq; from the menu.

This will import the namespace by adding a using statement to the top of the file. Once a namespace is imported at the top of a code file, then all the types within the namespace are available for use in that code file by just typing their name without the type name needing to be fully qualified by prefixing it with its namespace.

Sometimes I like to add a comment with a type name after importing a namespace to remind me why I need to import that namespace, as shown in the following code:

```
using System.Xml.Linq; // XDocument
```

# Relating C# keywords to .NET types

One of the common questions I get from new C# programmers is, "What is the difference between string with a lowercase s and String with an uppercase S?"

The short answer is easy: none. The long answer is that all C# type keywords like string or int are aliases for a .NET type in a class library assembly.

When you use the string keyword, the compiler recognizes it as a System.String type. When you use the int type, the compiler recognizes it as a System.Int32 type.

Let's see this in action with some code:

1. In Program.cs, declare two variables to hold string values, one using lowercase string and one using uppercase String, as shown in the following code:

```
string s1 = "Hello";
String s2 = "World";
```