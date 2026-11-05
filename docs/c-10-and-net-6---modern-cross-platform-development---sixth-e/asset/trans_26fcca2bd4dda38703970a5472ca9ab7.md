
| SDK | 新类库的默认目标框架 |
|---|---|
| .NET Core 3.1 | netstandard2.0 |
| .NET 5 | net5.0 |
| .NET 6 | net6.0 |

当然，仅因为类库在默认情况下针对特定版本的 .NET 并不意味着在创建类库项目使用默认模板后就无法更改它。

你可以手动将目标框架设置为支持需要引用该库的项目的值，如下表所示：

| 类库目标框架 | 可被以下目标的项目使用 |
|---|---|
| netstandard2.0 | .NET Framework 4.6.1 或更高版本, .NET Core 2.0 或更高版本, .NET 5.0 或更高版本, Mono 5.4 或更高版本, Xamarin.Android 8.0 或更高版本, Xamarin.iOS 10.14 或更高版本 |
| netstandard2.1 | .NET Core 3.0 或更高版本, .NET 5.0 或更高版本, Mono 6.4 或更高版本, Xamarin.Android 10.0 或更高版本, Xamarin.iOS 12.16 或更高版本 |
| net5.0 | .NET 5.0 或更高版本 |
| net6.0 | .NET 6.0 或更高版本 |

![](img/c63b00dca7192cc58cfb9e1f1e64f483_315_0.png)

> **良好实践：** 始终检查类库的目标框架，然后在必要时手动将其更改为更合适的框架。应有意识地决定它应该是什么，而不是接受默认值。

## 创建 .NET Standard 2.0 类库

我们将使用 .NET Standard 2.0 创建一个类库，以便它可以在所有重要的 .NET 旧版平台上使用，并在 Windows、macOS 和 Linux 操作系统上跨平台运行，同时还能访问广泛的 .NET API：

-   1. 使用您喜欢的代码编辑器向 Chapter07 解决方案/工作区添加一个名为 SharedLibrary 的新类库。
-   2. 如果您使用 Visual Studio 2022，在提示选择目标框架时，选择 .NET Standard 2.0，然后将解决方案的启动项目设置为当前选择。
-   3. 如果您使用 Visual Studio Code，请包含一个开关以将目标设为 .NET Standard 2.0，如以下命令所示：
-   4. 如果您使用 Visual Studio Code，请将 SharedLibrary 选择为活动的 OmniSharp 项目。

```
dotnet new classlib -f netstandard2.0
```

> **良好实践：** 如果您需要创建使用 .NET 6.0 中新功能的类型，以及仅使用 .NET Standard 2.0 功能的类型，那么您可以创建两个单独的类库：一个以 .NET Standard 2.0 为目标，另一个以 .NET 6.0 为目标。您将在 *第 10 章，使用 Entity Framework Core 操作数据* 中看到实际应用。

手动创建两个类库的替代方案是创建一个支持多目标的类库。如果您希望我在下一版中添加有关多目标的章节，请告诉我。您可以在此处阅读有关多目标的信息： https://docs.microsoft.com/en-us/dotnet/standard/library-guidance/cross-platform-targeting#multi-targeting.

## 控制 .NET SDK

默认情况下，执行 dotnet 命令会使用最新安装的 .NET SDK。可能有时候您想要控制使用哪个 SDK。

例如，第四版的一位读者希望他们的体验与书中使用 .NET Core 3.1 SDK 的步骤相匹配。但他们也安装了 .NET 5.0 SDK，并且默认使用的是该 SDK。如前一节所述，创建新类库时的行为已更改为以 .NET 5.0 为目标，而不是 .NET Standard 2.0，这让读者感到困惑。

您可以通过使用 global.json 文件来控制默认使用的 .NET SDK。dotnet 命令会在当前文件夹和祖先文件夹中搜索 global.json 文件。

-   1. 在 Chapter07 文件夹中创建一个名为 ControlSDK 的子目录/文件夹。
-   2. 在 Windows 上，启动 **命令提示符** 或 **Windows 终端**。在 macOS 上，启动 **终端**。如果您使用的是 Visual Studio Code，则可以使用集成终端。
-   3. 在 ControlSDK 文件夹中，在命令提示符或终端中，输入命令创建 global.json 文件以强制使用最新的 .NET Core 3.1 SDK，如以下命令所示：
-   4. 打开 global.json 文件并查看其内容，如以下标记所示：

```
dotnet new globaljson --sdk-version 3.1.412
```

```
{
  "sdk": {
    "version": "3.1.412"
  }
}
```

> 您可以在以下链接的表格中查找最新 .NET SDK 的版本号： https://dotnet.microsoft.com/download/visual-studio-sdks

5. 在 ControlSDK 文件夹中，在命令提示符或终端中，输入命令创建类库项目，如以下命令所示：

```
dotnet new classlib
```

6. 如果您未安装 .NET Core 3.1 SDK，则会看到错误，如以下输出所示：

```
Could not execute because the application was not found or a compatible .NET SDK is not installed.
```

7. 如果您已安装 .NET Core 3.1 SDK，则将创建一个默认以 .NET Standard 2.0 为目标的类库项目。

您无需完成上述步骤，但如果您想尝试并且尚未安装 .NET Core 3.1 SDK，则可以从以下链接安装：

https://dotnet.microsoft.com/download/dotnet/3.1

## 发布您的代码以进行部署

如果您写了一本小说并希望其他人阅读它，您必须发布它。

大多数开发人员编写代码供其他开发人员在其自己的代码中使用，或供用户作为应用程序运行。为此，您必须将代码作为打包的类库或可执行应用程序发布。

发布和部署 .NET 应用程序有三种方式，它们是：

-   1. 基于框架的部署 (FDD)。
-   2. 基于框架的可执行文件 (FDEs)。
-   3. 自包含。

如果您选择部署应用程序及其包依赖项，但不部署 .NET 本身，那么您依赖于目标计算机上已存在 .NET。这对于部署到服务器的 Web 应用程序效果很好，因为服务器上可能已经存在 .NET 和许多其他 Web 应用程序。

基于框架的部署 (FDD) 意味着您部署一个必须由 dotnet 命令行工具执行的 DLL。基于框架的可执行文件 (FDE) 意味着您部署一个可以直接从命令行运行的 EXE。两者都要求系统上已安装 .NET。

有时，您希望能够给某人一个包含应用程序的 USB 驱动器，并知道它可以在他们的计算机上执行。您希望执行自包含部署。虽然部署文件的大小会更大，但您会知道它会正常工作。

## 创建要发布的控制台应用程序

让我们探索如何发布控制台应用程序：

-   1. 使用您喜欢的代码编辑器向 Chapter07 解决方案/工作区添加一个名为 DotNetEverywhere 的新控制台应用。
-   2. 在 Visual Studio Code 中，将 DotNetEverywhere 选择为活动的 OmniSharp 项目。当您看到弹出警告消息说缺少必需的资源时，单击“是”以添加它们。
-   3. 在 Program.cs 中，删除注释并静态导入 Console 类。
-   4. 在 Program.cs 中，添加语句输出一条消息，说明控制台应用可以在任何地方运行以及有关操作系统的一些信息，如以下代码所示：
-   5. 打开 DotNetEverywhere.csproj 并在 <PropertyGroup> 元素内添加运行时标识符以针对三个操作系统，如以下标记中突出显示所示：

```csharp
WriteLine("I can run everywhere!");

WriteLine($"OS Version is {Environment.OSVersion}.");

if (OperatingSystem.IsMacOS())
{
    WriteLine("I am macOS.");
}
else if (OperatingSystem.IsWindowsVersionAtLeast(major: 10))
{
    WriteLine("I am Windows 10 or 11.");
}
else
{
    WriteLine("I am some other mysterious OS.");
}

WriteLine("Press ENTER to stop me.");
ReadLine();
```

```
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <OutputType>Exe</OutputType>
    <TargetFramework>net6.0</TargetFramework>
    <Nullable>enable</Nullable>
    <ImplicitUsings>enable</ImplicitUsings>
    <RuntimeIdentifiers>
      win10-x64;osx-x64;osx.11.0-arm64;linux-x64;linux-arm64
    </RuntimeIdentifiers>
  </PropertyGroup>

</Project>
```

-   - win10-x64 RID 值表示 Windows 10 或 Windows Server 2016 64 位。您也可以使用 win10-arm64 RID 值部署到 Microsoft Surface Pro X。
-   - osx-x64 RID 值表示 macOS Sierra 10.12 或更高版本。您还可以指定特定版本的 RID 值，如 osx.10.15-x64（Catalina）、osx.11.0-x64（Intel 上的 Big Sur）或 osx.11.0-arm64（Apple Silicon 上的 Big Sur）。
-   - linux-x64 RID 值表示大多数 Linux 桌面发行版，如 Ubuntu、CentOS、Debian 或 Fedora。使用 linux-arm 用于 Raspbian 或 Raspberry Pi OS 32 位。使用 linux-arm64 用于运行 Ubuntu 64 位的 Raspberry Pi。

## 了解 dotnet 命令

当您安装 .NET SDK 时，它包含一个名为 dotnet 的命令行界面 (CLI)。

## 创建新项目

.NET CLI 具有在当前文件夹上工作的命令，可使用模板创建新项目：

-   1. 在 Windows 上，启动命令提示符或 Windows 终端。在 macOS 上，启动终端。如果您使用的是 Visual Studio Code，则可以使用集成终端。
