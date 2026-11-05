10. Step into or over the four calls to Trace.WriteLineIf and note that all levels up to Info are written to the DEBUG CONSOLE or Output - Debug window, but not Verbose, as shown in Figure 4.20:

![](bbox=[0.17, 0.13, 0.93, 0.36])

Figure 4.20: Different trace levels shown in the DEBUG CONSOLE in Visual Studio Code

11. Stop debugging.

12. Modify appsettings.json to set a level of 2, which means warning, as shown in the following JSON file:

```
{
  "PacktSwitch": {
    "Level": "2"
  }
}
```

13. Save the changes.

14. In Visual Studio Code, run the console application by entering the following command in the TERMINAL window for the Instrumenting project:

```
dotnet run --configuration Release
```

15. In Visual Studio, in the standard toolbar, select Release in the Solution Configurations dropdown list and then run the console app by navigating to Debug | Start Without Debugging.

16. Open the file named log.txt and note that this time, only trace error and warning levels are the output of the four potential trace levels, as shown in the following text file:

```
Trace says, I am watching!
Trace error
Trace warning
```

If no argument is passed, the default trace switch level is Off (0), so none of the switch levels are output.