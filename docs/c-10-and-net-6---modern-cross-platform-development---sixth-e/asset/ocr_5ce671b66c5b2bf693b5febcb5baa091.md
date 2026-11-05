2. In the Chapter01-vscode folder, use the **New Folder** button to create a new folder named TopLevelProgram, select it, and click **Add**.

3. Navigate to **Terminal** | **New Terminal**, and in the drop-down list that appears, select **TopLevelProgram**. Alternatively, in **EXPLORER**, right-click the TopLevelProgram folder and then select **Open in Integrated Terminal**.

4. In **TERMINAL**, confirm that you are in the TopLevelProgram folder, and then enter the command to create a new console application, as shown in the following command:

```
```bash
dotnet new console
```
```

![](bbox=[0.17, 0.31, 0.25, 0.37])

> **Good Practice:** When using workspaces, be careful when entering commands in **TERMINAL**. Be sure that you are in the correct folder before entering potentially destructive commands! That is why I got you to create a new terminal for TopLevelProgram before issuing the command to create a new console app.

5. Navigate to **View** | **Command Palette**.

6. Enter omni, and then, in the drop-down list that appears, select **OmniSharp: Select Project**.

7. In the drop-down list of two projects, select the **TopLevelProgram** project, and when prompted, click **Yes** to add required assets to debug.

![](bbox=[0.17, 0.61, 0.25, 0.67])

> **Good Practice:** To enable debugging and other useful features, like code formatting and Go to Definition, you must tell OmniSharp which project you are actively working on in Visual Studio Code. You can quickly toggle active projects by clicking the project/folder to the right of the flame icon on the left side of the status bar.

8. In **EXPLORER**, in the TopLevelProgram folder, select Program.cs, and then change the existing statement to output a different message and also output the operating system version string, as shown in the following code:

```
```csharp
Console.WriteLine("Hello from a Top Level Program!");
Console.WriteLine(Environment.OSVersion.VersionString);
```
```

9. In **TERMINAL**, enter the command to run a program, as shown in the following command:

```
```bash
dotnet run
```
```