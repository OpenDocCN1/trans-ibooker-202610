
hood and isn't based on the way we write the function. We don't gain any performance benefit by writing multiple conditions in one lambda expression.

Let's take the previous code and rewrite it using two filter calls instead of one:

```
drivers.stream()
.filter(driver -> driver.getAge() > 21)
.filter(Driver::isDriversLicenseValid)
.map(Driver::getPrimaryCar)
.map(Car::getRegistration)
.forEach(System.out::println);
```

That looks a lot better than the previous version. Also, we managed to use a method reference in the second filter.

The version with multiple filter calls with one condition each has some serious benefits. First, it doesn't have any issues with performance, so we can set aside that fear. Second, the code is less noisy and much easier to read than the other version. Each line of code is very cohesive, does only one thing, and focuses on one condition. It's much easier to add a new condition—add a filter—without having to mess with one large condition. If we want to try out different behaviors by changing conditions, it's as trivial as commenting a filter line. It's also really easy to remove a condition. Finally, keeping the conditions small increases the opportunities to use method references, which can further improve readability.

Let's summarize the reasons to keep the conditions separate:

- The code is as performant as if we combined the conditions
- The code is less noisy
- The code is easier to read
- The code is easier to maintain
- It's easier to use method references to further improve readability
- It provides opportunities for further simplification

The code version with multiple filter calls with one condition is cohesive, easy to write, easy to read, easy to add or remove conditions, and overall much better compared to the other option, and it is the preferred way.

## Provide Good Domain-Specific Parameter Names

We write code once but read and change it many times, sometimes thousands of times. The considerations for the ease and cost of reading the code should outweigh those for writing the code the first time.

You've probably seen your share of poor names for lambda parameters. For some reason, x seems to be one of the most favorite bad names for parameters. Other poor choices seem to include p, k, and so on, with the worst offender being l—is it the number 1 or the letter l, the reader may wonder. It's likely that programmers picked up such practices from an example in a presentation, a book (such as this one), or a blog post. Whereas these short, cryptic names may be suitable for a quick example, unfortunately, such names don't help a team maintain production code.

A team who's in the practice of using meaningless variable names may create code like the following:

```
drivers.stream()
    .filter(x -> x.getAge() > 21) //Please avoid meaningless parameter names
    .filter(x -> x.isDriversLicenseValid())
    .map(x -> x.getPrimaryCar())
    .map(c -> c.getRegistration())
    .forEach(r -> System.out.println(r));
```

I have a theory about how these poor names creep into the code: maybe it's not the programmers who key them in. When they're momentarily distracted, their kitten pounces on the keyboard. How would anyone have the heart to change the variable names so purr-fectly chosen by Kitty?

Poor variables make the code hard to understand, which makes fixing the code rather an unpleasant and frustrating experience at the critical moments of fixing errors.

Resist the urge to give single-letter, meaningless variables. Instead, give domain-specific names for lambda parameters. The goal isn't to give lengthy names—they're as bad as the cryptic ones. Instead, choose a short name that clearly conveys what that variable represents. For example, instead of using `x`, `d`, `c`, and so on, we can write the previous code with descriptive names, like so:

```
drivers.stream()
    .filter(driver -> driver.getAge() > 21)
    .filter(driver -> driver.isDriversLicenseValid())
    .map(driver -> driver.getPrimaryCar())
    .map(car -> car.getRegistration())
    .forEach(registration -> System.out.println(registration));
```

Make giving good names a habit. Help your team refactor code that has poor names for parameters of lambda expressions. Cajole and coach them until they naturally start writing good parameter names. Continue to keep an eye on the names during code reviews.

## Use Type Inference for Parameters

Traditionally, in Java we're used to specifying the types of parameters. Starting with version 8, the language has been leaning more toward type inference. The Java compiler is smart enough to determine the type of the lambda expression parameters in most situations. That means less typing on the keyboard but without compromising type safety in code.

Many programmers who've been coding in Java for a long time, and especially those who mostly don't program in other languages as well, are often reluctant to use type inference. They feel comfortable when the types of the lambda expression parameters are specified and get frustrated reading code where the types aren't specified.

You can leverage the type inference capability of Java. The context in which the code is written often provides enough details about the type of the parameters. Combining that with good domain-specific names for the parameters often makes it easier to understand the code.

Suppose a programmer is comfortable writing the following code:

```
...
.filter((Driver x) -> x.getAge() > 21)
...
```

Encourage them to give a better name to the parameter and drop the type. As a bonus, you can drop the parentheses around the parameter as well if the lambda expression takes only one parameter.

```
...
.filter(driver -> driver.getAge() > 21)
...
```

Get used to using type inference for lambda expression parameters and provide domain-specific names for parameter names. If the code reads equally well or better without the type, but with well-chosen parameter names, leave the type out.

## Side Effects in Functional Pipelines

A common mistake many programmers make is mutating shared variables from the functional pipeline. That's a big no-no.

Purely functional programming languages, like Haskell, don't permit mutability. The compilers of such languages will disallow writing code, let alone lambda expressions that mutate data. Java is a hybrid language and the functional programming capabilities were added almost two decades after the language was created. Thus, the compiler for the most part won't complain if it finds impure lambda expressions—but we should.

A programmer once emailed me a snippet of code that had been working fine, but its behavior had become rather unpredictable after the most recent change they had made. They wanted another pair of eyes to look into the issue. A quick glance through the code revealed the culprit. In the last step of the functional pipeline, in the forEach, they added the result to a collection defined outside the functional pipeline. The code seemed OK until the last change where they converted the call to stream to a call to parallelStream instead. In the multithreaded scenario, the code was running into race conditions and some data was missing from the result.

A lambda expression is impure if it modifies an externally visible variable. It's also impure if it depends on an external variable that may possibly change. In other words, a pure lambda expression does no evil—that is mutate—and also sees no evil.

Parallel execution isn't the only situation that will run into issues with impure lambda expressions. Even sequential execution will cause confusion due to lazy evaluation. Let's dig into this to see the issue in action.

Suppose we have a lambda expression that depends on a field in a class. The field isn't defined final and, furthermore, it's modified in the run method. Let's take a look at the code:

```java
public class Impure {
  private int factor = 2;

  public void run() {
    var numbers = List.of(1, 2, 3);

    var stream = numbers.stream()
      .map(number -> number * factor);

    factor = 0;

    stream.forEach(System.out::println);
  }

  public static void main(String[] args) {
    new Impure().run();
  }
}
```

In the run method we iterate over a list of numbers and, using the Stream pipeline, transform the data into a product of the number and the value in the factor field. Then, before the execution of the final step in the stream, we set the value of the factor variable to 0.
