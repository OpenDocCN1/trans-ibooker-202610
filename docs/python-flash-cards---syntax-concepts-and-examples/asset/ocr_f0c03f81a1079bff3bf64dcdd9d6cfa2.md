The unittest module provides tools for code testing, such as the TestCase class. This class’s assertion methods ensure your code generates proper results; its setUp() method allows you to use the same initial conditions in multiple tests.

To test a function or class, import the test code into a new file and write a test case that inherits from unittest.TestCase. Then write methods to test code behavior. Use the module when modifying code, fixing bugs, and adding features. You can make these assertions in your testing code:

| Assertion | Description |
|---|---|
| `assertEqual()` | Two values match |
| `assertNotEqual()` | Two values don’t match |
| `assertTrue()` | A value is True |
| `assertFalse()` | A value is False |
| `assertIn()` | An item is in a list |
| `assertnotIn()` | An item is not in a list |