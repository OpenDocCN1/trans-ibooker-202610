# Understanding native-sized integers

C# 9 introduced nint and nuint keyword alias for native-sized integers, meaning that the storage size for the integer value is platform specific. They store a 32-bit integer in a 32-bit process and sizeof() returns 4 bytes; they store a 64-bit integer in a 64-bit process and sizeof() returns 8 bytes. The aliases represent pointers to the integer value in memory, which is why their .NET names are IntPtr and UIntPtr. The actual storage type will be either System.Int32 or System.Int64 depending on the process.

In a 64-bit process, the following code:

```
```csharp
WriteLine($"int.MaxValue = {int.MaxValue:N0}");
WriteLine($"nint.MaxValue = {nint.MaxValue:N0}");
```
```

produces this output:

```
```
int.MaxValue = 2,147,483,647
nint.MaxValue = 9,223,372,036,854,775,807
```
```

# Revealing the location of a type

Code editors provide built-in documentation for .NET types. Let's explore:

+   1. Right-click inside XDocument and choose Go to Definition.
+   2. Navigate to the top of the code file and note the assembly filename is System.Xml.XDocument.dll, but the class is in the System.Xml.Linq namespace, as shown in Figure 7.1:
+   3. Close the XDocument [from metadata] tab.
+   4. Right-click inside string or String and choose Go to Definition.
+   5. Navigate to the top of the code file and note the assembly filename is System.Runtime.dll but the class is in the System namespace.

![](bbox=[0.18, 0.55, 0.93, 0.74])

Figure 7.1: Assembly and namespace that contains the XDocument type

Actually, your code editor is technically lying to you. If you remember when we wrote code in Chapter 2, Speaking C#, when we revealed the extent of the C# vocabulary, we discovered that the System.Runtime.dll assembly contains zero types.