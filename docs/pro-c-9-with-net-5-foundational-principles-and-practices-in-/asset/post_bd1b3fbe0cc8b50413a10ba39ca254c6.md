
```
=> CurrentSpeed = 50
=> CurrentSpeed = 60
=> CurrentSpeed = 70
=> CurrentSpeed = 80
=> CurrentSpeed = 90
=> CurrentSpeed = 100
Zippy has overheated!
Zippy is out of order...
```

## Throwing a General Exception

Now that you have a functional Car class, I’ll demonstrate the simplest way to throw an exception. The current implementation of Accelerate() simply displays an error message if the caller attempts to speed up the Car beyond its upper limit.

To retrofit this method to throw an exception if the user attempts to speed up the automobile after it has met its maker, you want to create and configure a new instance of the System.Exception class, setting the value of the read-only Message property via the class constructor. When you want to send the exception object back to the caller, use the C# throw keyword. Here is the relevant code update to the Accelerate() method:

```
// This time, throw an exception if the user speeds up beyond MaxSpeed.
public void Accelerate(int delta)
{
    if (_carIsDead)
    {
        Console.WriteLine("{0} is out of order...", PetName);
    }
    else
    {
        CurrentSpeed += delta;
        if (CurrentSpeed >= MaxSpeed)
        {
            CurrentSpeed = 0;
            _carIsDead = true;

            // Use the "throw" keyword to raise an exception.
            throw new Exception($"{PetName} has overheated!");
        }
        Console.WriteLine("=> CurrentSpeed = {0}", CurrentSpeed);
    }
}
```

Before examining how a caller would catch this exception, let’s look at a few points of interest. First, when you are throwing an exception, it is always up to you to decide exactly what constitutes the error in question and when an exception should be thrown. Here, you are making the assumption that if the program attempts to increase the speed of a Car object beyond the maximum, a System.Exception object should be thrown to indicate the Accelerate() method cannot continue (which may or may not be a valid assumption; this will be a judgment call on your part based on the application you are creating).

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

## Catching Exceptions

> Note For those coming to .NET 5 from a Java background, understand that type members are not prototyped with the set of exceptions they may throw (in other words, .NET Core does not support checked exceptions). For better or for worse, you are not required to handle every exception thrown from a given member.

Because the Accelerate() method now throws an exception, the caller needs to be ready to handle the exception, should it occur. When you are invoking a method that may throw an exception, you make use of a try/catch block. After you have caught the exception object, you are able to invoke the members of the exception object to extract the details of the problem.

What you do with this data is largely up to you. You might want to log this information to a report file, write the data to the event log, email a system administrator, or display the problem to the end user. Here, you will simply dump the contents to the console window:

```
// Handle the thrown exception.
Console.WriteLine("***** Simple Exception Example *****");
Console.WriteLine("=> Creating a car and stepping on it!");
Car myCar = new Car("Zippy", 20);
myCar.CrankTunes(true);

// Speed up past the car's max speed to
// trigger the exception.
try
{
    for(int i = 0; i < 10; i++)
    {
        myCar.Accelerate(10);
    }
}
catch(Exception e)
{
    Console.WriteLine("\n*** Error! ***");
    Console.WriteLine("Method: {0}", e.TargetSite);
    Console.WriteLine("Message: {0}", e.Message);
    Console.WriteLine("Source: {0}", e.Source);
}
// The error has been handled, processing continues with the next statement.
Console.WriteLine("\n***** Out of exception logic *****");
Console.ReadLine();
```

In essence, a try block is a section of statements that may throw an exception during execution. If an exception is detected, the flow of program execution is sent to the appropriate catch block. On the other hand, if the code within a try block does not trigger an exception, the catch block is skipped entirely, and all is right with the world. The following output shows a test run of this program:

```
***** Simple Exception Example *****
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

*** Error! ***
Method: Void Accelerate(Int32)
Message: Zippy has overheated!
Source: SimpleException

***** Out of exception logic *****
```

As you can see, after an exception has been handled, the application is free to continue from the point after the catch block. In some circumstances, a given exception could be critical enough to warrant the termination of the application. However, in a good number of cases, the logic within the exception handler will ensure the application can continue on its merry way (although it could be slightly less functional, such as not being able to connect to a remote data source).

## Throw As Expression (New 7.0)

Prior to C# 7, throw was a statement, which meant you could throw an exception only where statements are allowed. With C# 7.0 and later, throw is available as an expression as well and can be called anywhere expressions are allowed.

## Configuring the State of an Exception

Currently, the System.Exception object configured within the Accelerate() method simply establishes a value exposed to the Message property (via a constructor parameter). As shown previously in Table 7-1, however, the Exception class also supplies a number of additional members (TargetSite, StackTrace, HelpLink, and Data) that can be useful in further qualifying the nature of the problem. To spruce up the current example, let’s examine further details of these members on a case-by-case basis.

### The TargetSite Property

The System.Exception.TargetSite property allows you to determine various details about the method that threw a given exception. As shown in the previous code example, printing the value of TargetSite will display the return type, name, and parameter types of the method that threw the exception. However, TargetSite does not return just a vanilla-flavored string but rather a strongly typed System.Reflection.MethodBase object. This type can be used to gather numerous details regarding the offending method, as well as the class that defines the offending method. To illustrate, assume the previous catch logic has been updated as follows:

```
...
// TargetSite actually returns a MethodBase object.
catch(Exception e)
{
    Console.WriteLine("\n*** Error! ***");
    Console.WriteLine("Member name: {0}", e.TargetSite);
    Console.WriteLine("Class defining member: {0}",
        e.TargetSite.DeclaringType);
    Console.WriteLine("Member type: {0}",
        e.TargetSite.MemberType);
    Console.WriteLine("Message: {0}", e.Message);
    Console.WriteLine("Source: {0}", e.Source);
}
Console.WriteLine("\n**** Out of exception logic ****");
Console.ReadLine();
```
