# Outer.java

```
private static String level = "outer";
```

# Inner.java

```
public static String getOuterField() {
    return Outer.level;
}
```

Figure 1-5. Accessing a parent field from a nested class

As you might know, the Java compiler creates a new class file for inner classes, in this case Outer$Inner.class. Under the hood, the compilation process also creates so-called accessibility-broadening bridge methods between the classes.

Java 11 pays down the technical debt of having to generate those bridge methods with the help of so-called nestmates. Inner classes can now access fields and methods from outer classes without additional work from the compiler.

You might ask yourself, “Compiler optimizations are great, but how does it affect me as a developer”? Well, prior to Java 11, it was not possible to access a field from the outer class via reflection without setting the accessibility control to true. With the change of nest-based access control, this control is no longer necessary. The following class hierarchy demonstrates the changed behavior:

```
import java.lang.reflect.Field;

public class Outer {
    private static String level = "outer";

    public static class Inner {
        public static String getOuterViaRegularFieldAccess() {
            return Outer.level;
        }
    }
```