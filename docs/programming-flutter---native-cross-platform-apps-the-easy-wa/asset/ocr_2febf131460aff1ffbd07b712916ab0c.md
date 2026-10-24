Here’s an example of a class that has a constructor that takes an integer value and assigns its square to the var1 while assigning var2 and var3 directly:

```
class ClassExample {
  ClassExample(this.var1, this.var2, this.var3);
  ClassExample.withSqrtOfVar1(int var1sqrt, this.var2, this.var3) {
    var1 = var1sqrt*var1sqrt;
  }

  int var1;
  String var2;
  Function var3;
}
```

# STATIC MEMBERS

Static class members can be accessed without requiring instantiation:

```
class ClassExample {
  // (...)

  static num square(num val) =>
    val*val;

  // (...)
}

// Unlike in C++, we can use regular dot notation
var n = ClassExample.square(0.1);

/*
 * Comparing floating-point numbers
 * using == is unreliable in
 * most programming languages,
 * including Dart, so use
 * syntax like this instead
 */
assert(n < 0.011 && n > 0.009);
```

# ABSTRACT CLASSES

An abstract class is a class that you can’t instantiate: you can’t create objects of that class. They are declared by substituting the class keyword with abstract class. Abstract classes are usually used as mixins (covered in Writing a Class with a Mixin) or interfaces (covered in Writing a Class That implements an Interface).

An abstract class can contain abstract methods, which are method that don’t have an implementation, but it can also contain regular methods: