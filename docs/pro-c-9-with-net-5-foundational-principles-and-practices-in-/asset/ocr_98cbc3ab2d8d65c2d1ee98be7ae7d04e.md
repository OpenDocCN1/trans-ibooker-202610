+   - A built-in style engine, which allows you to define “themes” for a WPF application.
+   - Use of vector graphics, which allows content to be automatically resized to fit the size and resolution of the screen hosting the application.
+   - Support for 2D and 3D graphics, animations, and video and audio playback.
+   - A rich typography API, such as support for XML Paper Specification (XPS) documents, fixed documents (WYSIWYG), flow documents, and document annotations (e.g., a Sticky Notes API).
+   - Support for interoperating with legacy GUI models (e.g., Windows Forms, ActiveX, and Win32 HWNDs). For example, you can incorporate custom Windows Forms controls into a WPF application and vice versa.

Now that you have some idea of what WPF brings to the table, let’s look at the various types of applications that can be created using this API. Many of these features will be explored in detail in the chapters to come.

# Investigating the WPF Assemblies

WPF is ultimately little more than a collection of types bundled within .NET Core assemblies. Table 24-3 describes the key assemblies used to build WPF applications, each of which must be referenced when creating a new project. As you would hope, Visual Studio WPF projects reference these required assemblies automatically.

**Table 24-3. Core WPF Assemblies**

| Assembly | Meaning in Life |
|---|---|
| PresentationCore | This assembly defines numerous namespaces that constitute the foundation of the WPF GUI layer. For example, this assembly contains support for the WPF Ink API, animation primitives, and numerous graphical rendering types. |
| PresentationFramework | This assembly contains a majority of the WPF controls, the Application and Window classes, support for interactive 2D graphics, and numerous types used in data binding. |
| System.Xaml.dll | This assembly provides namespaces that allow you to program against a XAML document at runtime. By and large, this library is useful only if you are authoring WPF support tools or need absolute control over XAML at runtime. |
| WindowsBase.dll | This assembly defines types that constitute the infrastructure of the WPF API, including those representing WPF threading types, security types, various type converters, and support for dependency properties and routed events (described in Chapter 27). |

Collectively, these four assemblies define new namespaces and .NET Core classes, interfaces, structures, enumerations, and delegates. Table 24-4 describes the role of some (but certainly not all) of the important namespaces.