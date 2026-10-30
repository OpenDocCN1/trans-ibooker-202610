# Enumerating the Windows Collection

Another interesting property exposed by Application is Windows, which provides access to a collection representing each window loaded into memory for the current WPF application. As you create new Window objects, they are automatically added into the Application.Windows collection. Here is an example method that will minimize each window of the application (perhaps in response to a given keyboard gesture or menu option triggered by the end user):

```
static void MinimizeAllWindows()
{
    foreach (Window wnd in Application.Current.Windows)
    {
        wnd.WindowState = WindowState.Minimized;
    }
}
```

You will build some WPF applications shortly, but until then, let’s check out the core functionality of the Window type and learn about a number of important WPF base classes in the process.

# The Role of the Window Class

The System.Windows.Window class (located in the PresentationFramework.dll assembly) represents a single window owned by the Application-derived class, including any dialog boxes displayed by the main window. Not surprisingly, Window has a series of parent classes, each of which brings more functionality to the table. Consider Figure 24-1, which shows the inheritance chain (and implemented interfaces) for System.Windows.Window as seen through the Visual Studio Object Browser.

![](bbox=[0.105, 0.525, 0.925, 0.745])

Figure 24-1. The hierarchy of the Window class

You will come to understand the functionality provided by many of these base classes as you progress through this chapter and the chapters to come. However, to whet your appetite, the following sections present a breakdown of the functionality provided by each base class (consult the .NET 5 documentation for full details).