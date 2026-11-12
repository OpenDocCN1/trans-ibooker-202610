break out of the loop if we found an element—here are other sources of rancid smells: primitive obsession, imperative style, and mutability. Once out of the loop, we had to check the response and print the appropriate result. That's quite a bit of code for a simple task.

Let's rethink the problem. We simply want to pick the first matching element and safely deal with the absence of such an element. Let's rewrite the pickName method, this time using lambda expressions.

```
collections/fpij/PickAnElementElegant.java

public static void pickName(
final List<String> names, final String startingLetter) {
final Optional<String> foundName =
names.stream()
.filter(name ->name.startsWith(startingLetter))
.findFirst();

System.out.println(String.format("A name starting with %s: %s",
startingLetter, foundName.orElse("No name found")));
}
```

Some powerful features in the JDK library came together to help achieve this conciseness. First, we used the filter method to grab all the elements matching the desired pattern. Then the findFirst method of the Stream class helped pick the first value from that collection. This method returns a special Optional object, which is the state-appointed null deodorizer in Java.

The Optional class is useful whenever the result may be absent. It protects us from getting a NullPointerException by accident and makes it quite explicit to the reader that “no result found” is a possible outcome. We can inquire if an object is present by using the isPresent method, and we can obtain the current value