

![Figure 5.9: The result of the correct exercise execution](image/C14583_05_09.jpg)

**Figure 5.9: The result of the correct exercise execution**

现在，我们将继续进行更改，以演示竞态条件。

6. 返回函数，在 `if` 条件后将子线程的休眠时间设置为 `2s`。添加日志：

```
if (0 != dividend)
{
    std::cout << "Child thread goes sleep" << std::endl;
    using namespace std::chrono_literals;
    std::this_thread::sleep_for(2s);
    std::cout << "Child thread woke up" << std::endl;
    std::cout << "Dividend = " << dividend << std::endl;
    std::cout << (divisor / dividend) << std::endl;
}
```

7. 返回 `main()` 函数，将主线程的休眠时间设置为 `1s`。之后，将 `dividend` 变量设置为 `0`。添加日志：

```
std::cout << "Main thread goes sleep" << std::endl;
using namespace std::chrono_literals;
std::this_thread::sleep_for(1s);
std::cout << "Main thread woke up" << std::endl;
dividend = 0;
std::cout << "Main thread set dividend to 0" << std::endl;
```

> **Note**
>
> `std::chrono_literals` 命名空间包含用于时间表示的字面量：`h` 代表**小时**，`min` 代表**分钟**，`s` 代表**秒**，`ms` 代表**毫秒**，`us` 代表**微秒**，`ns` 代表**纳秒**。要使用它们，只需将它们添加到数字末尾，例如 `1s`、`1min`、`1h` 等。

8. 在 `main()` 函数退出前添加 `std::getchar()` 函数调用。这可以防止我们关闭应用程序，从而有机会查看分离线程是如何工作的：

```
std::getchar();
```

9. 在编辑器中运行此代码。你将看到主线程休眠 `1s`。然后，子线程进入 `if` 条件并休眠 `2s`，这意味着它验证了 `dividend` 且其不等于 `0`。然后，主线程醒来并将 `dividend` 变量设置为 `0`。接着，子线程醒来并执行除法。但由于 `dividend` 现在等于 `0`，应用程序崩溃。如果你在 Debug 模式下运行此示例，将看到带有消息 "Arithmetic exception" 的 **SIGFPE exception**。你将得到以下输出：

![Figure 5.10: The result of the exercise's execution with race conditions](image/C14583_05_10.jpg)

**Figure 5.10: The result of the exercise's execution with race conditions**

在本练习中，我们考虑了“检查后行动”类型的竞态条件。我们为线程设置了休眠周期以模拟操作系统中断，但在实际程序中，这种情况很可能发生，也可能不发生。这完全取决于操作系统及其调度器。这使得调试和修复竞态条件变得极其困难。为了避免本例中的竞态条件，我们可以采取以下几种方式：

*   向线程函数传递变量的副本，而不是传递引用。
*   使用标准库原语同步线程间对共享变量的访问。
*   在主线程将 `dividend` 值更改为 `0` 之前，先连接（join）子线程。

让我们看看修复此竞态条件的几种其他方法。所有这些方法都取决于你尝试实现的任务。在下一节中，我们将讨论 C++ 标准库提供的同步原语。

## 数据冒险

之前，我们考虑了最无害的示例，但有时会出现数据损坏的情况，这会导致未定义的程序行为或异常终止。这种由竞态条件或单纯的错误设计导致的数据损坏被称为**数据冒险**。一般来说，这意味着一项工作的最终结果取决于线程执行的顺序。如果不同线程处理共享数据或全局变量，可能会因为不同线程以错误的顺序执行任务，导致结果随时间变化。这是由于数据在多线程环境下存在依赖关系而发生的。此类依赖问题有条件地分为三组：

*   **真依赖**：**写后读** (RAW)
*   **反依赖**：**读后写** (WAR)
*   **输出依赖**：**写后写** (WAW)

### RAW 依赖

当一个线程计算的值被另一个线程使用时，会发生 **RAW 依赖**。例如，**线程 A** 应完成其工作并将结果写入变量。**线程 B** 必须读取该变量的值并完成其工作。用伪代码表示如下：

```
Thread A: a = doSomeStuff();
Thread B: b = a - doOtherStuff();
```

如果 **线程 B** 先执行，就会出现困难。这将导致 **线程 B** 读取到无效值。线程执行的顺序必须严格保证。**线程 B** 必须读取变量的值，但只能在 **线程 A** 写入之后。否则，将导致未定义行为。下图将帮助你理清导致数据冒险的 RAW 数据依赖：

![Figure 5.11: RAW data dependency between two threads](image/C14583_05_11.jpg)

**Figure 5.11: RAW data dependency between two threads**

### WAR 依赖

当一个线程更改另一个线程正在使用的数据时，会发生 **WAR 依赖**。例如，**线程 A** 必须读取变量的值并完成其工作。之后，**线程 B** 应完成其工作并将结果写入变量。用伪代码表示如下：

```
Thread A: b = a - doSomeStuff();
Thread B: a = doOtherStuff();
```

如果 **线程 B** 先执行，就会出现困难。这将导致 **线程 B** 在 **线程 A** 读取值之前更改了该值。线程执行的顺序必须严格保证。**线程 B** 应仅在 **线程 A** 读取其值后，才将新值写入变量。下图将帮助你理清导致数据冒险的 WAR 数据依赖：

![Figure 5.12: WAR data dependency between two threads](image/C14583_05_12.jpg)

**Figure 5.12: WAR data dependency between two threads**

### WAW 依赖

当多个线程更改同一变量的值，且某个线程读取该值用于计算时，会发生 **WAW 依赖**。例如，**线程 A** 执行其工作并将结果写入变量。**线程 B** 读取变量的值并执行其工作。**线程 C** 执行其工作并将结果写入同一变量。用伪代码表示如下：

```
Thread A: a = doSomeStuff();
Thread B: b = a - doOtherStuff();
Thread C: a = doNewStuff();
```

如果 **线程 C** 在线程 A 和 B 之前执行，就会出现困难。这会导致 **线程 B** 读取到它不应读取的值。线程执行的顺序必须严格保证。**线程 C** 必须将新值写入变量，但只能在 **线程 A** 写入其值且 **线程 B** 读取之后。下图将帮助你理清导致数据冒险的 WAW 数据依赖：

![Figure 5.13: WAW data dependency between two threads](image/C14583_05_13.jpg)

**Figure 5.13: WAW data dependency between two threads**

## 资源同步

