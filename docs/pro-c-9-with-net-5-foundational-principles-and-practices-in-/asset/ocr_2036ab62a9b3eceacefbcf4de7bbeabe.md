Alternatively, you could implement Accelerate() to recover automatically without needing to throw an exception in the first place. By and large, exceptions should be thrown only when a more terminal condition has been met (e.g., not finding a necessary file, failing to connect to a database, and the like) and not used as a logic flow mechanism. Deciding exactly what justifies throwing an exception is a design issue you must always contend with. For the current purposes, assume that asking a doomed automobile to increase its speed is cause to throw an exception.

Second, notice how the final else was removed from the method. When an exception is thrown (either by the framework or by manually using a throw statement), control is returned to the calling method (or by the catch block in a try catch). This eliminates the need for the final else. Whether or not you leave it in for readability is up to you and your coding standards.

In any case, if you were to rerun the application at this point using the previous logic in the top-level statements, the exception will eventually be thrown. As shown in the following output, the result of not handling this error is less than ideal, given you receive a verbose error dump followed by the program's termination (with your specific file path and line numbers):

```
**** Simple Exception Example ****
=> Creating a car and stepping on it!
Jamming...
=> CurrentSpeed = 30
=> CurrentSpeed = 40
=> CurrentSpeed = 50
=> CurrentSpeed = 60
=> CurrentSpeed = 70
=> CurrentSpeed = 80
=> CurrentSpeed = 90
=> CurrentSpeed = 100

Unhandled exception. System.Exception: Zippy has overheated!
  at SimpleException.Car.Accelerate(Int32 delta) in [path to file]\Car.cs:line 52
  at SimpleException.Program.Main(String[] args) in [path to file]\Program.cs:line 16
```

# Catching Exceptions

> Note  For those coming to .NET 5 from a Java background, understand that type members are not prototyped with the set of exceptions they may throw (in other words, .NET Core does not support checked exceptions). For better or for worse, you are not required to handle every exception thrown from a given member.

Because the Accelerate() method now throws an exception, the caller needs to be ready to handle the exception, should it occur. When you are invoking a method that may throw an exception, you make use of a try/catch block. After you have caught the exception object, you are able to invoke the members of the exception object to extract the details of the problem.

What you do with this data is largely up to you. You might want to log this information to a report file, write the data to the event log, email a system administrator, or display the problem to the end user. Here, you will simply dump the contents to the console window: