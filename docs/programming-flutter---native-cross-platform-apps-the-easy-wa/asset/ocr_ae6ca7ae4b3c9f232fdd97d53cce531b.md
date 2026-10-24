# THROW THE EXCEPTION

That exception can be thrown by using:

```
throw DivideByZeroException();
```

In this specific case, inside the definition of Calculation.getResult in calculator.dart, at the point where we handle divisions (where you find else if(a[i] == "÷") ), we’ll add:

layout/calculator_error_if_divideby0/lib/calculator.dart

```
if(double.parse(a[i+1]) == 0)
  throw DivideByZeroException();
```

so the part of getResult that handles divisions and multiplications becomes:

layout/calculator_error_if_divideby0/lib/calculator.dart

```
for(int i = 0; i < a.length; i++) {
  if(a[i] == "x") {
    a[i-1] = "${double.parse(a[i-1]) * double.parse(a[i+1])}";
    a.removeAt(i);
    a.removeAt(i);
    i--;
  } else if(a[i] == "÷") {
    if(double.parse(a[i+1]) == 0)
      throw DivideByZeroException();
    a[i-1] = "${double.parse(a[i-1]) / double.parse(a[i+1])}";
    a.removeAt(i);
    a.removeAt(i);
    i--;
  }
}
```

# CATCH THE EXCEPTION

Exceptions in Dart are caught using:

```
try {
  code_that_might_generate_exceptions();
} catch(exception) {
  print("$exception");
}
```

This code catches any kind of exception; if you wanted to catch only certain types of exceptions you would use:

```
try {
  code_that_might_generate_exceptions();
```