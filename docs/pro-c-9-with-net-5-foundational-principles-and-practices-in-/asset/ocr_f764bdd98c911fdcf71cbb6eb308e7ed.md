The PowerShell equivalent of the preceding *.cmd file is as follows:

```
dotnet run
if ($LastExitCode -eq 0) {
    Write-Host "This application has succeeded!"
} else
{
    Write-Host "This application has failed!"
}
Write-Host "All Done."
```

To run this, type PowerShell into the VSC terminal and then execute the script by typing this:

```
.\SimpleCSharpApp.ps1
```

You will see the following in the terminal window:

```
***** My First C# App *****

Hello World!

This application has failed!
All Done.
```

A vast majority (if not all) of your C# applications will use void as the return value from Main(), which, as you recall, implicitly returns the error code of zero. To this end, the Main() methods used in this text (beyond the current example) will return void.

# Processing Command-Line Arguments (Updated 9.0)

Now that you better understand the return value of the Main() method or top-level statements, let’s examine the incoming array of string data. Assume that you now want to update your application to process any possible command-line parameters. One way to do this is using a C# for loop. (Note that C#’s iteration constructs will be examined in some detail near the end of this chapter.)

```
// Display a message and wait for Enter key to be pressed.
Console.WriteLine("***** My First C# App *****");
Console.WriteLine("Hello World!");
Console.WriteLine();
// Process any incoming args.
for (int i = 0; i < args.Length; i++)
{
    Console.WriteLine("Arg: {0}", args[i]);
}
Console.ReadLine();
// Return an arbitrary error code.
return 0;
```