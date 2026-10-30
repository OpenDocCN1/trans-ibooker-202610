With this, you would now update the CarEngineHandler delegate type definition as follows (the events would be unchanged):

```
public class Car
{
    public delegate void CarEngineHandler(object sender, CarEventArgs e);
    ...
}
```

Here, when firing the events from within the Accelerate() method, you would now need to supply a reference to the current Car (via the this keyword) and an instance of the CarEventArgs type. For example, consider the following partial update:

```
public void Accelerate(int delta)
{
    // If the car is dead, fire Exploded event.
    if (carIsDead)
    {
        Exploded?.Invoke(this, new CarEventArgs("Sorry, this car is dead..."));
    }
    ...
}
```

On the caller's side, all you would need to do is update your event handlers to receive the incoming parameters and obtain the message via the read-only field. Here's an example:

```
static void CarAboutToBlow(object sender, CarEventArgs e)
{
    Console.WriteLine($"{sender} says: {e.msg}");
}
```

If the receiver wants to interact with the object that sent the event, you can explicitly cast the System.Object. From this reference, you can make use of any public member of the object that sent the event notification.

```
static void CarAboutToBlow(object sender, CarEventArgs e)
{
    // Just to be safe, perform a
    // runtime check before casting.
    if (sender is Car c)
    {
        Console.WriteLine(
            $"Critical Message from {c.PetName}: {e.msg}");
    }
}
```