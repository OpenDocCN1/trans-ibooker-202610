```
Type.Missing, Type.Missing, Type.Missing);
// Must specify all missing optional args!
workSheet.SaveAs(
    $@"{Environment.CurrentDirectory}\InventoryManual.xlsx",
    Type.Missing, Type.Missing, Type.Missing,
    Type.Missing, Type.Missing, Type.Missing,
    Type.Missing, Type.Missing, Type.Missing);
excelApp.Quit();
Console.WriteLine("The InventoryManual.xlsx file has been saved to your app folder");
}
```

That wraps up your look at the C# dynamic keyword and the DLR. I hope you can see how these features can simplify complex programming tasks and (perhaps more importantly) understand the trade-offs. When you opt into dynamic data, you do lose a good amount of type safety, and your code base is prone to many more runtime errors.

While there is certainly more to say about the DLR, this chapter has tried to focus on topics that are practical and useful in your day-to-day programming. If you want to learn more about advanced features of the Dynamic Language Runtime, such as integrating with scripting languages, be sure to consult the .NET Core SDK documentation (look up the topic “Dynamic Language Runtime Overview” to get started).

# Summary

The dynamic keyword allows you to define data whose identity is not known until runtime. When processed by the Dynamic Language Runtime, the automatically created “expression tree” will be passed to the correct dynamic language binder, where the payload will be unpackaged and sent to the correct object member.

Using dynamic data and the DLR, complex C# programming tasks can be radically simplified, especially the act of incorporating COM libraries into your .NET Core applications. As you saw in this chapter, this provides a number of further simplifications to COM interop (which have nothing to do with dynamic data), such as embedding COM interop data into your applications, optional arguments, and named arguments.

While these features can certainly simplify your code, always remember that dynamic data makes your C# code much less type-safe and open to runtime errors. Be sure you weigh the pros and cons of using dynamic data in your C# projects, and test accordingly!