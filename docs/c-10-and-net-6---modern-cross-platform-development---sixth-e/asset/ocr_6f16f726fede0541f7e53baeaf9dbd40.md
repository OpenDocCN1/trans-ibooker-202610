**Good Practice:** In most cases today, UTF-8 is a good default, which is why it is literally the default encoding, that is, Encoding.Default.

# Encoding strings as byte arrays

Let's explore text encodings:

+   1. Use your preferred code editor to add a new console app named `WorkingWithEncodings` to the `Chapter09` solution/workspace.
+   2. In Visual Studio Code, select `WorkingWithEncodings` as the active OmniSharp project.
+   3. In `Program.cs`, import the `System.Text` namespace and statically import the `Console` class.
+   4. Add statements to encode a string using an encoding chosen by the user, loop through each byte, and then decode it back into a `string` and output it, as shown in the following code:

```
WriteLine("Encodings");
WriteLine("[1] ASCII");
WriteLine("[2] UTF-7");
WriteLine("[3] UTF-8");
WriteLine("[4] UTF-16 (Unicode)");
WriteLine("[5] UTF-32");
WriteLine("[any other key] Default");

// choose an encoding
Write("Press a number to choose an encoding: ");
ConsoleKey number = ReadKey(intercept: false).Key;
WriteLine();
WriteLine();

Encoding encoder = number switch
{
    ConsoleKey.D1 => Encoding.ASCII,
    ConsoleKey.D2 => Encoding.UTF7,
    ConsoleKey.D3 => Encoding.UTF8,
    ConsoleKey.D4 => Encoding.Unicode,
    ConsoleKey.D5 => Encoding.UTF32,
    _ => Encoding.Default
};

// define a string to encode
string message = "Café cost: £4.39";
```