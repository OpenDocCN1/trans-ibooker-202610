+   1. We declare the page class as an attribute in the test class.
+   2. We create the page object passing the WebDriver instance.
+   3. We invoke the method with defined by the page class to make the login operation.
+   4. We verify the success box is available on the resulting web page using a method provided by the page object.

This approach is a handy start for improving the maintainability of our tests because now, all the logic related to the login web page is centralized as a reusable class. Nevertheless, the code in the page classes is still brittle. For instance, imagine we need to implement a negative test for the login page, i.e., a login attempt using incorrect credentials. Example 7-5 seems a reasonable way to do that, given the current implementation of the page class. Nevertheless, if you run this test, you will discover that the test fails due to a NoSuchElementException exception. The next section explains how to solve this potential problem by creating more robust page objects.

# Example 7-5. Test using the basic page class to implement a failed login

```
@Test
void testBasicLoginFailure() {
    login.with("bad-user", "bad-password");
    assertThat(login.successBoxPresent()).isFalse();
}
```

# Robust Page Objects

The example presented in the previous sections improves code maintainability since the page operations are encapsulated in a single class instead of having them scattered throughout the test suite. Nevertheless, there are different ways to enhance the previous page classes implementation. First of all, our SUT is likely to have several web pages, and not only one. For this reason, it is a common strategy to follow an