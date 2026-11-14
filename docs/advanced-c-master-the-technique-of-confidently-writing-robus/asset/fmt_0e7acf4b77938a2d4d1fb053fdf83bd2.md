
## CMake 配置与测试执行

```cmake
add_executable(tests CanTest.cpp SumFuncTest.cpp ../src/SumFunc.cpp) # added files here

target_link_libraries(tests GTest::GTest)

gtest_discover_tests(tests)
```

注意，我们将测试文件（`SumFuncTest.cpp`）和被测试的代码（`../src/SumFunc.cpp`）都添加到了可执行文件中，因为我们的测试代码使用了实际项目中的代码。

4. 构建项目并像之前一样运行测试。你应该会看到以下报告：

![Figure 1.31: Output after running the test](image/C14508_01_31.jpg)

**Figure 1.31: Output after running the test**

我们可以向项目添加此类测试，所有测试都将如上图所示显示在屏幕上。

5. 现在，让我们添加一个实际会失败的测试。在 **tests/SumFuncTest.cpp** 文件中，进行以下更改：

```cpp
TEST_F(SumFuncTest, CanSumCorrectly) {
    EXPECT_EQ(7, sum(3, 4));
}

// add this test
TEST_F(SumFuncTest, CanSumAbsoluteValues) {
    EXPECT_EQ(6, sum(3, -3));
}
```

注意，此测试假设输入的绝对值被相加，这是不正确的。该调用的结果是 **0**，但在此示例中预期为 **6**。这是我们在项目中添加此测试所需的唯一更改。

6. 现在，构建项目并运行测试。你应该会看到此报告：

![Figure 1.32: The build report](image/C14508_01_32.jpg)

**Figure 1.32: The build report**

正如你在前图中所见，前两个测试通过了，最后一个测试失败了。当我们看到此输出时，有两种可能：要么我们的项目代码有误，要么测试有误。在这种情况下，我们的测试有误。这是因为我们的 `CanSumAbsoluteValues` 测试用例预期 **6** 等于 `sum(3, -3)`。这是因为我们假设我们的函数会对提供的整数的绝对值求和。然而，事实并非如此。我们的函数只是简单地相加给定的数字，无论它们是正数还是负数。因此，此测试基于错误的假设而失败。

7. 让我们更改测试并修复它。更改测试，使我们预期 **-3** 和 **3** 的和为 **0**。重命名测试以反映此测试实际的功能：

```cpp
TEST_F(SumFuncTest, CanSumCorrectly) {
    EXPECT_EQ(7, sum(3, 4));
}

// change this part
TEST_F(SumFuncTest, CanUseNegativeValues) {
    EXPECT_EQ(0, sum(3, -3));
}
```

8. 现在运行它，并在报告中观察所有测试均通过：

![Figure 1.33: Test execution is successful](image/C14508_01_33.jpg)

**Figure 1.33: Test execution is successful**

最后，我们已在系统和项目中使用 CMake 设置了 Google Test。我们还编写、构建并运行了使用 Google Test 的单元测试，既在终端中也在 Eclipse 中。理想情况下，你应该为每个类编写单元测试并覆盖每种可能的用法。你还应该在每次重大更改后运行测试，并确保不破坏现有代码。在下一节中，我们将执行添加新类及其测试的活动。

### Activity 2: Adding a New Class and Its Test

随着 C++ 项目的开发，你会随着项目增长向其中添加新的源文件。你也会为它们编写测试以确保其正常工作。在此活动中，我们将添加一个模拟 **1D** 线性运动的新类。该类将包含用于 **position** 和 **velocity** 的 double 字段。它还将有一个 `advanceTimeBy()` 方法，该方法接收一个 double `dt` 参数，根据 `velocity` 的值修改 `position`。对 double 值使用 `EXPECT_DOUBLE_EQ` 而不是 `EXPECT_EQ`。在此活动中，我们将向项目添加一个新类及其测试。执行以下步骤以完成此活动：

1. 在 Eclipse IDE 中打开我们创建的项目。
2. 向包含 `LinearMotion1D` 类的项目添加 **LinearMotion1D.cpp** 和 **LinearMotion1D.h** 文件对。在此类中，创建两个 double 字段：**position** 和 **velocity**。此外，创建一个 `advanceTimeBy(double dt)` 函数来修改 **position**。
3. 在 **tests/LinearMotion1DTest.cpp** 文件中为此编写测试。编写两个表示两个不同方向运动的测试。
4. 在 Eclipse IDE 中构建并运行它。
5. 验证测试已通过。

最终测试结果应类似于以下内容：

![Figure 1.34: Final test results](image/C14508_01_34.jpg)

**Figure 1.34: Final test results**

#### Note

此活动的解决方案可在 622 页找到。

添加新类及其测试是 C++ 开发中非常常见的任务。我们出于各种原因创建类。有时，我们有一个良好的软件设计计划，并创建它所要求的类。其他时候，当一个类变得过大且单一时，我们会以有意义的方式将其部分职责分离到另一个类中。使此任务变得实用对于防止拖延并最终得到巨大的单一类非常重要。在下一节中，我们将讨论编译和链接阶段发生的情况。这将使我们更好地了解 C++ 程序的内部机制。

## Understanding Compilation, Linking, and Object File Contents

使用 C++ 的主要原因之一是效率。C++ 赋予我们对内存管理的控制权，这就是为什么理解对象在内存中的布局很重要。此外，C++ 源文件和库被编译为目标硬件的目标文件并链接在一起。C++ 程序员经常需要处理链接器问题，这就是为什么理解编译步骤并能够检查目标文件很重要。另一方面，大型项目由团队在长时间内开发和维护，这就是为什么创建整洁且可理解的代码很重要。与任何其他软件一样，Bug 会在 C++ 项目中出现，需要通过观察程序行为仔细识别、分析和解决。因此，学习如何调试 C++ 代码也很重要。在下一节中，我们将学习如何创建高效、能与其他代码良好配合且可维护的代码。

### Compilation and Linking Steps

C++ 项目作为一组源代码文件和组织源文件和库依赖的项目配置文件创建。在编译步骤中，这些源文件首先被转换为目标文件。在链接步骤中，这些目标文件被链接在一起形成可执行文件，这是项目的最终输出。项目使用的库也在此步骤中链接。

在即将进行的练习中，我们将使用现有项目来观察编译和链接阶段。然后，我们将手动重现它们以更详细地查看该过程。

### Exercise 7: Identifying Build Steps

你一直在构建项目而未调查构建操作的细节。在此练习中，我们将调查项目构建步骤的细节。执行以下操作以完成练习：

1. 打开终端。
2. 通过输入以下命令导航到我们的 `Makefile` 文件所在的 **build** 文件夹：

```bash
cd build/Debug
```

3. 使用以下命令清理项目并以 **VERBOSE** 模式运行构建：

```bash
make clean
make VERBOSE=1 all
```

你将在终端中获得构建过程的详细输出，这可能看起来有点拥挤：

![Figure 1.35: The build process part 1](image/C14508_01_35.jpg)

**Figure 1.35: The build process part 1**

![Figure 1.36: The build process part 2](image/C14508_01_36.jpg)

**Figure 1.36: The build process part 2**

![Figure 1.37: The full build output](image/C14508_01_37.jpg)

**Figure 1.37: The full build output**
