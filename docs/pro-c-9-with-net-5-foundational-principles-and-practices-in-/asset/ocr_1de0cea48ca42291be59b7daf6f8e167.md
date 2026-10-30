```
Console.WriteLine("\nBefore by value call, Person is:");
fred.Display();

SendAPersonByValue(fred);
Console.WriteLine("\nAfter by value call, Person is:");
fred.Display();
Console.ReadLine();
```

The following is the output of this call:

```
**** Passing Person object by value ****

Before by value call, Person is:
Name: Fred, Age: 12

After by value call, Person is:
Name: Fred, Age: 99
```

As you can see, the value of personAge has been modified. This behavior, discussed earlier, should make more sense now that you understand the way reference types work. Given that you were able to change the state of the incoming Person, what was copied? The answer: a copy of the reference to the caller’s object. Therefore, as the SendAPersonByValue() method is pointing to the same object as the caller, it is possible to alter the object’s state data. What is not possible is to reassign what the reference is pointing to.

# Passing Reference Types by Reference

Now assume you have a SendAPersonByReference() method, which passes a reference type by reference (note the ref parameter modifier).

```
static void SendAPersonByReference(ref Person p)
{
    // Change some data of "p".
    p.personAge = 555;

    // "p" is now pointing to a new object on the heap!
    p = new Person("Nikki", 999);
}
```

As you might expect, this allows complete flexibility of how the callee is able to manipulate the incoming parameter. Not only can the callee change the state of the object, but if it so chooses, it may also reassign the reference to a new Person object. Now ponder the following updated code:

```
// Passing ref-types by ref.
Console.WriteLine("***** Passing Person object by reference *****");
...
```

```
Person mel = new Person("Mel", 23);
Console.WriteLine("Before by ref call, Person is:");
mel.Display();
```