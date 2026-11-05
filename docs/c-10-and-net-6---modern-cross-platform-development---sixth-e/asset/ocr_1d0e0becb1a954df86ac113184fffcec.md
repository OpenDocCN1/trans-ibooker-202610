5. In the ControlSDK folder, at the command prompt or terminal, enter a command to create a class library project, as shown in the following command:

```
dotnet new classlib
```

6. If you do not have the .NET Core 3.1 SDK installed then you will see an error, as shown in the following output:

```
Could not execute because the application was not found or a compatible .NET SDK is not installed.
```

7. If you do have the .NET Core 3.1 SDK installed, then a class library project will be created that targets .NET Standard 2.0 by default.

You do not need to complete the above steps, but if you want to try and do not already have .NET Core 3.1 SDK installed then you can install it from the following link:

https://dotnet.microsoft.com/download/dotnet/3.1

# Publishing your code for deployment

If you write a novel and you want other people to read it, you must publish it.

Most developers write code for other developers to use in their own code, or for users to run as an app. To do so, you must publish your code as packaged class libraries or executable applications.

There are three ways to publish and deploy a .NET application. They are:

+   1. Framework-dependent deployment (FDD).
+   2. Framework-dependent executables (FDEs).
+   3. Self-contained.

If you choose to deploy your application and its package dependencies, but not .NET itself, then you rely on .NET already being on the target computer. This works well for web applications deployed to a server because .NET and lots of other web applications are likely already on the server.

Framework-dependent deployment (FDD) means you deploy a DLL that must be executed by the dotnet command-line tool. Framework-dependent executables (FDE) means you deploy an EXE that can be run directly from the command line. Both require .NET to be already installed on the system.

Sometimes, you want to be able to give someone a USB stick containing your application and know that it can execute on their computer. You want to perform a self-contained deployment. While the size of the deployment files will be larger, you'll know that it will work.