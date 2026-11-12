```
final Optional<String> aLongName =
friends.stream()
.reduce((name1, name2) ->
name1.length() >= name2.length() ? name1 : name2);
aLongName.ifPresent(name ->
System.out.println(String.format("A longest name: %s", name)));
```

The lambda expression we are passing to the reduce method takes two parameters, name1 and name2, and returns one of them based on the length. The reduce method has no clue about our specific intent. That concern is separated from this method into the lambda expression that we pass to it—this is a lightweight application of the strategy pattern—see Design Patterns: Elements of Reusable Object-Oriented Software [GHJV95], by Gamma et al.

This lambda expression conforms to the interface of an apply method of a JDK functional interface named BinaryOperator. This is the type of the parameter the reduce method receives. Let’s run the reduce method and see if it picks the first of the two longest names from our friends list.

A longest name: Brian

As the reduce method iterated through the collection, it called the lambda expression first, with the first two elements in the list. The result from the lambda expression is used for the subsequent call. In the second call, name1 is bound to the result from the previous call to the lambda expression, and name2 is bound to the third element in the collection. The calls to the lambda expression continue for the rest of the elements in the collection. The result from the final call is returned as the result of the reduce method call.

The result of the reduce method is an Optional because the list on which reduce is called may be empty. In that case, there would