The Environment type has many other useful members that we did not use in this code, including the GetEnvironmentVariables method and the OSVersion and ProcessorCount properties.

5. In Program.cs, above the function, call the OutputFileSystemInfo method, as shown in the following code:

```
OutputFileSystemInfo();
```

6. Run the code and view the result, as shown in Figure 9.1:

![](bbox=[0.18, 0.28, 0.93, 0.45])

Figure 9.1: Running your application to show filesystem information on Windows

When running the console app using dotnet run with Visual Studio Code, the CurrentDirectory will be the project folder, not a folder inside bin.

Good Practice: Windows uses a backslash \ for the directory separator character. macOS and Linux use a forward slash / for the directory separator character. Do not assume what character is used in your code when combining paths.

# Managing drives

To manage drives, use the DriveInfo type, which has a static method that returns information about all the drives connected to your computer. Each drive has a drive type.

Let's explore drives:

1. Create a WorkWithDrives method, and write statements to get all the drives and output their name, type, size, available free space, and format, but only if the drive is ready, as shown in the following code:

```
static void WorkWithDrives()
{
```