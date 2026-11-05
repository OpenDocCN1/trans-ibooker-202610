```
// data field
public int AngerLevel;

// method
public void Poke()
{
    AngerLevel++;

    if (AngerLevel >= 3)
    {
        // if something is listening...
        if (Shout != null)
        {
            // ...then call the delegate
            Shout(this, EventArgs.Empty);
        }
    }
}
```

Checking whether an object is not null before calling one of its methods is very common. C# 6.0 and later allows null checks to be simplified inline using a ? symbol before the . operator, as shown in the following code:

```
Shout?.Invoke(this, EventArgs.Empty);
```

+   2. At the bottom of Program.cs, add a method with a matching signature that gets a reference to the Person object from the sender parameter and outputs some information about them, as shown in the following code:

```
static void Harry_Shout(object? sender, EventArgs e)
{
    if (sender is null) return;
    Person p = (Person)sender;
    WriteLine($"{p.Name} is this angry: {p.AngerLevel}.");
}
```

Microsoft's convention for method names that handle events is ObjectName_EventName.

+   3. In Program.cs, add a statement to assign the method to the delegate field, as shown in the following code:

```
harry.Shout = Harry_Shout;
```

+   4. Add statements to call the Poke method four times, after assigning the method to the Shout event, as shown highlighted in the following code:

```
harry.Shout = Harry_Shout;
harry.Poke();
harry.Poke();
harry.Poke();
harry.Poke();
```