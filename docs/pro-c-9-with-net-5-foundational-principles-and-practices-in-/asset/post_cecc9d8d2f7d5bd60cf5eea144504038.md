
- A built-in style engine, which allows you to define “themes” for a WPF application.
- Use of vector graphics, which allows content to be automatically resized to fit the size and resolution of the screen hosting the application.
- Support for 2D and 3D graphics, animations, and video and audio playback.
- A rich typography API, such as support for XML Paper Specification (XPS) documents, fixed documents (WYSIWYG), flow documents, and document annotations (e.g., a Sticky Notes API).
- Support for interoperating with legacy GUI models (e.g., Windows Forms, ActiveX, and Win32 HWNDs). For example, you can incorporate custom Windows Forms controls into a WPF application and vice versa.

Now that you have some idea of what WPF brings to the table, let’s look at the various types of applications that can be created using this API. Many of these features will be explored in detail in the chapters to come.

## Investigating the WPF Assemblies

WPF is ultimately little more than a collection of types bundled within .NET Core assemblies. Table 24-3 describes the key assemblies used to build WPF applications, each of which must be referenced when creating a new project. As you would hope, Visual Studio WPF projects reference these required assemblies automatically.

**Table 24-3. Core WPF Assemblies**

| Assembly | Meaning in Life |
|---|---|
| PresentationCore | This assembly defines numerous namespaces that constitute the foundation of the WPF GUI layer. For example, this assembly contains support for the WPF Ink API, animation primitives, and numerous graphical rendering types. |
| PresentationFramework | This assembly contains a majority of the WPF controls, the Application and Window classes, support for interactive 2D graphics, and numerous types used in data binding. |
| System.Xaml.dll | This assembly provides namespaces that allow you to program against a XAML document at runtime. By and large, this library is useful only if you are authoring WPF support tools or need absolute control over XAML at runtime. |
| WindowsBase.dll | This assembly defines types that constitute the infrastructure of the WPF API, including those representing WPF threading types, security types, various type converters, and support for dependency properties and routed events (described in Chapter 27). |

Collectively, these four assemblies define new namespaces and .NET Core classes, interfaces, structures, enumerations, and delegates. Table 24-4 describes the role of some (but certainly not all) of the important namespaces.

**Table 24-4. Core WPF Namespaces**

| Namespace | Meaning in Life |
|---|---|
| System.Windows | This is the root namespace of WPF. Here, you will find core classes (such as Application and Window) that are required by any WPF desktop project. |
| System.Windows.Controls | This contains all the expected WPF widgets, including types to build menu systems, tooltips, and numerous layout managers. |
| System.Windows.Data | This contains types to work with the WPF data-binding engine, as well as support for data-binding templates. |
| System.Windows.Documents | This contains types to work with the documents API, which allows you to integrate PDF-style functionality into your WPF applications, via the XML Paper Specification (XPS) protocol. |
| System.Windows.Ink | This provides support for the Ink API, which allows you to capture input from a stylus or mouse, respond to input gestures, and so forth. This is useful for Tablet PC programming; however, any WPF can make use of this API. |
| System.Windows.Markup | This namespace defines several types that allow XAML markup (and the equivalent binary format, BAML) to be parsed and processed programmatically. |
| System.Windows.Media | This is the root namespace to several media-centric namespaces. Within these namespaces you will find types to work with animations, 3D rendering, text rendering, and other multimedia primitives. |
| System.Windows.Navigation | This namespace provides types to account for the navigation logic employed by XAML browser applications (XBAPs) as well as standard desktop applications that require a navigational page model. |
| System.Windows.Shapes | This defines classes that allow you to render interactive 2D graphics that automatically respond to mouse input. |

To begin your journey into the WPF programming model, you will examine two members of the System.Windows namespace that are commonplace to any traditional desktop development effort: Application and Window.

> Note If you have created desktop UIs using the Windows Forms API, be aware that the System.Windows.Forms.* and System.Drawing.* assemblies are not related to WPF. These libraries represent the original .NET GUI toolkit, Windows Forms/GDI+.

## The Role of the Application Class

The System.Windows.Application class represents a global instance of a running WPF application. This class supplies a Run() method (to start the application), a series of events that you can handle in order to interact with the application’s lifetime (such as Startup and Exit). Table 24-5 details some of the key properties.

**Table 24-5. Key Properties of the Application Type**

| Property | Meaning in Life |
|---|---|
| Current | This static property allows you to gain access to the running Application object from anywhere in your code. This can be helpful when a window or dialog box needs to gain access to the Application object that created it, typically to access application-wide variables and functionality. |
| MainWindow | This property allows you to programmatically get or set the main window of the application. |
| Properties | This property allows you to establish and obtain data that is accessible throughout all aspects of a WPF application (windows, dialog boxes, etc.). |
| StartupUri | This property gets or sets a URI that specifies a window or page to open automatically when the application starts. |
| Windows | This property returns a WindowCollection type, which provides access to each window created from the thread that created the Application object. This can be helpful when you want to iterate over each open window of an application and alter its state (such as minimizing all windows). |

## Constructing an Application Class

Any WPF application will need to define a class that extends Application. Within this class, you will define your program’s entry point (the Main() method), which creates an instance of this subclass and typically handles the Startup and Exit events (as necessary). Here is an example:

```
// Define the global application object
// for this WPF program.
class MyApp : Application
{
    [STAThread]
    static void Main(string[] args)
    {
        // Create the application object.
        MyApp app = new MyApp();

        // Register the Startup/Exit events.
        app.Startup += (s, e) => { /* Start up the app */ };
        app.Exit += (s, e) => { /* Exit the app */ };
    }
}
```

Within the Startup handler, you will most often process any incoming command-line arguments and launch the main window of the program. The Exit handler, as you would expect, is where you can author any necessary shutdown logic for the program (e.g., save user preferences, write to the Windows registry).

> Note The Main() method of a WPF application must be attributed with the [STAThread] attribute, which ensures any legacy COM objects used by your application are thread-safe. If you do not annotate Main() in this way, you will encounter a runtime exception. Even with the introduction of top-level statements in C# 9.0, you will still want to use the more traditional Main() method in your WPF applications. In fact, the Main() method is autogenerated for you.

## Enumerating the Windows Collection

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

## The Role of the Window Class

The System.Windows.Window class (located in the PresentationFramework.dll assembly) represents a single window owned by the Application-derived class, including any dialog boxes displayed by the main window. Not surprisingly, Window has a series of parent classes, each of which brings more functionality to the table. Consider Figure 24-1, which shows the inheritance chain (and implemented interfaces) for System.Windows.Window as seen through the Visual Studio Object Browser.

![](img/587021f4d5ba039be70a11107130377b_1007_0.png)

Figure 24-1. The hierarchy of the Window class

You will come to understand the functionality provided by many of these base classes as you progress through this chapter and the chapters to come. However, to whet your appetite, the following sections present a breakdown of the functionality provided by each base class (consult the .NET 5 documentation for full details).
