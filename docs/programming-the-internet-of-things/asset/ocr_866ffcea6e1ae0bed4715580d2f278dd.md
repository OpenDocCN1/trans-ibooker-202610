# NOTE

In your GDA project, you’ve likely noticed that Maven creates two directory structures for your source code: one for Java source code located in ./src/main/java, and another Java unit test code located in ./src/test/java. Since this is the default convention for Maven projects, to leave it as is.

In your CDA project, the code path structure is a bit different than the GDA project: the ./src/main/java and ./src/test/java are missing. That probably seems quite sensible, since this is a Python project and not Java! All the source code is contained within ./src/programmingtheiot, while all the tests are contained within ./tests/programmingtheiot.

Although I provide a bunch of unit tests for you to use, we’ll dig into creating some of your own in Chapter 2. If you’re new to this area of development, I’ve provided a couple simple code snippets below in both Java and Python to help you become more familiar with the code structure. Here’s a simple unit test in Java using JUnit ⁶ that verifies the method addTwoIntegers() behaves as expected:

```
@Test
```

```
public int testAddTwoIntegers(int a, int b)
```

```
{
```

```
MyClass mc = new MyClass();
```