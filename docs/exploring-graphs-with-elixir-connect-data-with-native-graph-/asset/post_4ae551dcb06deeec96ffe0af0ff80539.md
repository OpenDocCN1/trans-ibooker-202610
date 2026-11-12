
The filter() function is useful to cherry-pick elements in a collection based on the criteria provided in the predicate passed to the function. If we would rather skip processing some elements, we could use the skip() or dropWhile() functions, as we'll see next.

## Skipping Values

Given a collection of values, if we want to skip processing a certain number of values or until a certain condition is met, we may use the `continue` statement along with the `if` statement in the traditional `for` loop. In the functional style, we don’t use `if` or `continue`. Instead, we can control the iteration using the `skip()` or `dropWhile()` functions.

If you like to skip the first four values in the collection and only process the rest, pass the number of values you want to skip to the `skip()` function, like so:

```
collections/fpij/SkipElements.java

friends.stream()
.skip(4)
.map(String::toUpperCase)
.forEach(System.out::println);
```

The call to `skip()` in the previous example ignores the first four values in the collection from further processing. Thus the output is the transformation of all values except the first four:

- SARA
- SCOTT

If, instead of skipping a certain number of values, you want to skip values until an element that meets a certain condition is encountered, use `dropWhile()`, which was introduced in the JDK 9. In the next example, we skip until we encounter a name whose length isn’t greater than 4.

```
friends.stream()
.dropWhile(name -> name.length() > 4)
.map(String::toUpperCase)
.forEach(System.out::println);
```

Whereas filter() works like a garage gate that opens or shuts for each approaching car, skip() and dropWhile() work more like a door that is closed initially, swings open, and then stays open.

In the previous code, since the first element of the collection, Brian, has a length greater than four, it is skipped or dropped from further processing. The next element, Nate, is of length 4, and thus processing starts from that element. Even though the element Scott is greater than four letters in length, it is processed since the door, so to say, has already swung open, as we see in this output:

- NATE
- NEAL
- RAJU
- SARA
- SCOTT

We discussed the equivalent of the imperative style continue in the functional style, but what about the popular break statement?

## Terminating Iterations

About an hour after I started teaching a class on functional programming for a team of developers, a programmer asked if we had any planned breaks. I quipped "In functional programming we don’t have breaks." While that comment was met with chuckles, terminating an iteration is serious business. Java provides at least two ways to exit an iteration before reaching the end of a collection—limit() and takeWhile(), where the latter was added in the JDK 9.

To only process the first three values in a collection, use the limit() function, like so:

```
collections/fpij/TakeElements.java

friends.stream()
  .limit(3)
  .map(String::toUpperCase)
  .forEach(System.out::println);
```

The output shows that only the first three values in the collection were processed and then the iteration was terminated:

BRIAN
NATE
NEAL

If, instead of a specific number of elements, we want to terminate the iteration upon encountering an element that meets a certain criteria, we can use takeWhile(), as in the following code:

```
friends.stream()
.takeWhile(name -> name.length() > 4)
.map(String::toUpperCase)
.forEach(System.out::println);
```

In this example, the processing continues as long as the length of the elements encountered is greater than four. The first element, Brian, meets that expectation but the second element, Nate, doesn't and thus the iteration is terminated right away as we see from the output:

BRIAN

Much like the skip() and dropWhile() functions, the limit() and takeWhile() methods also behave like a door. But instead of initially being shut, these methods behave like a door that is open and then shut forever when the given criteria isn't met.

The conciseness we've achieved by using lambda expressions so far is nice, but code duplication may sneak in quickly if we're not careful. Let's address that concern next.

## Reusing Lambda Expressions

Lambda expressions are deceivingly concise and it's easy to carelessly duplicate them in code. Duplicate code leads to poor-quality code that's hard to maintain; if we needed to make a change, we'd have to find and touch the relevant code in several places.

Avoiding duplication can also help improve performance. By keeping the code related to a piece of knowledge concentrated in one place, we can easily study its performance profile and make changes in one place to get better performance.

Now let's see how easy it is to fall into the duplication trap when using lambda expressions, and also consider ways to avoid it.

Suppose we have a few collections of names: friends, editors, comrades, and so on.

```
final List<String> friends =
Arrays.asList("Brian", "Nate", "Neal", "Raju", "Sara", "Scott");
```

```
final List<String> editors =
Arrays.asList("Brian", "Jackie", "John", "Mike");
```

```
final List<String> comrades =
Arrays.asList("Kate", "Ken", "Nick", "Paula", "Zach");
```

We want to filter out names that start with a certain letter. Let's first take a naive approach to this using the filter method.

collections/fpij/PickElementsMultipleCollection.java

```
final long countFriendsStartN =
friends.stream()
.filter(name -> name.startsWith("N"))
.count();
```

```
final long countEditorsStartN =
editors.stream()
.filter(name -> name.startsWith("N"))
.count();
```

```
final long countComradesStartN =
comrades.stream()
.filter(name -> name.startsWith("N"))
.count();
```

The lambda expressions made the code concise, but quietly led to duplicate code. In the previous example, one change to the lambda expression needs to change in more than one place—that's a no-no. Fortunately, we can assign lambda expressions to variables and reuse them, just like with objects.

The filter method, the receiver of the lambda expression in the previous example, takes a reference to a java.util.function.Predicate functional interface. Here, the Java compiler works its magic to synthesize an implementation of the Predicate’s test method from the given lambda expression. Rather than asking Java to synthesize the method at the argument-definition location, we can be more explicit. In this example, it's possible to store the lambda expression in an explicit reference of type Predicate and then pass it to the function; this is an easy way to remove the duplication.

Let's refactor the previous code to make it DRY.[4] (See the Don't Repeat Yourself—DRY—principle in The Pragmatic Programmer: From Journeyman to Master [HT00], by Andy Hunt and Dave Thomas.)

collections/fpij/PickElementsMultipleCollection.java

```
final Predicate<String> startsWithN = name -> name.startsWith("N");
```

```
final long countFriendsStartN =
    friends.stream()
        .filter(startsWithN)
        .count();
final long countEditorsStartN =
    editors.stream()
        .filter(startsWithN)
        .count();
final long countComradesStartN =
    comrades.stream()
        .filter(startsWithN)
        .count();
```

Rather than duplicate the lambda expression several times,
we created it once and stored it in a reference named `startsWithN`
of type `Predicate`. In the three calls to the `filter` method, the Java
compiler happily took the lambda expression stored in the
variable under the guise of the `Predicate` instance.

The new variable gently removed the duplication that sneaked
in. Unfortunately, it’s about to sneak back in with a
vengeance, as we’ll see next, and we need something a bit
more powerful to thwart it.

## Using Lexical Scoping and Closures

There's a misconception among some developers that using lambda expressions may introduce duplication and lower code quality. Contrary to that belief, even when the code gets more complicated, we still don't need to compromise code quality to enjoy the conciseness that lambda expressions give, as we'll see in this section.

We managed to reuse the lambda expression in the previous example, but duplication will sneak in quickly when we bring in another letter to match. Let's explore the problem further and then solve it using lexical scoping and closures.

### Duplication in Lambda Expressions

Let's pick the names that start with N or B from the *friends* collection of names. Continuing with the previous example, we may be tempted to write something like the following:

```
collections/fpij/PickDifferentNames.java

final Predicate<String> startsWithN = name -> name.startsWith("N");
final Predicate<String> startsWithB = name -> name.startsWith("B");

final long countFriendsStartN =
    friends.stream()
        .filter(startsWithN)
        .count();

final long countFriendsStartB =
    friends.stream()
        .filter(startsWithB)
        .count();
```

The first predicate tests if the name starts with an N and the second tests for a B. We pass these two instances to the two calls to the filter method, respectively. That seems reasonable,
