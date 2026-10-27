# Substring extraction

There are a few ways to pull part of a string out of another string in VB:

+   The Mid function: Using the Mid function, you can extract a substring from a string, for example:

```
Dim myStr As String = "Hello World!"
Dim substr As String = Mid(myStr, 6, 5)
```

This example extracts the substring starting at index 6 with a length of 5 to pull out the word World and store it in another variable. The last parameter can be left off.

+   The Substring method: Using the Substring method was added to VB.NET so you can extract a substring from a string, for example:

```
Dim myStr As String = "Hello World!"
Dim substr As String = myStr.Substring(6, 5)
```

The preceding example extracts the substring starting at index 6 with a length of 5 to pull out the word World and store it in another variable. With VB.NET, you can still utilize the Mid function to do the same.

# String replacement

Using the Replace method, you can replace specific occurrences of a substring within a string. Here is an example in VB.NET:

```
Dim myStr As String = "Hello World!"
Dim repStr As String = myStr.Replace("World", "Sally")
```

The preceding code replaces the word "World" with the word "Sally". The repStr variable will now hold the string "Hello Sally!"

In earlier family members of VB, the Replace function takes the string to operate as the first parameter. Here is an example in VBA:

```
Dim myStr As String = "Hello World!"
Dim repStr As String = Replace(myStr,"World", "Sally")
```

This code works just like the VB.NET version and replaces the word "World" with the word "Sally". The repStr variable will now hold the "Hello Sally!" string.

# Case conversion

Using the ToUpper method, you can convert a string to uppercase. For example, in VB.NET, you can do the following:

```
Dim myStr As String = "hello world!"
Dim uppStr As String = MyStr.ToUpper()
```