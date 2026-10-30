# The Scope of the dynamic Keyword

Recall that implicitly typed data (declared with the var keyword) is possible only for local variables in a member scope. The var keyword can never be used as a return value, a parameter, or a member of a class/structure. This is not the case with the dynamic keyword, however. Consider the following class definition:

```
namespace DynamicKeyword
{
    class VeryDynamicClass
    {
        // A dynamic field.
        private static dynamic _myDynamicField;

        // A dynamic property.
        public dynamic DynamicProperty { get; set; }

        // A dynamic return type and a dynamic parameter type.
        public dynamic DynamicMethod(dynamic dynamicParam)
        {
            // A dynamic local variable.
            dynamic dynamicLocalVar = "Local variable";

            int myInt = 10;

            if (dynamicParam is int)
            {
                return dynamicLocalVar;
            }
            else
            {
                return myInt;
            }
        }
    }
}
```

You could now invoke the public members as expected; however, as you are operating on dynamic methods and properties, you cannot be completely sure what the data type will be! To be sure, the VeryDynamicClass definition might not be useful in a real-world application, but it does illustrate the scope of where you can apply this C# keyword.

# Limitations of the dynamic Keyword

While a great many things can be defined using the dynamic keyword, there are some limitations regarding its usage. While they are not showstoppers, do know that a dynamic data item cannot make use of lambda expressions or C# anonymous methods when calling a method. For example, the following code will always result in errors, even if the target method does indeed take a delegate parameter that takes a string value and returns void:

```
dynamic a = GetDynamicObject();

// Error! Methods on dynamic data can't use lambdas!
a.Method(arg => Console.WriteLine(arg));
```