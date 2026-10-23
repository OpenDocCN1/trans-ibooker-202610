
To execute, run the java command with Java 11. Java will compile the source file on the fly and run the program with the class file on the classpath. Any compilation error that might occur will be rendered on the console:

```
$ java -version
openjdk version "11" 2018-09-25
OpenJDK Runtime Environment 18.9 (build 11+28)
OpenJDK 64-Bit Server VM 18.9 (build 11+28, mixed mode)

$ java HelloWorld.java
Hello World!
```

Launching a single-file program without compilation is very convenient, but there are a couple limitations that you need to be aware of. With Java 11, the program cannot use any external dependencies other than the java.base module. Furthermore, you can launch only a single-file program. Calling methods from other Java source files is currently not supported, but you can define multiple classes in the same file if needed. I personally see this feature as a helpful tool for rapid prototyping.

In the next section, you learn about the new methods that have been introduced to existing library classes.

## New Library Methods for Strings, Collections, and Files

All too often, Java developers must rely on external libraries like Google Guava or Apache Commons for convenience methods that they otherwise would need to copy and paste from one project to another. The implementations might be small, but they do create additional API surface that needs to be maintained and tested. Java 11 adds convenience methods to the String, Collections, and Files APIs.

Let’s explore the new functionality by example. For the fun of it, let’s write some JUnit tests that apply the new APIs and verify their correct behavior for different use cases.

## STRING API ENHANCEMENTS

The String API adds four new convenience methods. Let’s have a look at each one of them by example.

### Repeating a string

The first method we look at is String.repeat(Integer). Judging by the name, you might have already guessed the purpose of the functionality. It simply repeats a string n times.

I can think of more than one situation in which the repeat method could come in handy. Imagine that you need to build a data table with padded whitespaces between key/value pairs to improve its readability. The following test case demonstrates the use of the method for that very scenario:

```
@Test
void canRepeatString() {
    assertEquals("CPU Usage:               5%",
        renderInfo("CPU Usage:", "5%"));
    assertEquals("Memory Usage:          9.14 MB",
        renderInfo("Memory Usage:", "9.14 MB"));
    assertEquals("Free Disk:             96.5 GB",
        renderInfo("Free Disk:", "96.5 GB"));
}

private String renderedInfo(String title, String value) {
    return title + " ".repeat(30 - title.length()
        - value.length()) + value;
}
```

### Checking for empty or whitespaces

I am sure you have needed to implement data validation at some point of your career. Suppose that you want to verify the validity of a value entered into a web-based input field. Of course, we do not want to allow blank values.

Java 11 introduces the method String.isBlank(), which indicates whether a string is empty or contains only whitespaces. Now that this functionality has become a Java core feature, there’s no more need to pull in Apache StringUtils as external dependencies. You can see its use in the following test case:

```
@Test
void canCheckIfStringContainsWhitespaces() {
    String nameFormFieldWithoutWhitespace = "Duke";
    String nameFormFieldWithWhitespace = "   ";
    assertFalse(nameFormFieldWithoutWhitespace.isBlank());
    assertTrue(nameFormFieldWithWhitespace.isBlank());
}
```

### Removing leading and trailing whitespaces

Validating strings goes hand in hand with sanitizing strings. Developers often encounter data that needs to be cleaned to extract the actual information. This might involve filtering characters that are hidden, erroneous, or not useful.

The newly introduced method `String.strip()` takes care of removing leading and trailing whitespaces. You can be even more specific by removing just the leading characters by using `String.stripLeading()` or just the trailing characters by using `String.stripTrailing()`. The following test case demonstrates the use of all three methods:

```
@Test
void canStripStringOfLeadingAndTrailingWhitespaces() {
    String nameFormField = "   Java Duke   ";
    assertEquals("Java Duke", nameFormField.strip());
    assertEquals("Java Duke", nameFormField.stripLeading());
    assertEquals("   Java Duke", nameFormField.stripTrailing());
}
```

### Processing multiline strings

Processing multiline strings line by line can be tedious and memory intensive. First, you must split the string based on line terminators. Next, you need to iterate over each line. For software systems that handle the operation for large texts, memory consumption can be a big concern.

The method `String.lines()` introduced in Java 11 lets you process multiline texts as a Stream. A Stream represents a sequence of elements that can be processed sequentially without having to load all of the elements into memory at once. The following shows a test case that uses the method in practice:

```
@Test
void canStreamLines() {
    String testString = "This\nis\na\ntest";
    List<String> lines = new ArrayList<>();
    testString.lines().forEach(line -> lines.add(line));
    assertEquals(List.of("This", "is", "a", "test"), lines);
}
```

## COLLECTIONS API ENHANCEMENTS

In the past, turning a collection into an array was cumbersome. You were required to provide a new instance of the array, its generic type, and the final size of the array.

Java 11 makes the conversion more convenient. Instead of passing an array instance, you now can provide a function by using the method `Collection.toArray(IntFunction<T[]>)`. The following code example compares the old and the new way for converting a collection to an array:

```
@Test
void canTurnListIntoArray() {
    List<String> months = new ArrayList<>();
    months.add("January");
    months.add("February");
    months.add("March");
    assertArrayEquals(new String[]
        { "January", "February", "March" },
        months.toArray(new String[months.size()]));
    assertArrayEquals(new String[]
        { "January", "February", "March" },
        months.toArray(String[]::new));
}
```

1 Converting a collection to an array pre-Java 11.

2 Converting a collection to an array using the new method in Java 11.

## FILES API ENHANCEMENTS

For developers, reading and writing files is a very common operation. Pre-Java 11 code requires you to write unnecessary boilerplate code. To work around this issue, projects will usually introduce a reusable utility method or resort to one of the many open source libraries.

Java 11 adds the methods Files.readString(Path) and Files.writeString(Path, CharSequence, OpenOption) with various overloads, which make it much easier to read and write files. The following code snippet shows both methods in action:

```
@Test
void canReadString() throws URISyntaxException, IOException {
    URI txtFileUri = getClass().getClassLoader()
        .getResource("helloworld.txt").toURI();
    String content = Files.readString(Path.of(txtFileUri),
        Charset.defaultCharset());
    assertEquals("Hello World!", content);
}

@Test
void canWriteString() throws IOException {
    Path tmpFilePath = Path.of(
        File.createTempFile("tempFile", ".tmp").toURI());
    Path returnedFilePath = Files.writeString(tmpFilePath,
        "Hello World!", Charset.defaultCharset(),
        StandardOpenOption.WRITE);
    assertEquals(tmpFilePath, returnedFilePath);
}
```

## Enhancements to Optional and Predicate

Java 8 added the `Optional` type. This type is a single-value container that contains an optional value. Its main purpose is to avoid cluttering the code with unnecessary null checks. You can check whether a value is present within the container without actually retrieving it by calling the `isPresent()` method.

In the latest version of Java, you can also ask for the inverse: “Is the value empty?” Instead of using the negation of the `isPresent()` method, you can now just use the `isEmpty()` method to make the code easier to understand. The following test case compares the use of both methods:

```
@Test
void canCheckOptionalForEmpty() {
    String payDay = null;
    assertTrue(!Optional.ofNullable(payDay).isPresent());    ①
    assertTrue(Optional.ofNullable(payDay).isEmpty());    ②
    payDay = "Monday";
    assertFalse(!Optional.ofNullable(payDay).isPresent());    ①
    assertFalse(Optional.ofNullable(payDay).isEmpty());    ②
}
```

① Checking whether a value in `Optional` is present pre-Java 11.

② Checking whether a value in `Optional` is present in Java 11.

A `Predicate` is often used to filter elements in a collection of objects. Java 11 adds the static method `Predicate.not(Predicate)`, which returns a predefined `Predicate` instance that negates the given `Predicate`.

Suppose that you want to filter a stream of strings, such as certain months. The new `Predicate` method can be enormously helpful with filtering all months that do not fulfill a certain condition. In the code that follows, you’ll find an example that filters out all months that do not begin with the letter “M.” That’s far more expressive than having to pass `(Predicate<String>) month -> month.startsWith("M")).negate()` to the `filter` method.

```
@Test
void canUsePredicateNotAsFilter() {
    List<String> months = List.of("January", "February", "March");
```
