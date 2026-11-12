
same age, our lambda expression will return a 0 to indicate they're equal. Otherwise, it will indicate the first person is younger by returning a negative number or older by returning a positive number for the age difference.

The sorted method will iterate over each element in the target collection (people in this example) and apply the given Comparator (a lambda expression in this case) to decide the logical ordering of the elements. The execution mechanism of sorted is much like the reduce method we saw earlier. The reduce method reduces the list down to one value. The sorted method, on the other hand, uses the result of the comparison to perform the ordering.

Once we sort the instances, we want to print the values, so we invoke a convenience method printPeople; let's write that method next.

```
compare/fpij/Compare.java
public static void printPeople(
final String message, final List<Person> people) {
System.out.println(message);
people.forEach(System.out::println);
}
```

In this method we print a message and iterate over the given collection, printing each of the instances.

Let's call the sorted method, and the people in the list will be printed in ascending order by age.

Sorted in ascending order by age:
John - 20
Sara - 21
Jane - 21
Greg - 35

Let's revisit the call to the sorted method and make one more improvement to it.

```
.sorted((person1, person2) -> person1.ageDifference(person2))
```

In the lambda expression we're passing to the sorted method, we're simply routing the two parameters—the first parameter as the target to the ageDifference method and the second as its argument. Rather than writing this code, we can use the office-space pattern (that is, ask the Java compiler to do the routing again, using a method reference).

The parameter routing we want here is a bit different from the ones we saw earlier. So far, we've seen a parameter being used as a target in one case and as an argument in another case. In the current situation, however, we have two parameters that we want to be split: the first to be used as a target to the method and the second as an argument. No worries. The Java compiler gives us a friendly nod: “I can take care of that for you.”

Let's replace the lambda expression in the previous call to the sorted method with a short and sweet reference to the ageDifference method.

```
people.stream()

.sorted(Person::ageDifference)
```

The code is fantastically concise thanks to the method-reference convenience the Java compiler offers. The compiler took the parameters, the two-person instances being compared, and made the first the ageDifference method's target and the second the parameter. Rather than explicitly connecting these, we let the compiler work a little extra for us. When using this conciseness, we must be careful to ensure that the first parameter is the intended target of the method referenced and the remaining parameters are its arguments.

## Reusing a Comparator

We easily sorted the people in ascending order by age, and we can as easily sort them in descending order. Let’s give that a shot.

```
compare/fpij/Compare.java
printPeople("Sorted in descending order by age: ",
people.stream()
.sorted((person1, person2) -> person2.ageDifference(person1))
.collect(toList()));
```

We called the sorted method and passed a lambda expression that conforms to the Comparator interface, much like the previous time. The only difference is the implementation of the lambda expression—we switched the people in the age comparison. The result should be a sort by descending order of their ages. Let’s look at the output.

```
Sorted in descending order by age:
Greg - 35
Sara - 21
Jane - 21
John - 20
```

Changing the logic for our comparison was effortless. But we can’t refactor this version to use the method reference because the parameter order here doesn’t follow the parameter-routing conventions for method reference; the first parameter isn’t used as a target to the method, but rather as its argument. There’s a way to fix that and, in the process, remove the duplication of effort that crept in. Let’s see how.

Earlier we created two lambda expressions: one to order the ages of two people in ascending order and the other to do it in descending order. In so doing, we duplicated the logic and the effort, and we violated the DRY principle.[6] If all we want is a reverse of the comparison, the JDK has us covered with a reversed method on the Comparator, marked with a special method modifier called default. We’ll discuss default methods in A Peek into the default Methods, but here we’ll use the reversed method to remove the duplication.

```
Comparator<Person> compareAscending =
(person1, person2) -> person1.ageDifference(person2);
Comparator<Person> compareDescending = compareAscending.reversed();
```

We first created a Comparator, compareAscending, to compare the age of the people in ascending order using the lambda expression syntax. To reverse the order of comparison, instead of duplicating the effort, we can simply call reversed on the first Comparator to get another Comparator with the comparison order in reverse. Under the hood, the reversed method creates a comparator that swaps its parameters’ order of comparison. This makes the reversed method a higher-order method—this function creates and returns another functional expression with no side effect. Let’s use these two comparators in the code.

```
printPeople("Sorted in ascending order by age: ",
people.stream()
.sorted(compareAscending)
.collect(toList())
);
printPeople("Sorted in descending order by age: ",
people.stream()
.sorted(compareDescending)
.collect(toList())
);
```

It's becoming clear how the modern features in Java can greatly reduce code complexity and duplication of effort, but to get all the benefits, we have to explore the seemingly endless possibilities the JDK offers.

We've been sorting by age, but we could easily sort by name too. Let's sort in ascending alphabetical order by name; again, only the logic within the lambda expression needs to change.

```
compare/fpij/Compare.java

printPeople("Sorted in ascending order by name: ",
people.stream()
.sorted((person1, person2) ->
person1.getName().compareTo(person2.getName()))
.collect(toList()));
```

In the output we should now see the people with names listed in ascending alphabetical order.

```
Sorted in ascending order by name:
Greg - 35
Jane - 21
John - 20
Sara - 21
```

So far, our comparisons have worked on either the age or the name property. We can make the logic in the lambda expression more intelligent. For example, we could sort based on both name and age.

Let's pick the youngest person in the list. We could find the first person after we've sorted by age in ascending order. But we don't need to go that far; the Stream has us covered with a min method. This method also accepts a Comparator but returns the smallest object in the collection based on the given comparator.

Let's use that method.

```
compare/fpij/Compare.java

people.stream()
    .min(Person::ageDifference)
    .ifPresent(youngest -> System.out.println("Youngest: " + youngest));
```

We use the reference for the ageDifference method in the call to the min method. The min method returns an Optional because the list may be empty and so there may not be a youngest person. We then print the details of the youngest person that we get access to from the Optional using its ifPresent method.

Let's look at the output.

Youngest: John - 20

We can as easily find the oldest person in the list. Simply pass that method reference to a max method.

```
compare/fpij/Compare.java

people.stream()
    .max(Person::ageDifference)
    .ifPresent(eldest -> System.out.println("Eldest: " + eldest));
```

Let's look at the output for the name and age of the oldest in the list.

Eldest: Greg - 35

We saw how lambda expressions and method references make implementing comparators concise and easy. For its part, the JDK has evolved with a few convenience methods
