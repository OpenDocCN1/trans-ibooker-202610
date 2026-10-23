If you click that badge, a menu that displays the type of container you will get when you run the docker build command is rendered. If you see Switch To Windows Containers, it indicates that you are in Linux mode. If you see “Switch To Windows Containers,” you are in Windows mode. The sample program you will use in the next exercise is an ASP.NET Core Web API that can run on both Windows and Linux. We will use Linux for the next exercise, so make sure you have it set correctly. In Exercise 4.2, you will create a local Git repository, download an ASP.NET Core Web API hosted on GitHub, create a Docker image, and run it in a Linux Docker container.

# EXERCISE 4.2

# Building a Docker Image and Running the Container

+   1. Create a local Git repository by executing the following command in PowerShell (Run As Administrator). The result should resemble something similar to Figure 4.14.

```
git init csharpguitar-aci
cd csharpguitar-aci
```

# FIGURE 4.14 Creating a local git repository

![](bbox=[0.29, 0.45, 0.73, 0.53])

+   2. Download a small ASP.NET Core 2.2 Web API located on GitHub using the following command. Type the command all on one line.

```
git svn clone https://github.com/benperk/ASA.git/trunk/Chapter04/Ch04Ex02/csharpguitar-aci -r HEAD
```

+   3. Download the Runtime & Hosting Bundle and install the .NET Core 2.2 runtime from https://dotnet.microsoft.com/download/dotnet-core/2.2. Accept the defaults and execute the following command. The output should be similar to that shown in Figure 4.15.

```
dotnet --version
```

# FIGURE 4.15 How to check the version of .NET Core

![](bbox=[0.33, 0.79, 0.68, 0.83])