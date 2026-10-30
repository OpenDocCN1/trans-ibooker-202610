Be aware that this is a courtesy provided by C#. Not all .NET Core languages support this shorthand attribute syntax.

# Specifying Constructor Parameters for Attributes

Notice that the [Obsolete] attribute can accept what appears to be a constructor parameter. If you view the formal definition of the [Obsolete] attribute by right-clicking the item in the code editor and selecting the Go To Definition menu option, you will find that this class indeed provides a constructor receiving a System.String.

```
public sealed class ObsoleteAttribute : Attribute
{
public ObsoleteAttribute(string message, bool error);
public ObsoleteAttribute(string message);
public ObsoleteAttribute();
public bool IsError { get; }
public string? Message { get; }
}
```

Understand that when you supply constructor parameters to an attribute, the attribute is not allocated into memory until the parameters are reflected upon by another type or an external tool. The string data defined at the attribute level is simply stored within the assembly as a blurb of metadata.

# The Obsolete Attribute in Action

Now that HorseAndBuggy has been marked as obsolete, if you were to allocate an instance of this type:

```
using System;
using ApplyingAttributes;

Console.WriteLine("Hello World!");
HorseAndBuggy mule = new HorseAndBuggy();
```

you would find that a compiler warning is issued. The warning is specifically CS0618, and the message includes the information passed into the attribute.

'HorseAndBuggy' is obsolete: 'Use another vehicle!'

Visual Studio and Visual Studio Code also help with IntelliSense, which gets their information through reflection. Figure 17-1 shows the results of the Obsolete attribute in Visual Studio, and Figure 17-2 is using Visual Studio Code. Note that both use the term deprecated instead of obsolete.