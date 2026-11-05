
Microsoft purchased Xamarin in 2016 and now gives away what used to be an expensive Xamarin extension for free with Visual Studio. Microsoft renamed the Xamarin Studio development tool, which could only create mobile apps, to Visual Studio for Mac and gave it the ability to create other types of projects like console apps and web services. With Visual Studio 2022 for Mac, Microsoft has replaced parts of the Xamarin Studio editor with parts from Visual Studio 2022 for Windows to provide closer parity of experience and performance. Visual Studio 2022 for Mac was also rewritten to be a truly native macOS UI app to improve reliability and work with macOS's built-in assistive technologies.

## Understanding .NET Core

Today, we live in a truly cross-platform world where modern mobile and cloud development have made Windows, as an operating system, much less important. Because of that, Microsoft has been working on an effort to decouple .NET from its close ties with Windows. While rewriting .NET Framework to be truly cross-platform, they've taken the opportunity to refactor and remove major parts that are no longer considered core.

This new product was branded .NET Core and includes a cross-platform implementation of the CLR known as CoreCLR and a streamlined BCL known as CoreFX.

Scott Hunter, Microsoft Partner Director Program Manager for .NET, has said that "Forty percent of our .NET Core customers are brand-new developers to the platform, which is what we want with .NET Core. We want to bring new people in."

.NET Core is fast-moving, and because it can be deployed side by side with an app, it can change frequently, knowing those changes will not affect other .NET Core apps on the same machine. Most improvements that Microsoft makes to .NET Core and modern .NET cannot be easily added to .NET Framework.

## Understanding the journey to one .NET

At the Microsoft Build developer conference in May 2020, the .NET team announced that their plans for the unification of .NET had been delayed. They said that .NET 5 would be released on November 10, 2020, and it would unify all the various .NET platforms except mobile. It would not be until .NET 6 in November 2021 that mobile will also be supported by the unified .NET platform.

.NET Core has been renamed .NET and the major version number has skipped the number four to avoid confusion with .NET Framework 4.x. Microsoft plans on annual major version releases every November, rather like Apple does major version number releases of iOS every September.

The following table shows when the key versions of modern .NET were released, when future releases are planned, and which version is used by the various editions of this book:

| Version | Released | Edition | Published |
|---|---|---|---|
| .NET Core RC1 | November 2015 | First | March 2016 |
| .NET Core 1.0 | June 2016 |  |  |
| .NET Core 1.1 | November 2016 |  |  |
| .NET Core 1.0.4 and .NET Core 1.1.1 | March 2017 | Second | March 2017 |
| .NET Core 2.0 | August 2017 |  |  |
| .NET Core for UWP in Windows 10 Fall Creators Update | October 2017 | Third | November 2017 |
| .NET Core 2.1 (LTS) | May 2018 |  |  |
| .NET Core 2.2 (Current) | December 2018 |  |  |
| .NET Core 3.0 (Current) | September 2019 | Fourth | October 2019 |
| .NET Core 3.1 (LTS) | December 2019 |  |  |
| Blazor WebAssembly 3.2 (Current) | May 2020 |  |  |
| .NET 5.0 (Current) | November 2020 | Fifth | November 2020 |
| .NET 6.0 (LTS) | November 2021 | Sixth | November 2021 |
| .NET 7.0 (Current) | November 2022 | Seventh | November 2022 |
| .NET 8.0 (LTS) | November 2023 | Eighth | November 2023 |

.NET Core 3.1 included Blazor Server for building web components. Microsoft had also planned to include Blazor WebAssembly in that release, but it was delayed. Blazor WebAssembly was later released as an optional add-on for .NET Core 3.1. I include it in the table above because it was versioned as 3.2 to exclude it from the LTS of .NET Core 3.1.

## Understanding .NET support

.NET versions are either Long Term Support (LTS) or Current, as described in the following list:

- **LTS** releases are stable and require fewer updates over their lifetime. These are a good choice for applications that you do not intend to update frequently. LTS releases will be supported for 3 years after general availability, or 1 year after the next LTS release ships, whichever is longer.
- **Current** releases include features that may change based on feedback. These are a good choice for applications that you are actively developing because they provide access to the latest improvements. After a 6-month maintenance period, or 18 months after general availability, the previous minor version will no longer be supported.

Both receive critical fixes throughout their lifetime for security and reliability. You must stay up to date with the latest patches to get support. For example, if a system is running 1.0 and 1.0.1 has been released, 1.0.1 will need to be installed to get support.

To better understand your choices of Current and LTS releases, it is helpful to see it visually, with 3-year-long black bars for LTS releases, and variable-length gray bars for Current releases that end with cross-hatching for the 6 months after a new major or minor release that they retain support for, as shown in Figure 1.2:

![](img/c63b00dca7192cc58cfb9e1f1e64f483_43_0.png)

Figure 1.2: Support for various versions

For example, if you had created a project using .NET Core 3.0, then when Microsoft released .NET Core 3.1 in December 2019, you had to upgrade your project to .NET Core 3.1 by March 2020. (Before .NET 5, the maintenance period for Current releases was only three months.)

If you need long-term support from Microsoft, then choose .NET 6.0 today and stick with it until .NET 8.0, even once Microsoft releases .NET 7.0. This is because .NET 7.0 will be a current release and it will therefore lose support before .NET 6.0 does. Just remember that even with LTS releases you must upgrade to bug fix releases like 6.0.1.

All versions of .NET Core and modern .NET have reached their end of life except those shown in the following list:

- .NET 5.0 will reach end of life in May 2022.
- .NET Core 3.1 will reach end of life on December 3, 2022.
- .NET 6.0 will reach end of life in November 2024.

## Understanding .NET Runtime and .NET SDK versions

.NET Runtime versioning follows semantic versioning, that is, a major increment indicates breaking changes, minor increments indicate new features, and patch increments indicate bug fixes.

.NET SDK versioning does not follow semantic versioning. The major and minor version numbers are tied to the runtime version it is matched with. The patch number follows a convention that indicates the major and minor versions of the SDK.

You can see an example of this in the following table:

| Change | Runtime | SDK |
|---|---|---|
| Initial release | 6.0.0 | 6.0.100 |
| SDK bug fix | 6.0.0 | 6.0.101 |
| Runtime and SDK bug fix | 6.0.1 | 6.0.102 |
| SDK new feature | 6.0.1 | 6.0.200 |

## Removing old versions of .NET

.NET Runtime updates are compatible with a major version such as 6.x, and updated releases of the .NET SDK maintain the ability to build applications that target previous versions of the runtime, which enables the safe removal of older versions.

You can see which SDKs and runtimes are currently installed using the following commands:

- `dotnet --list-sdks`
- `dotnet --list-runtimes`

On Windows, use the Apps & features section to remove .NET SDKs. On macOS or Windows, use the dotnet-core-uninstall tool. This tool is not installed by default.

For example, while writing the fourth edition, I used the following command every month:

```
dotnet-core-uninstall remove --all-previews-but-latest --sdk
```

## What is different about modern .NET?

Modern .NET is modularized compared to the legacy .NET Framework, which is monolithic. It is open source and Microsoft makes decisions about improvements and changes in the open. Microsoft has put particular effort into improving the performance of modern .NET.

It is smaller than the last version of .NET Framework due to the removal of legacy and non-cross-platform technologies. For example, workloads such as Windows Forms and Windows Presentation Foundation (WPF) can be used to build graphical user interface (GUI) applications, but they are tightly bound to the Windows ecosystem, so they are not included with .NET on macOS and Linux.

## Windows development

One of the features of modern .NET is support for running old Windows Forms and WPF applications using the Windows Desktop Pack that is included with the Windows version of .NET Core 3.1 or later, which is why it is bigger than the SDKs for macOS and Linux. You can make some small changes to your legacy Windows app if necessary, and then rebuild it for .NET 6 to take advantage of new features and performance improvements.
