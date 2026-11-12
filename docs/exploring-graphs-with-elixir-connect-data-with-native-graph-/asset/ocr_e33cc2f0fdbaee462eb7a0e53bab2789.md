```
for(int i = 0; i < friends.size() - 1; i++) {
System.out.print(friends.get(i) + ", ");
}
```

```
if(friends.size() > 0)
System.out.println(friends.get(friends.size() - 1));
```

Let’s see if the output of this version was decent.

Brian, Nate, Neal, Raju, Sara, Scott

The result looks good, but the code to produce the output doesn’t. Beam us up, modern Java.

We don’t have to endure that pain. A `StringJoiner` class cleans up all that mess and the `String` class has an added convenience method `join` to turn that smelly code into a simple one-liner.

collections/fpij/PrintList.java

```
System.out.println(String.join(", ", friends));
```

Let’s quickly verify the output is as charming as the code that produced it.

Brian, Nate, Neal, Raju, Sara, Scott

Under the hood, the `String`’s `join` method calls upon the `StringJoiner` class to concatenate the values in the second argument, a `varargs`, into a larger string separated by the first argument. We’re not limited to concatenating only with a comma using this feature. We could, for example, take a bunch of `paths` and concatenate them to form a `classpath` easily thanks to the new methods and classes.

We saw how to join a list of elements; we can also transform the elements before joining them. We already know how to transform elements using the `map` method. We can also be