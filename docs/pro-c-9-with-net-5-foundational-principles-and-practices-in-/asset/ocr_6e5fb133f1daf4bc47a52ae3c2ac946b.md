As you can see, this is just a normal class. The nullability comes in when you use this class in your code. Take the following declarations:

```
string? nullableString = null;
TestClass? myNullableClass = null;
```

The project file setting makes the entire project a nullable context. The nullable context allows the declarations of the string and TestClass types to use the nullable annotation (?). The following line of code generates a warning (CS8600) due to the assignment of a null to a non-nullable type in a nullable context:

```
//Warning CS8600 Converting null literal or possible null value to non-nullable type
TestClass myNonNullableClass = myNullableClass;
```

For finer control of where the nullable contexts are in your project, you can use compiler directives (as discussed earlier) to enable or disable the context. The following code turns off the nullable context (set at the project level) and then reenables it by restoring the project settings:

```
#nullable disable
TestClass anotherNullableClass = null;
//Warning CS8632 The annotation for nullable reference types
//should only be used in code within a '#nullable' annotations
TestClass? badDefinition = null;
//Warning CS8632 The annotation for nullable reference types
//should only be used in code within a '#nullable' annotations
string? anotherNullableString = null;
#nullable restore
```

As a final note, the nullable reference types do not have the HasValue and Value properties, as those are supplied by System.Nullable<T>.

# Migration Considerations

When migrating your code from C# 7 to C# 8 or C# 9 and you want to make use of nullable reference types, you can use a combination of the project setting and compiler directives to work through your code. A common practice is to start by enabling warnings and disabling nullable annotations for the entire project. Then, as you clean up areas of code, use the compiler directives to gradually enable the annotations.

# Operating on Nullable Types

C# provides several operators for working with nullable types. The next sessions code the null-coalescing operator, the null-coalescing assignment operator, and the null conditional operator. For these examples, go back to the FunWithNullableValueTypes project.

# The Null-Coalescing Operator

The next aspect to be aware of is any variable that might have a null value can make use of the C# ?? operator, which is formally termed the null-coalescing operator. This operator allows you to assign a value to a nullable type if the retrieved value is in fact null. For this example, assume you want to assign a local nullable integer to 100 if the value returned from GetIntFromDatabase() is null (of course, this