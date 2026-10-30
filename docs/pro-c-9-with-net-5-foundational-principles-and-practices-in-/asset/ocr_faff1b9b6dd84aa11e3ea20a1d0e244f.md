Like C#, CIL allows you to define a nested namespace as follows:

```
// Defining a nested namespace.
.namespace MyCompany.MyNamespace {}
```

# Defining Class Types in CIL

Empty namespaces are not remarkably interesting, so let’s now check out the process of defining a class type using CIL. Not surprisingly, the .class directive is used to define a new class. However, this simple directive can be adorned with numerous additional attributes, to further qualify the nature of the type. To illustrate, add a public class to your namespace named MyBaseClass. As in C#, if you do not specify an explicit base class, your type will automatically be derived from System.Object.

```
.namespace MyNamespace
{
    // System.Object base class assumed.
    .class public MyBaseClass {}
}
```

When you are building a class type that derives from any class other than System.Object, you use the extends attribute. Whenever you need to reference a type defined within the same assembly, CIL demands that you also use the fully qualified name (however, if the base type is within the same assembly, you can omit the assembly’s friendly name prefix). Therefore, the following attempt to extend MyBaseClass results in a compiler error:

```
// This will not compile!
.namespace MyNamespace
{
    .class public MyBaseClass {}

    .class public MyDerivedClass
        extends MyBaseClass {}
}
```

To correctly define the parent class of MyDerivedClass, you must specify the full name of MyBaseClass as follows:

```
// Better!
.namespace MyNamespace
{
    .class public MyBaseClass {}

    .class public MyDerivedClass
        extends MyNamespace.MyBaseClass {}
}
```

In addition to the public and extends attributes, a CIL class definition may take numerous additional qualifiers that control the type’s visibility, field layout, and so on. Table 19-2 illustrates some (but not all) of the attributes that may be used in conjunction with the .class directive.