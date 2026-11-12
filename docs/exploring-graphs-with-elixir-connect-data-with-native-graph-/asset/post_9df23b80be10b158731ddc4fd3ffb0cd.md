
We're focused on the design of CalculateNAV; we want to quickly run it and get feedback. When test-driving the design of this class using unit tests, we don't want to depend on external services—that would make the tests brittle. We want to stub the web service.

In general, though, creating a stub (or mock) in Java can be arduous and we often rely on libraries. Thanks to the lambda expressions and their fluency, this just got easier. Let’s create a unit test to try out our computeStockWorth method, stubbing away the implementation of the apply method.

```
designing/fpij/CalculateNAVTest.java
public class CalculateNAVTest {
@Test
public void computeStockWorth() {
    final CalculateNAV calculateNAV =
        new CalculateNAV(ticker -> new BigDecimal("6.01"));
    BigDecimal expected = new BigDecimal("6010.00");
    BigDecimal actual = calculateNAV.computeStockWorth("GOOG", 1000);
    BigDecimal delta = actual.subtract(expected);

    assertEquals(0, delta.doubleValue(), 0.001);
}

//...
}
```

Creating the test was effortless. We passed a lambda expression to the CalculateNAV constructor—this is lightweight stubbing of the web service. From within the lambda expression, we returned a contrived value for the price in response to the call. Then, in the test, we asserted that the computeStockWorth returned the expected result, within a tolerance of 0.001, for a given ticker and the number of shares.

We can also assert that the ticker passed to the lambda expression is the right one. We can take this further quite easily to add other tests—for example, an exception test to ensure code properly handles web-service failures. We can do all this without spending the time to create the code to talk to the web service, but instead merely stubbing away the implementation.

Let's run the test and ensure JUnit reports that it passed.

```
...
Test run finished after 53 ms
[	3 containers found ]
[	0 containers skipped ]
[	3 containers started ]
[	0 containers aborted ]
[	3 containers successful ]
[	0 containers failed ]
[	1 tests found ]
[	0 tests skipped ]
[	1 tests started ]
[	0 tests aborted ]
[	1 tests successful ]
[	0 tests failed ]
```

Testing the code was quick; we easily stubbed away the dependency to the web service, which helped to rapidly develop and test the code. But we can’t call it done until we run it with a real web service. That’s our next task.

## Integrating with a Web Service

Let’s first invoke the computeStockWorth on an instance of our CalculateNAV:

designing/fpij/CalculateNAV.java

```
final CalculateNAV calculateNav = new CalculateNAV(FinanceData::getPrice);
System.out.println(String.format("100 shares of Apple worth: $%.2f",
calculateNav.computeStockWorth("AAPL", 100)));
```

Rather than stubbing away the implementation here, we pass a method reference to a **FinanceData**'s `getPrice` method. We need to implement that method to complete this task.

Talking to a real web service is easy if we can find a service that will provide current stock prices. For this example, we'll use the trial URL from Unicorn Data Services[10] to get the stock price for one ticker symbol. The value for the `api_token` that's hardcoded in the next code works only for select ticker symbols. If you would like to try the example for different stock ticker symbols, register with the site to get your own value for the `api_token` or use an alternate service you like.

### designing/fpij/FinanceData.java

```
public class FinanceData {
public static BigDecimal getPrice(final String ticker) {
try {
final String URL =
"https://eodhistoricaldata.com/api/eod/%s.US?%s&s&s&%s";
final URL url = new URI(String.format(URL,
ticker,
"fmt=json",
"filter=last_close",
"api_token=0eAFFmLifG5orCUuwAKQ8l4WWFQ67YX")).toURL();
try(Scanner scanner = new Scanner(url.openStream())) {
return new BigDecimal(scanner.nextLine());
} } catch(Exception ex) {
throw new RuntimeException(ex);
}
}
}
```

In the `getPrice` method we send a request to the web service asking only for the latest closing price. We convert the `String` data returned by the service into a BigDecimal and return. Let’s now exercise the call to the computeStockWorth method we wrote.

100 shares of Apple worth: $14264.00

In the output, we see the worth of 100 shares of Apple stock at the time the book was written. If we run the code now, the value may be something insanely higher—let’s hope.

We have to figure out a way to deal with exceptions that may arise when calling the web service. Suppressing exceptions with empty catch blocks or printing them in arbitrary places is pure evil. Rather than dealing with exceptions within lambda expressions, we have to find the right place to handle them. That requires us to rethrow the exceptions so they can be handled upstream.

Lambda expressions and method references can throw checked exceptions only if those exceptions are declared using the throws clause in the abstract method of the functional interface they stand in for. Since the Function interface’s apply method doesn’t specify any expected exceptions, we can’t directly throw the checked exception in this example. As a work-around, we wrapped the exception into the unchecked RuntimeException. The lambda expression now simply passes the exception through, and we’ll have to handle it upstream in the code. Any runtime exception that’s not handled, of course, will abruptly terminate the application.

We delegated part of the responsibility of our class to lambda expressions and method references in this example, as the figure demonstrates.

![](img/f9269002f2414fff96e5e6b4dc20839f_178_0.png)

The approach we took helped us to decouple the implementation of the call to the web service from the method that used the data that is fetched from the service. This also helped us to easily stub the service to quickly test our calculate method. Next, we’ll use lambda expressions to combine multiple behaviors.

## Decorating Using Lambda Expressions

So far we've avoided creating implementation classes to support the delegate interfaces. We needed simple implementations for these interfaces, so that worked out fairly well. Let's increase our demands on these implementations, ask them to interact with multiple delegates, and see how lambda expressions handle that.

Delegation is great, but we can take it further if we can chain delegates to add behavior. We can then create discrete flavors of behavior and sprinkle them onto the classes, like the toppings at the ice cream shop.

The decorator pattern is powerful, but programmers often hesitate to use it due to the burdensome hierarchy of classes and interfaces—like FilterInputStream and FilterOutputStream in the JDK—that are used to implement the pattern (see Gamma et al.'s Design Patterns: Elements of Reusable Object-Oriented Software [GHJV95]). We can now realize this pattern with less effort in Java.

In the next example, we'll chain delegates—using lambda expressions, of course—to realize the decorator pattern. This will help us see how we can create a flexible and extensible lightweight design with just a few lines of code.

### Designing Filters

Adding filters to a camera is a good example of chaining behavior or responsibilities. We may start with no filters, then add a filter, and then a few more. We want our design to be flexible so that it doesn’t care how many filters we have. Let's create an example Camera that'll use filters to process the captured colors.

designing/fpij/Camera.java

```
@SuppressWarnings("unchecked")
public class Camera {
private Function<Color, Color> filter;

public Color capture(final Color inputColor) {
    final Color processedColor = filter.apply(inputColor);
    //... more processing of color...
    return processedColor;
}

//... other functions that use the filter ...
}
```

The Camera has a field for the filter, a reference to an instance of Function (much like the delegation example we saw earlier). This filter function can receive a Color and return a processed Color. Looking at what we have so far, the class may appear to use only one filter, but with a design tweak, we'll make it work with multiple filters.

To achieve this flexibility, we'll use a method that belongs to a special type called default methods. In addition to abstract methods, interfaces can have methods with implementation, marked as default. These methods are automatically added to the classes that implement the interfaces. This was done as a trick in Java to enhance existing classes with new methods without having to change each one of them. In addition, interfaces can have static methods.

In addition to the apply abstract method, the Function interface has a default method, andThen, to combine or chain multiple Functions.
