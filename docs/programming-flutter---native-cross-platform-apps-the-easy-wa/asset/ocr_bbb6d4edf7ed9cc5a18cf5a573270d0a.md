```
catch(exception) {
  // Deal with the exception
}
// ...
}
```

You can substitute exception in catch’s parentheses with whatever you want (it is usually just e). It contains the name of the exception that was thrown. You can print it to the debug console by using print(exception). Additionally, you can pass a stack trace of the source of the error, which you can also print.

For example, the following:

```
try {
  int.parse("Flutter");
} catch(e, stacktrace) {
  print(e);
  print(stacktrace);
}
```

will trigger a FormatException and print the following to the debug console:

```
FormatException: Invalid radix-10 number (at character 1)
Flutter
^
#0 int._throwFormatException
(dart:core/runtime/lib/integers_patch.dart:131:5)
#1 int._parseRadix (dart:core/runtime/lib/integers_patch.dart:142:16)
#2 int._parse (dart:core/runtime/lib/integers_patch.dart:100:12)
#3 int.parse (dart:core/runtime/lib/integers_patch.dart:63:12)
#4 main (file:///home/carmine/flutter/example_app/lib/main.dart:21:9)
```

on SomeException catch()

What if that called function had thrown another kind of exception, which didn’t have myOwnPersonalMember or maybe you needed to deal with different exceptions in different ways? That’s what on is for.

For example, let’s say we have a function that takes two numbers in a string format, parses them, and performs integer division on them. This means two different kinds exceptions can be thrown during its execution: a FormatException if it can’t parse the numbers, or a IntegerDivisionByZeroException exception if we try to divide by zero.