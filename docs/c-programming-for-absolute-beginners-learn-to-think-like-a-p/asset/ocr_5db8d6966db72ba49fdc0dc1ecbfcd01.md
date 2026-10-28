```
```csharp
if (dayOfWeek == DayOfWeek.Friday)
    spanish = "Viernes";
if (dayOfWeek == DayOfWeek.Saturday)
    spanish = "Sábado";
if (dayOfWeek == DayOfWeek.Sunday)
    spanish = "Domingo";

// Output
Console.WriteLine(spanish);
if (enteredDate == new DateTime(1945, 5, 8))
    Console.WriteLine("The happiest day of the 20th century.");

// Waiting for Enter
Console.ReadLine();
}
```
```

# Discussion

Note the following:

+   - You have omitted braces surrounding individual if branches. You can do that because there is only a single statement in every branch. I normally do not do this, but in this case of many simple ifs, it seemed to me that it would make the code neater.
+   
+   - Individual days of the week are members of the DayOfWeek enumeration. Visual Studio offers you the enumeration as soon as you hit the spacebar on your keyboard after entering two equal signs (see Figure 16-9). Use what Visual Studio offers!