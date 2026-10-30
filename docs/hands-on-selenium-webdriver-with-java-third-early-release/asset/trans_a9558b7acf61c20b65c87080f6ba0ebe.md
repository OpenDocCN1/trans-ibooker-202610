

你可以在任何主流操作系统中使用 Selenium WebDriver：Windows、Linux 或 macOS。因此，你可以选择自己偏好的计算机类型。原则上，对于内存、CPU、硬盘等硬件没有特定要求，任何中端电脑即可。

## Java 虚拟机

接下来，你需要在电脑上安装 Java 虚拟机（JVM）。JVM 有两种发行类型。第一个选项是 Java 运行时环境（JRE），它包含 JVM 和 Java 标准 API。第二个选项是 Java 开发工具包（JDK），它是 JRE 加上用于 Java 的软件开发工具包（SDK）（例如 `javac` 编译器和其他工具）。由于我们使用 Java 进行开发，我建议使用 JDK（尽管某些 IDE 也内置了 Java SDK）。关于 Java 版本，我建议至少使用 JDK 8，因为在撰写本文时它是 Selenium 4 支持的长期支持（LTS）版本。

## 文本编辑器或 IDE

为了编写我们的 Java 测试，我们需要一个文本编辑器或 IDE。IDE 为开发提供了出色的体验，因为它们拥有完整的环境（用于编码、运行、调试、自动补全等）。尽管如此，你也可以使用任何喜欢的文本编辑器，结合命令行工具（用于运行、调试等）获得类似的实践。总的来说，选择取决于你的个人偏好。一些流行的文本编辑器替代方案有 Sublime Text、Atom、Notepad++ 或 Vim 等。关于 IDE，我们可以使用 Eclipse、IntelliJ IDEA、NetBeans 或 Visual Studio Code。

## 浏览器和驱动

使用 Selenium WebDriver 进行自动化的初始方式是使用本地浏览器。对于本书，我考虑以下浏览器：Chrome、Edge 和 Firefox。我将它们称为 *主要浏览器*，原因有几个。首先，它们在全球非常流行，而且我们使用 Selenium WebDriver 测试 Web 应用程序，很可能希望使用与潜在用户相同的浏览器。其次，这些浏览器是 *常青* 的（即它们会自动升级）。第三，这些浏览器可用于主要操作系统：Windows、Linux 和 macOS（不像 Safari，它也是一个流行的浏览器，但仅在 macOS 上可用）。最后，这些浏览器可在 GitHub 仓库中使用的 CI 环境中使用（即 GitHub Actions）。

控制 Web 浏览器使用 Selenium WebDriver 的最后一个要求是驱动程序二进制文件：chromedriver（用于 Chrome）、msedgedriver（用于 Edge）和 geckodriver（用于 Firefox）。如第 1 章所述，驱动管理涉及三个步骤：下载、设置和维护。为了避免该章节中解释的潜在问题，我强烈建议使用 WebDriverManager 来自动化此过程。

> **TIP**
>
> *附录 2 提供了 WebDriverManager 执行的自动化驱动管理过程的精细细节。此外，以防你出于某种原因需要，本附录还解释了如何手动进行驱动管理。*

## 构建工具

另一个重要组件是 *构建工具*。构建工具是用于从源代码自动化创建可执行应用程序的软件实用程序。这些工具在依赖管理、编译、打包、测试执行和部署方面简化了项目管理。总的来说，构建工具是自动化软件项目开发的便捷方式，无论是在构建服务器（例如 GitHub Actions）还是开发人员机器上。因此，我强烈建议使用构建工具来设置你的项目。本书中涵盖的替代方案有：

- Maven 是一个由 Apache 软件基金会维护的开源构建自动化工具。它主要用于 Java 项目，尽管它也支持其他语言，如 C#、Ruby 或 Scala。
- Gradle 是另一个用于软件开发的开源构建自动化工具。它支持 Java 和其他语言，如 Kotlin、Groovy、Scala、C/C++ 或 JavaScript。

推荐的版本是 Maven 3+ 和 Gradle 6+。为完整起见，我在示例仓库中使用了两种构建工具。再次，选择使用哪一个取决于你的偏好。

> **NOTE**
>
> 如果你计划使用 IDE 进行开发和运行测试，构建工具并非严格必要。尽管如此，我建议在你的电脑上至少安装其中一种工具，以复制在构建服务器（例如 Jenkins、GitHub Actions 等）中通常使用的相同环境。

## 可选软件

除了已解释的软件外，还有一些额外的程序可以让你更好地利用本书。首先，你可以使用 Git 进行源代码管理。由于本书中介绍的测试示例可在 GitHub 上获取，你可以使用 Git 来 fork（或克隆）并更新此仓库。

第二个可选工具是 Docker。在本书中，我向你展示了如何使用 Docker 执行容器化浏览器（参见第 6 章）。因此，我强烈建议你在电脑上安装 Docker Engine（它适用于 Linux、macOS 和 Windows 10）。

最后，如果需要，你可以使用不同的 Web 浏览器。除了主要浏览器（Chrome、Edge 和 Firefox）之外，还可以使用其他浏览器与 Selenium WebDriver，例如 macOS 上的 Safari，或任何操作系统上的 Opera、Chromium 和 HtmlUnit（一种无头浏览器，即无 GUI 浏览器）。

## 旧版浏览器

除了已介绍的浏览器之外，你还可以使用更多浏览器与 Selenium WebDriver。然而，我不推荐这些浏览器，因为它们已弃用且不再维护。这些浏览器是：

- Internet Explorer：微软为 Windows 系统推出的网络浏览器，首次发布于 1995 年，于 2022 年停止维护。
- PhantomJS：无头浏览器，首次发布于 2011 年，于 2018 年停止维护。

## 项目设置

你可以在以下 GitHub 仓库中找到本书的所有代码示例：https://github.com/bonigarcia/selenium-webdriver-java。该仓库是开源的，采用 Apache 2.0 许可证发布。该仓库的目标是多方面的。首先，它是集中所有示例的便捷方式。此外，你可以使用其设置（Maven 或 Gradle）作为你项目的骨架。

> **TIP**
>
> 以下小节描述了创建包含 Selenium WebDriver 测试的 Java 项目的一般要求。附录 3 提供了示例仓库配置的低级细节。

### 项目布局

项目布局是用于存储软件项目不同资产（例如源代码、二进制文件、静态资源等）的目录结构。Maven 和 Gradle 都使用等效的 Java 项目布局。得益于此，我们可以使用两种构建工具执行示例仓库。

如图 2-1 所示，以下文件夹集（标记为脚手架文件夹）在两种构建工具中是相同的：

- `src/main/java`：应用程序源代码（即 Java 文件）。
- `src/main/resources`：应用程序资源文件（例如属性、配置文件等）。
- `src/test/java`：测试源代码（即用于测试的 Java 文件）。
- `src/test/resources`：测试资源文件（即用于测试的额外资产）。

![](img/b67ec8e49249e340add6ed66ab413d78_65_0.png)

图 2-1. Maven 和 Gradle 中的项目布局

其余项目布局在两种构建工具中不同。第一个区别是配置文件。一方面，该文件在 Maven 中是唯一的，称为 `pom.xml`（项目对象模型）。另一方面，Gradle 用于配置有两个文件，称为 `settings.gradle` 和 `build.gradle`。Maven 和 Gradle 之间的第二个区别是输出文件夹。在这两种情况下，构建工具都会创建此文件夹以保存构建结果（即编译后的类、生成的打包文件等）。该文件夹的名称在 Maven 中为 `target`，在 Gradle 中为 `build`。最后，Gradle

