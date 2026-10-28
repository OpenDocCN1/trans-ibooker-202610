

在本教程中，你将学习如何使用此工具分析你的 Java 并发应用程序。

## 开始之前

在开始本教程之前，从项目的网页（http://findbugs.sourceforge.net/）下载 FindBugs。你可以下载独立应用程序或 Eclipse 插件。在本教程中，我使用了独立版本。

截至本文撰写时，FindBugs 的实际版本（3.0.1）不支持 Java 9。你可以从 https://github.com/findbugsproject/findbugs/releases/tag/3.1.0_preview1 下载支持 Java 9 的 3.1.0 预览版。

## 如何操作...

按照以下步骤实现示例：

- 1. 创建一个名为 Task 的类并实现 Runnable 接口：
```
public class Task implements Runnable {
```
- 2. 声明一个名为 lock 的私有 ReentrantLock 属性：
```
private ReentrantLock lock;
```
- 3. 实现类的构造函数：
```
public Task(ReentrantLock lock) {
    this.lock=lock;
}
```
- 4. 实现 run() 方法。获取锁的控制权，让线程休眠 2 秒，然后释放锁：
```
@Override
public void run() {
    lock.lock();
    try {
        TimeUnit.SECONDS.sleep(1);
        lock.unlock();
    } catch (InterruptedException e) {
        e.printStackTrace();
    }
}
```
- 5. 通过创建一个名为 Main 的类并包含 main() 方法来创建示例的主类：
```
public class Main {
    public static void main(String[] args) {
```
- 6. 声明并创建一个名为 lock 的 ReentrantLock 对象：
```
ReentrantLock lock=new ReentrantLock();
```
- 7. 创建 10 个 Task 对象和 10 个线程来执行任务。通过调用 run() 方法启动线程：
```
for (int i=0; i<10; i++) {
    Task task=new Task(lock);
    Thread thread=new Thread(task);
    thread.run();
}
```
- 8. 将项目导出为 .jar 文件。命名为 recipe7.jar。使用 IDE 的菜单选项或 javac 和 .jar 命令来编译和压缩你的应用程序。
- 9. 通过在 Windows 中运行 findbugs.bat 命令或在 Linux 中运行 findbugs.sh 命令来启动 FindBugs 独立应用程序。
- 10. 通过在菜单栏的 File 菜单下点击 New Project 选项来创建一个新项目：

![](img/cc06c59cd21f558f43e9475d7f212170_483_0.png)

- 11. FindBugs 应用程序显示一个用于配置项目的窗口。在 Project name 字段中，输入 Recipe07。在 Classpath for analysis 字段（jar, ear, war, zip, 或 directory）中，添加包含项目的 .jar 文件。在 Source directories 字段（可选；浏览发现的 bug 时使用的类）中，添加示例源代码的目录。参考以下截图：

![](img/cc06c59cd21f558f43e9475d7f212170_483_1.png)

- 12. 点击 **Analyze** 按钮来创建新项目并分析其代码。
- 13. *FindBugs* 应用程序显示代码分析的结果。在这种情况下，它发现了两个 bug。
- 14. 点击其中一个 bug，你将在右侧面板看到 bug 的源代码，并在屏幕底部的面板中看到 bug 的描述。

## 原理...

以下截图显示了 FindBugs 的分析结果：

![](img/cc06c59cd21f558f43e9475d7f212170_484_0.png)

分析在应用程序中检测到以下两个潜在 bug：

- 其中一个 bug 在 `Task` 类的 `run()` 方法中被检测到。如果抛出 `InterruptedException` 异常，任务不会释放锁，因为它不会执行 `unlock()` 方法。这很可能会导致应用程序出现死锁情况。
- 另一个 bug 在 `Main` 类的 `main()` 方法中被检测到，因为你直接调用了线程的 `run()` 方法，而不是调用 `start()` 方法来开始线程的执行。

如果你双击其中一个 bug，你将看到有关它的详细信息。由于你在项目的配置中包含了源代码引用，你还将看到检测到 bug 的源代码。以下截图显示了此情况的示例：

![](img/cc06c59cd21f558f43e9475d7f212170_485_0.png)

## 还有更多...

请注意，FindBugs 只能检测一些有问题的状况（与并发代码相关或不相关）。例如，如果你在 Task 类的 run() 方法中删除 unlock() 调用并重复分析，FindBugs 不会提醒你你在任务中获取了锁但永远无法释放它。

将静态代码分析工具作为一种辅助手段来提高代码质量，但不要期望它能检测到所有 bug。

## 另请参阅

- 本章中的 *Configuring NetBeans for debugging concurrency code* 教程

## 为调试并发代码配置 Eclipse

如今，几乎每个程序员，无论使用何种编程语言，都使用 IDE 创建他们的应用程序。它们在同一个应用程序中提供了许多有趣的集成功能，例如：

- 项目管理
- 自动代码生成
- 自动文档生成
- 与版本控制系统的集成
- 用于测试应用程序的调试器
- 用于创建项目和应用程序元素的不同向导

IDE 的最有帮助功能之一是调试器。使用它，你可以逐步执行应用程序并分析程序中所有对象和变量的值。

如果你使用 Java，Eclipse 是最受欢迎的 IDE 之一。它具有集成调试器，允许你测试应用程序。默认情况下，当你调试并发应用程序并且调试器找到断点时，它只会停止具有断点的线程，同时允许其他线程继续执行。在本教程中，你将学习如何更改此配置以帮助你测试并发应用程序。

## 开始之前

你必须已安装 Eclipse IDE。打开它并选择一个包含并发应用程序实现的项目，例如，书中实现的一个教程。

## 如何操作...

按照以下步骤实现示例：

- 1. 导航到 Window | Preferences。
- 2. 在左侧菜单中展开 Java 选项。
- 3. 然后，选择 Debug 选项。以下截图说明了该窗口：

![](img/cc06c59cd21f558f43e9475d7f212170_487_0.png)

- 4. 将新断点的默认挂起策略的值从 Suspend Thread 更改为 Suspend VM（在截图中用红色标记）。
- 5. 点击 OK 按钮确认更改。

## 原理...

如本教程介绍中所述，默认情况下，当你在 Eclipse 中调试并发 Java 应用程序并且调试过程找到断点时，它只会挂起首先命中断点的线程，但允许其他线程继续执行。以下截图显示了此情况的示例：

![](img/cc06c59cd21f558f43e9475d7f212170_488_0.png)

你可以看到只有 **worker-21** 被挂起（在截图中用红色标记），而其他线程正在运行。然而，在调试并发应用程序时，如果你将 **Default suspend policy for new breakpoints** 更改为 **Suspend VM**，所有线程将挂起执行并且调试过程将命中断点。以下截图显示了此情况的示例：

![](img/cc06c59cd21f558f43e9475d7f212170_489_0.png)

更改后，你可以看到所有线程都被挂起。你可以继续调试任何你想要的线程。选择最适合你需求的挂起策略。

## 为调试并发代码配置 NetBeans

软件对于开发工作正常、符合公司质量标准并且将来可以轻松修改的应用程序（在有限的时间和尽可能低的成本下）是必要的。为了实现这一目标，至关重要的是使用可以集成多个工具（编译器和调试器）的 IDE，以便在一个通用界面下促进应用程序的开发。

如果你使用 Java，NetBeans 是最受欢迎的 IDE 之一。它具有集成调试器，允许你测试你的应用程序。

在本教程中，你将学习如何更改 Netbeans 调试器的配置以帮助你测试并发应用程序。

## 开始之前

你应该已安装 NetBeans IDE。打开它并创建一个新的 Java 项目。

## 如何操作...

按照以下步骤实现示例：

- 1. 创建一个名为 Task1 的类并指定它实现 Runnable 接口：
```
public class Task1 implements Runnable {
```
- 2. 声明两个私有 Lock 属性，名为 lock1 和 lock2：
```
private Lock lock1, lock2;
```
- 3. 实现类的构造函数以初始化其属性：
```
public Task1 (Lock lock1, Lock lock2) {
    this.lock1=lock1;
    this.lock2=lock2;
}
```

