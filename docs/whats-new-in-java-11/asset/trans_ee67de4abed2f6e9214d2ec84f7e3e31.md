

## Table 1-1. *从 Java 11 中移除的模块*

| Module | Packages |
|---|---|
| JavaBeans Activation Framework (JAF) | `javax.activation` |
| Java Transaction API (JTA) | `java.transaction` |
| Commons Annotation API | `java.xml.ws.annotation` |
| Java Architecture for XML Binding (JAXB) | `jdk.xml.bind` |
| Java API for XML-Based Web Services (JAX-WS) | `jdk.xml.ws` |
| CORBA | `javax.activity`, `javax.rmi`, `org.omg` |

那么，如果你在代码中使用了这些模块中的一个并直接升级到 Java 11 会怎样？你的代码将无法再编译，或者你可能会遇到运行时错误。虽然之前的版本允许通过手动使用 `--add-modules` 添加模块来变通，但这在 Java 11 中不再有效。

最好的办法是找到作为外部依赖可用的功能替代品并将其添加到你的编译类路径中。你会发现许多被移除的模块在 Maven Central 上都有对应的表示。表 1-2 列出了一些可能的替代方案。有关更多参考，请参阅 JEP 320。

## Table 1-2. *Java 11 中被移除模块的替代库*

| Module | Replacement on Maven Central |
|---|---|
| JavaBeans Activation Framework (JAF) | `javax.activation:javax.activation-api` |
| Java Transaction API (JTA) | `javax.transaction:javax.transaction-api` |
| Commons Annotation API | `javax.annotation:javax.annotation-api` |
| Java Architecture for XML Binding (JAXB) | `org.glassfish.jaxb:jaxb-runtime` |
| Java API for XML-Based Web Services (JAX-WS) | `com.sun.xml.ws:jaxws-ri` |
| CORBA | `org.glassfish.corba:glassfish-corba` |

为了说明迁移场景，我们假设你正在处理一个需要处理 XML 的项目。代码库的一部分使用 JAXB 将 POJO 及其字段值转换为 XML。在下面的列表中，你可以找到这样一个 POJO，包括 JAXB 典型的注解：

```
import javax.xml.bind.annotation.XmlElement;
import javax.xml.bind.annotation.XmlRootElement;

@XmlRootElement(name = "person")
public class Person {
    private String name;

    @XmlElement(name = "fullName")
    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}
```

使用 Java 11 编译该类会产生类似的错误，如以下片段所示。JAXB 类已从 JDK 中移除：

```
/Users/bmuschko/dev/projects/whats-new-in-java-11/src/main/java/
com/bmuschko/javall/removals/jaxb/Person.java
Error:(3, 33) java: package javax.xml.bind.annotation
does not exist
Error:(4, 33) java: package javax.xml.bind.annotation
does not exist
Error:(6, 2) java: cannot find symbol
  symbol: class XmlRootElement
```

要修复编译问题，你需要在构建文件中添加一个外部依赖。以下 XML 片段演示了在 Maven pom.xml 文件中使用 Glassfish JAXB 运行时依赖：

```
<dependencies>
    <dependency>
        <groupId>org.glassfish.jaxb</groupId>
        <artifactId>jaxb-runtime</artifactId>
        <version>2.3.1</version>
    </dependency>
</dependencies>
```

当然，你也可以将相同的依赖添加到 Gradle 构建脚本中。以下示例构建脚本在 Java 库项目的上下文中声明了该依赖：

```
plugins {
    id 'java-library'
}

repositories {
    mavenCentral()
}

dependencies {
    implementation 'org.glassfish.jaxb:jaxb-runtime:2.3.1'
}
```

除了被移除的 Java EE 模块外，Java 11 还移除了一些 API。由于这些移除对终端用户的影响相对较小，我们在此不做进一步详细说明。有关更多信息，请查看已移除 API 的列表。

## Applets and Java Web Start

还记得那些我们主要在 Applet 上下文中思考 Java 的日子吗？好吧，那个时代早已过去。今天，Java 在许多其他上下文中使用，例如服务器
应用程序、移动和物联网 (IoT) 设备。

快进到 2018 年，浏览器提供商已开始移除或已移除对 Flash 和 Java 等基于插件技术的支持，因为性能和安全方面的担忧。Java 9 遵循这一趋势并在 2017 年弃用了对 Applets 的支持。Java 11 完成了这一循环并完全移除了 Applets，且没有替代品。

Java Web Start 提供了一种从浏览器下载 Java 桌面应用程序而不是在浏览器中运行的方式。对许多用户来说，Java 11 也移除了对 Java Web Start 的支持，这有点令人惊讶。因此，将 Applet 转换为 Java Web Start 应用程序已不可行。

如果你仍在维护基于 Applets 或 Java Web Start 的应用程序，如果计划升级到 Java 11，你将需要寻找替代方案。下一节涉及 JavaFX，这是在桌面应用程序开发中一个有效且强大的选择。

## JavaFX Migrates to OpenJFX

从 Java 11 开始，JavaFX——用于构建 Java 客户端应用程序的技术——不再随 Oracle JDK 分发。但这并不意味着该项目已死亡。它只是被精简为 OpenJFX，即 JavaFX 的开源对应物。因此，OpenJFX 可以独立于 JDK 发布周期发布新版本。尤其是在更广泛的 Java 社区参与下，这是一个巨大的胜利。我们很可能会看到更频繁的发布。

OpenJFX 提供了一个非常易于接近且详细的文档页面，如图 1-7 所示。你可以将其技术与 Maven 和 Gradle 构建流程集成。开始使用 OpenFX 从未如此简单。

![](img/2331c4028acf1b9f21065457e9b2d8d4_28_0.png)

Figure 1-7. The community-driven home of OpenJFX

与之前的 Java 版本一样，功能和 API 在实际移除之前会经历弃用周期，我们将在下一节讨论。

## Deprecated Features and APIs

Java 11 弃用了 Nashorn JavaScript 引擎、一些 VM 选项以及对 Pack200 压缩方案的支持。我强烈建议尽快采用推荐的替代方案，以确保顺利升级到 Java 11 之后的版本。让我们逐一介绍。

### NOTE

> 很难跟踪所有 Java 版本，尤其是在跨多个版本跳跃时。工具 jdeps 有助于检测项目代码及其依赖项中使用已弃用 API 的情况。将 jdeps 集成到你的持续集成流程中是一个好主意。你将获得快速反馈并深入了解已弃用 API 的使用情况。

### Nashorn JavaScript Engine

*Pack200* 是在 Java 5 中引入的用于 JAR 文件的压缩方案。该压缩机制在 Applet 上下文中尤其流行。如前所述，Applet 的支持已被放弃，因此该工具变得不太有用。正因如此，Java 将其标记为已弃用。有关更多信息，请参阅 JEP 336。

本报告中最后一个但非常重要的部分涵盖了性能和安全。

## Performance and Security

开发人员通常会寻找新 Java 版本中那些直接影响并改善日常工作流程的闪亮、便捷功能。另一方面，非功能性方面如性能和安全，在你将项目发布到生产环境后变得极其重要。运行缓慢的应用程序不会赢得客户。运行时环境中的安全漏洞会导致用户信任度下降。Java 11 在这方面也能提供支持。首先，我们来看性能改进。

## Garbage Collection

垃圾收集是 Java 的内置功能，负责对象的创建和销毁。让我们首先回顾一下对默认垃圾收集器的改进。稍后，我们将分析本次发布中添加的新实验性垃圾收集器。

### IMPROVEMENTS TO THE DEFAULT G1 COLLECTOR

Java 7 引入了 Garbage-First (G1) 收集器。它后来在 Java 9 中成为默认垃圾收集器。G1 针对高概率和吞吐量进行了优化。正如 Thomas Schatzl 所提到的，Java 11 在 x64 处理器上将暂停时间提高了高达 60%，同时内存占用大幅减少。对于准备将 Java 11 升级到生产系统的人来说，这是一个巨大的提升。

此外，本次 Java 发布引入了两个实验性垃圾收集器。让我们看看它们能带来什么。我们从 ZGC 开始。

### ZGC: A SCALABLE, LOW-LATENCY GARBAGE COLLECTOR

垃圾收集是 Java 的一项引人注目的功能。作为开发人员，你无需在不再使用对象后手动标记对象以便移除。垃圾收集器会在幕后处理此事。你可能会遇到的问题之一是垃圾收集启动时的暂停时间。这会导致糟糕的客户体验，因为用户必须等待应用程序响应。对于致力于响应性、高吞吐量和可用性并具有约定服务级别的任务关键型应用程序来说，等待时间更是致命的。

