```
{
    CurrentSpeed = currSp;
    MaxSpeed = maxSp;
    PetName = name;
}
}
```

Now, consider the following updates, which address the first three points:

```
public class Car
{

// 1) Define a delegate type.
public delegate void CarEngineHandler(string msgForCaller);

// 2) Define a member variable of this delegate.
private CarEngineHandler _listOfHandlers;

// 3) Add registration function for the caller.
public void RegisterWithCarEngine(CarEngineHandler methodToCall)
{
    _listOfHandlers = methodToCall;
}
}
```

Notice in this example that you define the delegate types directly within the scope of the Car class, which is certainly not necessary but does help enforce the idea that the delegate works naturally with this class. The delegate type, CarEngineHandler, can point to any method taking a single string as input and void as a return value.

Next, note that you declare a private member variable of your delegate type (named _listOfHandlers) and a helper function (named RegisterWithCarEngine()) that allows the caller to assign a method to the delegate's invocation list.

> Note Strictly speaking, you could have defined your delegate member variable as public, therefore avoiding the need to create additional registration methods. However, by defining the delegate member variable as private, you are enforcing encapsulation services and providing a more type-safe solution. You'll revisit the risk of public delegate member variables later in this chapter when you look at the C# event keyword.

At this point, you need to create the Accelerate() method. Recall, the point here is to allow a Car object to send engine-related messages to any subscribed listener. Here is the update:

```
// 4) Implement the Accelerate() method to invoke the delegate's
// invocation list under the correct circumstances.
public void Accelerate(int delta)
{
    // If this car is "dead," send dead message.
    if (_carIsDead)
```