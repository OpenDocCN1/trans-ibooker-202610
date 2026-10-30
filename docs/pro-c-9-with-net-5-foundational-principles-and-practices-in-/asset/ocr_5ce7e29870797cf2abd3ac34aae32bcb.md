> Note  An attribute applies to the “very next” item.

At this point, do not concern yourself with the actual process of object serialization (Chapter 20 examines the details). Just notice that when you want to apply an attribute, the name of the attribute is sandwiched between square brackets.

As you might guess, a single item can be attributed with multiple attributes. Assume you have a legacy C# class type (HorseAndBuggy) that was attributed to have a custom XML namespace. The code base has changed over time, and the class is now considered obsolete for current development. Rather than deleting the class definition from your code base (and risk breaking existing software), you can mark the class with the [Obsolete] attribute. To apply multiple attributes to a single item, simply use a comma-delimited list, like so:

```
using System;
using System.Xml.Serialization;

namespace ApplyingAttributes
{
    [XmlRoot(Namespace = "http://www.MyCompany.com"), Obsolete("Use another vehicle!")]
    public class HorseAndBuggy
    {
        // ...
    }
}
```

As an alternative, you can also apply multiple attributes on a single item by stacking each attribute as follows:

```
[XmlRoot(Namespace = "http://www.MyCompany.com")]
[Obsolete("Use another vehicle!")]
public class HorseAndBuggy
{
    // ...
}
```

# C# Attribute Shorthand Notation

If you were consulting the .NET Core documentation, you might have noticed that the actual class name of the [Obsolete] attribute is ObsoleteAttribute, not Obsolete. As a naming convention, all .NET Core attributes (including custom attributes you may create yourself) are suffixed with the Attribute token. However, to simplify the process of applying attributes, the C# language does not require you to type in the Attribute suffix. Given this, the following iteration of the HorseAndBuggy type is identical to the previous (it just involves a few more keystrokes):

```
[SerializableAttribute]
[ObsoleteAttribute("Use another vehicle!")]
public class HorseAndBuggy
{
    // ...
}
```