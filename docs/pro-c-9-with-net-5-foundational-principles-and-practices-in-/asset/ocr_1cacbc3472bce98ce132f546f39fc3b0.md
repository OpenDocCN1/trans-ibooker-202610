# Table 3-2. Select Members of System.Console

| Member | Meaning in Life |
|---|---|
| Beep() | This method forces the console to emit a beep of a specified frequency and duration. |
| BackgroundColor | These properties set the background/foreground colors for the current output. |
| ForegroundColor | They may be assigned any member of the ConsoleColor enumeration. |
| BufferHeightBufferWidth | These properties control the height/width of the console’s buffer area. |
| Title | This property gets or sets the title of the current console. |
| WindowHeightWindowWidthWindowTopWindowLeft | These properties control the dimensions of the console in relation to the established buffer. |
| Clear() | This method clears the established buffer and console display area. |

# Performing Basic Input and Output (I/O) with the Console Class

In addition to the members in Table 3-2, the Console type defines a set of methods to capture input and output, all of which are static and are, therefore, called by prefixing the name of the class (Console) to the method name. As you have seen, WriteLine() pumps a text string (including a carriage return) to the output stream. The Write() method pumps text to the output stream without a carriage return. ReadLine() allows you to receive information from the input stream up until the Enter key is pressed, while Read() is used to capture a single character from the input stream.

To illustrate simple I/O using the Console class, create a new Console Application project named BasicConsoleIO and add it to your solution with these CLI commands:

```
dotnet new console -lang c# -n BasicConsoleIO -o .\BasicConsoleIO -f net5.0
dotnet sln .\Chapter3_AllProjects.sln add .\BasicConsoleIO
```

```
Replace the Program.cs code with the following:
using System;
Console.WriteLine("***** Basic Console I/O *****");
GetUserData();
Console.ReadLine();
static void GetUserData()
{
}
```

> Note Visual Studio and Visual Studio Code both support a number of “code snippets” that will insert code once activated. The cw code snippet is quite useful during the early chapters of this text, in that it will automatically expand to Console.WriteLine()! To test this for yourself, type in cw somewhere within your code and hit the Tab key. Note: In Visual Studio Code, you hit the Tab key once; in Visual Studio, you must hit the Tab key twice.