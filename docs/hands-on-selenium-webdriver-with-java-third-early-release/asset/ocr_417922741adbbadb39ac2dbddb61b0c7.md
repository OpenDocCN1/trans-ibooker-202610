```
```java
@EmptySource
@ParameterizedTest
@ValueSource(strings = { "three", "four" })
void test(String argument) {
    log.debug("arg: {}", argument);
}
```
```

A null plus an empty argument

```
```java
@NullAndEmptySource
@ParameterizedTest
@ValueSource(strings = { "five", "six" })
void test(String arg) {
    log.debug("arg: {}", arg);
}
```
```

Example 8-3 illustrates the Jupiter version of the same parameterized test shown in previous examples. We can use different argument providers to implement this parameterized test. In this case, we use @MethodSource to return a stream of arguments. An alternative that might fit well for this test is using @CsvSource to embed the input data and expected outcome as CSV.

# *Example 8-3. Parameterized test using JUnit 5*

```
```java
class ParameterizedJupiterTest {

    WebDriver driver;

    @BeforeEach
    void setup() {
```
```