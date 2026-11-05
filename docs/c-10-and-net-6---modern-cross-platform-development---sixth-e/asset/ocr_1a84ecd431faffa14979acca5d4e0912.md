+   - **macOS**: https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf
+   - **Linux**: https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf

# Understanding .NET

.NET 6, .NET Core, .NET Framework, and Xamarin are related and overlapping platforms for developers used to build applications and services. In this section, I'm going to introduce you to each of these .NET concepts.

# Understanding .NET Framework

.NET Framework is a development platform that includes a Common Language Runtime (CLR), which manages the execution of code, and a Base Class Library (BCL), which provides a rich library of classes to build applications from.

Microsoft originally designed .NET Framework to have the possibility of being cross-platform, but Microsoft put their implementation effort into making it work best with Windows.

Since .NET Framework 4.5.2, it has been an official component of the Windows operating system. Components have the same support as their parent products, so 4.5.2 and later follow the life cycle policy of the Windows OS on which it is installed. .NET Framework is installed on over one billion computers, so it must change as little as possible. Even bug fixes can cause problems, so it is updated infrequently.

For .NET Framework 4.0 or later, all of the apps on a computer written for .NET Framework share the same version of the CLR and libraries stored in the Global Assembly Cache (GAC), which can lead to issues if some of them need a specific version for compatibility.

> **Good Practice**: Practically speaking, .NET Framework is Windows-only and a legacy platform. Do not create new apps using it.

# Understanding the Mono, Xamarin, and Unity projects

Third parties developed a .NET Framework implementation named the Mono project. Mono is cross-platform, but it fell well behind the official implementation of .NET Framework.

Mono has found a niche as the foundation of the Xamarin mobile platform as well as cross-platform game development platforms like Unity.