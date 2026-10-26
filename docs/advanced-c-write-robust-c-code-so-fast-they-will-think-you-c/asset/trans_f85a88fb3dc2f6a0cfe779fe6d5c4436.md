

![](img/6507d435e2edfa3c00c220d0ccfcaf94_57_0.png)

图 1.37：完整的构建输出

以下是此输出中的一些行。以下行与主可执行文件的编译和链接相关：

/usr/bin/c++   -g   -pthread -std=gnu++1z -o CMakeFiles/CxxTemplate.dir/src/CxxTemplate.cpp.o -c /home/username/Packt/Cpp2019/CxxTemplate/src/CxxTemplate.cpp

/usr/bin/c++   -g   -pthread -std=gnu++1z -o CMakeFiles/CxxTemplate.dir/src/ANewClass.cpp.o -c /home/username/Packt/Cpp2019/CxxTemplate/src/ANewClass.cpp

/usr/bin/c++   -g   -pthread -std=gnu++1z -o CMakeFiles/CxxTemplate.dir/src/SumFunc.cpp.o -c /home/username/Packt/Cpp2019/CxxTemplate/src/SumFunc.cpp

/usr/bin/c++  -g  -pthread -std=gnu++1z -o CMakeFiles/CxxTemplate.dir/src/LinearMotion1D.cpp.o -c /home/username/Packt/Cpp2019/CxxTemplate/src/LinearMotion1D.cpp

/usr/bin/c++ -g  CMakeFiles/CxxTemplate.dir/src/CxxTemplate.cpp.o CMakeFiles/CxxTemplate.dir/src/ANewClass.cpp.o CMakeFiles/CxxTemplate.dir/src/SumFunc.cpp.o CMakeFiles/CxxTemplate.dir/src/LinearMotion1D.cpp.o  -o CxxTemplate -pthread

4. 这里的 c++ 命令只是指向 g++ 编译器的符号链接。要查看它实际上是一串符号链接，请键入以下命令：

```
namei /usr/bin/c++
```

你将看到以下输出：

![](img/6507d435e2edfa3c00c220d0ccfcaf94_58_0.png)

图 1.38：/usr/bin/c++ 的符号链接链

因此，在我们的讨论中，我们将 c++ 和 g++ 互换使用。在前面引用的构建输出中，前四行是将每个 .cpp 源文件编译并创建相应的 .o 目标文件。最后一行是将这些目标文件链接在一起以创建 CxxTemplate 可执行文件。以下图示直观地呈现了此过程：

8. 在下一步中，将目标文件链接在一起形成我们的可执行文件。键入以下命令：

/usr/bin/c++  CxxTemplate.o ANewClass.o SumFunc.o LinearMotion1D.o  -o
CxxTemplate

9. 现在，通过键入以下命令查看我们的工作目录中的可执行文件：

```
ls
```

这将在以下图中显示新的 CxxTemplate 文件：

![](img/6507d435e2edfa3c00c220d0ccfcaf94_60_0.png)

图 1.41：链接后的可执行文件

10. 现在，通过键入以下命令运行我们的可执行文件：

```
./CxxTemplate
```

并查看我们之前看到的输出：

![](img/6507d435e2edfa3c00c220d0ccfcaf94_60_1.png)

图 1.42：可执行文件输出

现在你已经检查了构建过程的细节并自行重现了它们，在下一节中，让我们探索链接过程。

## 链接步骤

在本节中，让我们看一下两个源文件之间的连接以及它们如何最终进入同一个可执行文件。查看以下图中的 sum 函数：

![](img/6507d435e2edfa3c00c220d0ccfcaf94_61_0.png)

图 1.43：链接过程

sum 函数的函数体在 SumFunc.cpp 中定义。它在 SumFunc.h 中有前向声明。这样，想要使用 sum 函数的源文件就可以知道其签名。一旦它们知道签名，它们就可以调用它并信任实际的函数定义会在运行时存在，而无需与 SumFunc.cpp 产生任何实际交互，而函数就在那里定义。

编译后，调用 sum 函数的 CxxTemplate.cpp 会将其调用带入其目标文件。仍然，它不知道函数定义在哪里。SumFunc.cpp 的目标文件有该定义，但它与 CxxTemplate.o 尚无关系。

在链接步骤中，链接器将 CxxTemplate.o 中的调用与 SumFunc.o 中的定义匹配。因此，调用在可执行文件中正常工作。如果链接器找不到 sum 函数的定义，它将给出链接错误。

链接器通过其名称和参数查找 sum 函数。这称为解析符号。对象文件中定义的类、函数和变量被放置在符号表中，每个符号的引用都通过在此表中的查找来解析。当符号不存在时，你会收到符号无法解析错误。

这带我们经历了构建过程的两个阶段：**编译**和**链接**。请注意，与我们手动编译源代码时相比，我们使用了更简单的命令。随时键入 `man g++` 以查看其中的所有选项。后来，我们讨论了链接以及如何解析符号。我们还谈到了链接步骤中可能出现的问题。在下一节中，我们将学习有关对象文件的内容。

## 深入：查看对象文件

为了使链接步骤无错误地工作，我们需要使所有符号引用与符号定义匹配。大多数时候，我们只需查看源文件就可以分析事物将如何被解析。有时，在复杂的情况下，我们可能很难理解为什么某个符号无法被解析。在这种情况下，查看对象文件的内容以调查引用和定义可能有助于解决问题。除了链接错误之外，了解对象文件内容以及链接的一般工作原理对 C++ 程序员也很有用。了解幕后发生的事情可以帮助程序员更好地理解整个过程。

当我们的源代码编译为对象文件时，我们的语句和表达式会被转换为汇编代码，即 CPU 理解的低级语言。汇编中的每条指令都包含一个操作，后跟操作数，它们是 CPU 的寄存器。有指令将数据加载到寄存器中、从寄存器中加载数据以及对寄存器中的值进行操作。Linux 中的 `objdump` 命令可以帮助我们查看这些对象文件的内容。

> **注意**
> 
> 我们将使用 Compiler Explorer，这是一个更易于使用的在线工具，你可以在左侧窗口编写代码，右侧可以看到编译后的汇编代码。以下是 Compiler Explorer 的链接：https://godbolt.org。

虽然不是很清楚，但你或多或少可以看出它在做什么。我们不会深入汇编代码的细节，但我们将重点关注链接器阶段期间如何解析符号。现在让我们关注以下几行：

```
sum(int, int):
```

```
...
```

```
callSum():
```

```
...
    call  sum(int, int)
```

```
...
```

**call sum(int, int)** 行按预期执行：它调用前面的 **sum** 函数并将参数放在某些寄存器中。这里的重要一点是，函数通过其名称及其参数的类型按顺序标识。链接器会查找具有此签名的适当函数。请注意，返回值不是签名的一部分。

- 2. 禁用 **Demangle** 复选框并查看这些函数名称实际是如何存储的：

![](img/6507d435e2edfa3c00c220d0ccfcaf94_65_0.png)

# 图 1.45：未进行去修饰的编译代码

这里，我们的行变成了这样：

```
_Z3sumii:

...

_Z7callSumv:

...
    call    _Z3sumii

...
```

以下是这些添加行的编译版本：

![](img/6507d435e2edfa3c00c220d0ccfcaf94_67_0.png)

**图 1.46：编译版本**

调试器是处理运行时错误的更好工具。调试器可以让你逐行运行代码，继续运行并在想要的行上暂停，调查内存的值，并在错误时暂停，等等。这让你可以观察程序运行时内存中到底发生了什么，并识别导致不需要行为的代码行。

gdb 是可以调试 C++ 程序的经典命令行调试器。然而，使用起来可能很困难，因为调试本质上是一个视觉任务——你希望能够同时查看代码行、变量值和程序输出。幸运的是，Eclipse CDT 包含一个易于使用的可视化调试器。

## 练习 9：使用 Eclipse CDT 调试

你一直只是运行项目并查看输出。现在你想学习如何详细调试代码。在本练习中，我们将探索 Eclipse CDT 的调试功能。执行以下步骤来完成练习：

- 1. 在 Eclipse CDT 中打开 CMake 项目。
- 2. 为确保我们有一个现有的运行配置，点击 Run | Run Configurations。在那里，你应该在 C/C++ Application 下看到一个 CxxTemplate 条目。
- 3. 关闭对话框以继续。

> 注意
> 
> 由于我们之前运行过项目，它应该在那里。如果没有，请返回并重新创建它。

