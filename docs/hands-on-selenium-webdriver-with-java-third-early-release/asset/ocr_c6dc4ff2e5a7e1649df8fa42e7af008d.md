# EXCEPTIONS IN JAVA

In Java, an exception is an event that disrupts the execution of a program. There is a class hierarchy used to model different exception types in the standard Java API. The root of this hierarchy is the `Throwable` class. It has two subtypes:

+   - `Error`: Irrecoverable problems. As a general rule, applications crash rather than try to handle these severe errors. Some example are `OutOfMemoryError` or `StackOverflowError`.
+   - `Exception`: Recoverable problems. Applications can handle these exceptions by using `try-catch` blocks. There are two types of `Exception` classes:
+     - *Checked* exceptions: Classes that directly inherit the `Throwable` class (except *RuntimeException* and *Error*). These exceptions are validated at compile-time, and therefore, we must handle them with `try-catch` blocks or rethrow using `throws`. Examples of check exceptions are `IOException` or `MalformedURLException`.
+     - *Unchecked* exceptions: Classes that inherit from the `RuntimeException` class, which is a subtype of `Exception`. These exceptions do not require to be handled with `try-catch` nor rethrowing with `throws`. Some examples of unchecked exceptions are `NullPointerException` or `ArrayIndexOutOfBoundException`.