+   - Instantiating a class
+   - Methods
+   - One-to-one has-a relationships

# Classes and fields

In VB, classes are used to define objects with specific properties (fields) and behaviors (methods). Here’s an overview of how classes and fields work in VB.

# Class declaration

To define a class in VB, use the **Class** keyword followed by the class’s name. Here’s an example:

```
Class MyClass
    ' Class members will be defined here
End Class
```

# Fields

Fields, also known as attributes or variables, are the data members of a class. They represent the properties or characteristics of an object. Fields hold data values that are associated with objects created from the class. Fields are declared within the class, typically at the beginning, and can have different access modifiers (**Public, Private, Protected**, and so on) to control their visibility and accessibility:

```
Class MyClass
    Private myField As Integer
    Public anotherField As String
End Class
```

In the preceding example, **myField** is a private field and **anotherField** is a public field.

Next, let’s look at access modifiers, which we can use to control access to the fields in the class.

**Access modifiers** determine the accessibility of the fields within and outside the class. VB provides several access modifiers, including the following:

+   - **Private:** The field is only accessible within the class
+   - **Public:** The field is accessible from any code that can access the object
+   - **Protected:** The field is accessible within the class and its derived classes
+   - **Friend:** The field is accessible within the same assembly or project