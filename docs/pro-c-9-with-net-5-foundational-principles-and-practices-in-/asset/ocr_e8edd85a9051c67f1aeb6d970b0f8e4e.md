# CHAPTER 15

# Multithreaded, Parallel, and Async Programming

Nobody enjoys working with an application that is sluggish during its execution. Moreover, nobody enjoys starting a task in an application (perhaps initiated by clicking a toolbar item) that prevents other parts of the program from being as responsive as possible. Before the release of .NET (and .NET Core), building applications that had the ability to perform multiple tasks typically required authoring complex C++ code that used the Windows threading APIs. Thankfully, the .NET/.NET Core platform provides several ways for you to build software that can perform complex operations on unique paths of execution, with far fewer pain points.

This chapter begins by defining the overall nature of a “multithreaded application.” Next, you will be introduced to the original threading namespace that has shipped since .NET 1.0, specifically System.Threading. Here, you will examine numerous types (Thread, ThreadStart, etc.) that allow you to explicitly create additional threads of execution and synchronize your shared resources, which helps ensure that multiple threads can share data in a nonvolatile manner.

The remaining parts of this chapter will examine three more recent techniques .NET Core developers can use to build multithreaded software, specifically the Task Parallel Library (TPL), Parallel LINQ (PLINQ), and the relatively new (as of C# 6) intrinsic asynchronous keywords of C# (async and await). As you will see, these features can dramatically simplify how you can build responsive multithreaded software applications.

# The Process/AppDomain/Context/Thread Relationship

In Chapter 14, a thread was defined as a path of execution within an executable application. While many .NET Core applications can live happy and productive single-threaded lives, an assembly’s primary thread (spawned by the runtime when the application’s entry point executes) may create secondary threads of execution at any time to perform additional units of work. By creating additional threads, you can build more responsive (but not necessarily faster executing on single-core machines) applications.

The System.Threading namespace was released with .NET 1.0 and offers one approach to build multithreaded applications. The Thread class is perhaps the core type, as it represents a given thread. If you want to programmatically obtain a reference to the thread currently executing a given member, simply call the static Thread.CurrentThread property, like so:

```
static void ExtractExecutingThread()
{
    // Get the thread currently
    // executing this method.
    Thread currThread = Thread.CurrentThread;
}
```