![](bbox=[0.175, 0.065, 0.935, 0.36])

Figure 7.4: Decompiled GetParent method of Directory class on Windows

+   13. Note the good practice of checking the path parameter and throwing an ArgumentNullException if it is null or an ArgumentException if it is zero length.
+   14. Close ILSpy.

# Decompiling using the ILSpy extension for Visual Studio Code

A similar capability is available cross-platform as an extension for Visual Studio Code.

+   1. If you have not already installed the ILSpy .NET Decompiler extension for Visual Studio Code, then search for it and install it now.
+   2. On macOS or Linux the extension has a dependency on Mono so you will also need to install Mono from the following link: https://www.mono-project.com/download/stable/.
+   3. In Visual Studio Code, navigate to View | Command Palette....
+   4. Type ilspy and then select ILSpy: Decompile IL Assembly (pick file).
+   5. Navigate to the following folder:

```
Code/Chapter07/DotNetEverywhere/bin/Release/net6.0/linux-x64
```