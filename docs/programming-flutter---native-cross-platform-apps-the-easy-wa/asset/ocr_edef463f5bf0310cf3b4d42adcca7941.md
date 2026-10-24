```
} on ExceptionClass catch(exception) {
  print("$exception");
}
```

And if you don’t need to use the value of the exception you would leave out the catch(exception) part:

```
try {
  code_that_might_generate_exceptions();
} on ExceptionClass {
  print("you did something bad");
}
```

In the specific case, the code that might generate exceptions is the _str = _calculation.getResult().toString() function call and assignment and we want to set _str to something like You mustn’t divide by 0.

The getResult method will therefore become:

layout/calculator_error_if_divideby0/lib/main.dart

```
void getResult() {
  setState(() {
    try {
      _str = _calculation.getResult().toString();
    }
    on DivideByZeroException {
      _str = "You mustn't divide by 0";
    }
    finally {
      _calculation = new Calculation();
    }
  });
}
```

Adding a finally clause to reset the calculation regardless of whether or not the exception was thrown.