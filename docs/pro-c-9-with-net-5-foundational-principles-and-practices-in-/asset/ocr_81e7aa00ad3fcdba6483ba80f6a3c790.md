# Table 24-1. Pre-WPF Solutions to Desired Functionalities

| Desired Functionality | Technology |
|---|---|
| Building windows with controls | Windows Forms |
| 2D graphics support | GDI+ (System.Drawing.dll) |
| 3D graphics support | DirectX APIs |
| Support for streaming video | Windows Media Player APIs |
| Support for flow-style documents | Programmatic manipulation of PDF files |

As you can see, a Windows Forms developer must pull in types from several unrelated APIs and object models. While it is true that making use of these diverse APIs might look similar syntactically (it is just C# code, after all), you might also agree that each technology requires a radically different mindset. For example, the skills required to create a 3D rendered animation using DirectX are completely different from those used to bind data to a grid. To be sure, it is difficult for a Windows Forms programmer to master the diverse nature of each API.

# Unifying Diverse APIs

WPF was purposely created to merge these previously unrelated programming tasks into a single unified object model. Thus, if you need to author a 3D animation, you have no need to manually program against the DirectX API (although you could) because 3D functionality is baked directly into WPF. To see how well things have cleaned up, consider Table 24-2, which illustrates the desktop development model ushered in as of .NET 3.0.

# Table 24-2. .NET 3.0+ Solutions to Desired Functionalities

| Desired Functionality | Technology |
|---|---|
| Building forms with controls | WPF |
| 2D graphics support | WPF |
| 3D graphics support | WPF |
| Support for streaming video | WPF |
| Support for flow-style documents | WPF |

The obvious benefit here is that .NET programmers now have a single, symmetrical API for all common GUI desktop programming needs. After you become comfortable with the functionality of the key WPF assemblies and the grammar of XAML, you will be amazed how quickly you can create sophisticated UIs.

# Providing a Separation of Concerns via XAML

Perhaps one of the most compelling benefits is that WPF provides a way to cleanly separate the look and feel of a GUI application from the programming logic that drives it. Using XAML, it is possible to define the UI of an application via XML markup. This markup (ideally generated using tools such as Microsoft Visual Studio or Blend for Visual Studio) can then be connected to a related C# code file to provide the guts of the program’s functionality.