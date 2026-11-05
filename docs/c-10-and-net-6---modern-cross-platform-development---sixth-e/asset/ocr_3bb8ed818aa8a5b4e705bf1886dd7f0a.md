# Working with numbers

One of the most common types of data is numbers. The most common types in .NET for working with numbers are shown in the following table:

| Namespace | Example type(s) | Description |
|---|---|---|
| System | SByte, Int16, Int32, Int64 | Integers; that is, zero and positive and negative whole numbers |
| System | Byte, UInt16, UInt32, UInt64 | Cardinals; that is, zero and positive whole numbers |
| System | Half, Single, Double | Reals; that is, floating-point numbers |
| System | Decimal | Accurate reals; that is, for use in science, engineering, or financial scenarios |
| System.Numerics | BigInteger, Complex, Quaternion | Arbitrarily large integers, complex numbers, and quaternion numbers |

.NET has had the 32-bit float and 64-bit double types since .NET Framework 1.0. The IEEE 754 specification also defines a 16-bit floating point standard. Machine learning and other algorithms would benefit from this smaller, lower-precision number type so Microsoft introduced the System.Half type with .NET 5 and later.

Currently, the C# language does not define a half alias so you must use the .NET type System.Half. This might change in the future.

# Working with big integers

The largest whole number that can be stored in .NET types that have a C# alias is about eighteen and a half quintillion, stored in an unsigned long integer. But what if you need to store numbers larger than that?

Let's explore numerics:

+   1. Use your preferred code editor to create a new solution/workspace named `Chapter08`.
+   2. Add a console app project, as defined in the following list:
+      1. Project template: **Console Application** / `console`
+      2. Workspace/solution file and folder: `Chapter08`
+      3. Project file and folder: `WorkingWithNumbers`
+   3. In `Program.cs`, delete the existing statements and add a statement to import `System.Numerics`, as shown in the following code:
+   4. Add statements to output the maximum value of the `ulong` type, and a number with 30 digits using `BigInteger`, as shown in the following code:

```
using System.Numerics;
```