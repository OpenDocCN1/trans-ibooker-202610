The versions using the lambda expressions have no explicit mutation; they're concise. These versions also didn't need any initial empty collection or garbage variable; that variable quietly receded into the shadows of the underlying implementation.

# # Using Method References

We can nudge the code to be just a bit more concise by using a feature called *method reference*. The Java compiler will take either a lambda expression or a reference to a method where an implementation of a functional interface is expected. With this feature, a short `String::toUpperCase` can replace `name -> name.toUpperCase()`, like so:

collections/fpij/Transform.java

```
```java
friends.stream()
        .map(String::toUpperCase)
        .forEach(name -> System.out.println(name));
```
```

Java knows to invoke the String class's given method toUpperCase on the parameter passed into the synthesized method—the implementation of the functional interface's abstract method. That parameter reference is implicit here. In simple situations like the previous example, we can substitute method references for lambda expressions; see When Should We Use Method References?.

In the preceding example, the method reference was for an instance method. Method references can also refer to static methods and methods that take parameters. We'll see examples of these later.

> > Joe asks:
> > When Should We Use Method References?