The method stream is available on all collections in the JDK and it wraps the collection into an instance of Stream. The map method applies the given lambda expression or block of code, within the parenthesis, on each element in the Stream. The map method is quite unlike the forEach method, which simply runs the block in the context of each element in the collection. In addition, the map method collects the result of running the lambda expression and returns the result collection. Finally, we print the elements in this result using the forEach method. The names in the new collection are in all caps:

> BRIAN NATE NEAL RAJU SARA SCOTT

The map method is useful to map or transform an input collection into a new output collection. This method will ensure that the same number of elements exists in the input and the output sequence. But the element types in the input don't have to match the element types in the output collection. In this example, both the input and the output are a collection of strings. We could've passed to the map method a block of code that returned, for example, the number of characters in a given name. In this case, the input would still be a sequence of strings, but the output would be a sequence of numbers, as in the next example.

![](bbox=[0.143, 0.692, 0.883, 0.72])

```
friends.stream()
.map(name -> name.length())
.forEach(count -> System.out.print(count + " "));
```

The result is a count of the number of letters in each name:

> 5 4 4 4 5