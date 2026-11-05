# Building mobile and desktop apps

There are two major mobile platforms: Apple's iOS and Google's Android, each with its own programming languages and platform APIs. There are also two major desktop platforms: Apple's macOS and Microsoft's Windows, each with its own programming languages and platform APIs, as shown in the following list:

+   - **iOS**: Objective C or Swift and UIKit.
+   - **Android**: Java or Kotlin and the Android API.
+   - **macOS**: Objective C or Swift and AppKit or Catalyst.
+   - **Windows**: C, C++, or many other languages and the Win32 API or Windows App SDK.

Since this book is about modern cross-platform development using C# and .NET it does not include coverage of building desktop apps using Windows Forms, Windows Presentation Foundation (WPF), or Universal Windows Platform (UWP) apps because they are Windows-only.

Cross-platform mobile and desktop apps can be built once for the .NET Multi-platform App User Interfaces (MAUI) platform, and then can run on many mobile and desktop platforms.

.NET MAUI makes it easy to develop those apps by sharing user interface components as well as business logic. They can target the same .NET APIs as used by console apps, websites, and web services. The app will be executed by the Mono runtime on mobile devices and the CoreCLR runtime on desktop devices. The Mono runtime is better optimized for mobile devices compared to the normal .NET CoreCLR runtime. Blazor WebAssembly also uses the Mono runtime because like a mobile app, it is resource constrained.

The apps can exist on their own, but they usually call services to provide an experience that spans across all your computing devices, from servers and laptops to phones and gaming systems.

Future updates to .NET MAUI will support existing MVVM and XAML patterns as well as ones like Model-View-Update (MVU) with C#, which is like Apple's Swift UI.

The penultimate chapter in this sixth edition is Chapter 19, Building Mobile and Desktop Apps Using .NET MAUI, and covers using .NET MAUI to build cross-platform mobile and desktop apps.

# Alternatives to .NET MAUI

Before Microsoft created .NET MAUI, third parties created open-source initiatives to enable .NET developers to build cross-platform apps using XAML named Uno and Avalonia.

# Understanding Uno Platform

As Uno state on their website, it is "the first and only UI Platform for single-codebase applications for Windows, WebAssembly, iOS, macOS, Android, and Linux."