the first one, but performs a different operation or transformation. Finally, the third parameter, a BiFunction, is used to combine the results of the two operations to produce a combined cumulative result. Let’s use teeing to find the min and max as desired:

```
record MinMax(String least, String most) {}

var leastAndMostEmailAddressPerson =
SAMPLE_DATA.stream()
.collect(
teeing(
minBy(comparing(person -> person.emailAddresses().size())),
maxBy(comparing(person -> person.emailAddresses().size())),
(min, max) ->
new MinMax(min.map(Person::fullName).orElse(""),
max.map(Person::fullName).orElse(""))));

System.out.println(leastAndMostEmailAddressPerson);
```

We created a local record MinMax within the method where it’s needed. The record makes the intent clear and the code expressive as well. Records are a great way to create tuples in modern Java and are useful for conveniently handling a small arbitrary collection of data. The first argument passed to the teeing function picks a person with the minimum number of email addresses, using the minBy function of Collectors. The second argument picks the person with the maximum number of email addresses, using the maxBy method. If the data that flows is empty, then there may not be a min or max. Thus the results of minBy and maxBy are Optional<T>. In the function to combine the two values into a cumulative result, we create an instance of the local record MinMax with the two components or properties. The first, least, will hold the full name of the person picked by the minBy of