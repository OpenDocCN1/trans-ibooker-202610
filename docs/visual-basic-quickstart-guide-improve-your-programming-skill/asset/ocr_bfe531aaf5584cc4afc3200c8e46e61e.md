# Formatting and Modifying Data

Formatting data is a common task in programming languages to present data in a desired format. A programmer will also have to convert data in the same format or to a different format to allow it to be used in an algorithm.

In this chapter, we’re going to cover the following main topics:

+   - Modifying string data
+   - Modifying numerical data
+   - Modifying date and time data
+   - Formatting numeric values
+   - Formatting date and time values

# Modifying string data

In VB, you can modify string data using various built-in functions and operators. Here are some standard operations you can perform on string data:

# Concatenation

There are a few ways to combine strings together in VB:

+   - **The & operator:** You can concatenate strings using the **&** operator. Here is an example:

```
Dim firstName As String = "Sally"
Dim lastName As String = "Stone"
Dim fullName As String = firstName & " " & lastName
```

The preceding code will make one string from the three separate strings **Sally**, a space, and the last name, **Stone**. The concatenation operator is available for all VB family members.

+   - **The String.Concat method:** Another way to concatenate strings is using the **String.Concat** method. Here is the same example utilizing the **String.Concat** method:

```
Dim firstName As String = "Sally"
Dim lastName As String = "Stone"
Dim fullName As String = String.Concat(firstName, " ", lastName)
```

The preceding code has the same effect as the concatenation operator example. It will make one string from the three separate strings **Sally**, a space, and the last name **Stone**. The **String.Concat** method is only available in the VB.NET family member.