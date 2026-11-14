
# 构建、运行和编写单元测试

现在，我们将讨论如何构建、运行和编写单元测试。到目前为止，我们的示例是一个简单的虚拟测试，已准备好构建和运行。稍后，我们将添加更有意义的测试，并查看通过和失败测试的输出。在下面的练习中，我们将为上一个练习中创建的项目构建、运行和编写单元测试。

## 练习 5：构建和运行测试

到目前为止，你已经创建了一个配置好 `GoogleTest` 的项目，但尚未构建或运行我们创建的测试。在本练习中，我们将构建并运行我们创建的测试。由于我们使用 `add_subdirectory` 添加了 `tests` 文件夹，构建项目将自动构建测试。运行测试需要一些额外的步骤。执行以下步骤以完成练习：

1. 在 `Eclipse CDT` 中打开我们的 `CMake` 项目。

2. 要构建测试，只需像以前一样构建项目。以下是使用 `Project | Build All` 进行完整构建后，从 Eclipse 再次构建项目的输出：

   ![图 1.20：构建操作及其输出](image/C14508_01_20.jpg)

   **图 1.20：构建操作及其输出**

3. 如果你看不到此输出，你的控制台可能处于错误的视图中。你可以按照下图所示进行修正：

   ![图 1.21：查看正确的控制台输出](image/C14508_01_21.jpg)

   **图 1.21：查看正确的控制台输出**

   ![图 1.22：查看正确的控制台输出](image/C14508_01_22.jpg)

   **图 1.22：查看正确的控制台输出**

   如你所见，我们的项目现在有两个可执行目标。它们都位于 `build` 文件夹中，就像其他构建产物一样。它们的位置分别是 `build/Debug/CxxTemplate` 和 `build/Debug/tests/tests`。既然它们是可执行文件，我们只需运行它们即可。

4. 我们之前运行过 `CxxTemplate`，现在不会看到任何额外输出。在我们位于项目文件夹时，在终端中输入以下命令来运行另一个可执行文件：

   `./build/Debug/tests/tests`

   上述命令在终端中生成以下输出：

   ![图 1.23：运行测试可执行文件](image/C14508_01_23.jpg)

   **图 1.23：运行测试可执行文件**

   这是我们 `tests` 可执行文件的简单输出。如果你想查看测试是否通过，只需运行它即可。然而，测试远不止于此。

5. 运行测试的方法之一是使用 `ctest` 命令。在你位于项目文件夹时，在终端中输入以下命令。我们进入 `tests` 可执行文件所在的文件夹，在那里运行 `ctest`，然后返回：

   ```
   cd build/Debug/tests
   ctest
   cd ../../..
   ```

   你将看到以下输出：

   ![图 1.24：运行 ctest](image/C14508_01_24.jpg)

   **图 1.24：运行 ctest**

### 注

`ctest` 命令可以使用多种选项运行你的 `tests` 可执行文件，包括自动将测试结果提交到在线仪表板的功能。在这里，我们将仅运行 `ctest` 命令；其更多功能留待感兴趣的读者自行探索。你可以输入 `ctest --help` 或访问在线文档，在 [https://cmake.org/cmake/help/latest/manual/ctest.1.html#](https://cmake.org/cmake/help/latest/manual/ctest.1.html#) 进一步了解 `ctest`。

6. 运行测试的另一种方法是在 Eclipse 内部运行它们，以获得漂亮的图形报告格式。为此，我们将创建一个测试感知的运行配置。在 Eclipse 中，点击 `Run | Run Configurations…`，在左侧右键点击 `C/C++ Unit`，然后选择 `New Configuration`。

7. 将名称从 `CxxTemplate Debug` 更改为 `CxxTemplate Tests`，如下所示：

   ![图 1.25：更改运行配置的名称](image/C14508_01_25.jpg)

   **图 1.25：更改运行配置的名称**

8. 在 `C/C++ Application` 下，选择 `Search Project` 选项：

   ![图 1.26：运行配置](image/C14508_01_26.jpg)

   **图 1.26：运行配置**

9. 在新对话框中选择 `tests`：

   ![图 1.27：创建测试运行配置并选择测试可执行文件](image/C14508_01_27.jpg)

   **图 1.27：创建测试运行配置并选择测试可执行文件**

10. 接下来，转到 `C/C++ Testing` 选项卡，在下拉菜单中选择 `Google Tests Runner`。点击对话框底部的 `Apply`，然后点击 `Run` 选项以首次运行测试：

    ![图 1.28：运行配置](image/C14508_01_28.jpg)

    **图 1.28：运行配置**

11. 在后续的运行中，你可以点击工具栏中播放按钮旁边的下拉菜单，或选择 `Run | Run History` 来选择 `CxxTemplate Tests`：

    ![图 1.29：完成运行配置设置并选择要运行的配置](image/C14508_01_29.jpg)

    **图 1.29：完成运行配置设置并选择要运行的配置**

    结果将类似于以下截图：

    ![图 1.30：单元测试的运行结果](image/C14508_01_30.jpg)

    **图 1.30：单元测试的运行结果**

    这是一份漂亮的报告，包含所有测试的条目——目前只有一个。如果你不想离开 IDE，可能会更喜欢这种方式。此外，当你有很多测试时，此界面可以帮助你有效地筛选它们。现在你已经构建并运行了使用 `Google Test` 编写的测试。你用几种不同的方式运行了它们，包括直接执行测试、使用 `ctest` 和使用 `Eclipse CDT`。在下一节中，我们将完成一个练习，实际测试我们代码的功能。

## 练习 6：测试代码功能

你已经运行了简单的测试，但现在想要编写测试功能的有意义的测试。在最初的活动中，我们创建了包含 `sum` 函数的 `SumFunc.cpp`。现在，在本练习中，我们将为该文件编写测试。在此测试中，我们将使用 `sum` 函数添加两个数字并验证结果是否正确。让我们回顾一下之前包含 `sum` 函数的以下文件内容：

### `src/SumFunc.h`

```cpp
#ifndef SRC_SUMFUNC_H_
#define SRC_SUMFUNC_H_

int sum(int a, int b);

#endif /* SRC_SUMFUNC_H_ */
```

### `src/SumFunc.cpp`

```cpp
#include "SumFunc.h"
#include <iostream>

int sum(int a, int b) {
  return a + b;
}
```

### `CMakeLists.txt` (relevant lines)

```cmake
add_executable(CxxTemplate
  src/CxxTemplate.cpp
  src/ANewClass.cpp
  src/SumFunc.cpp
)
```

### `CantTest.cpp`

```cpp
#include "gtest/gtest.h"

namespace {

class CanTest: public ::testing::Test {};

TEST_F(CanTest, CanReallyTest) {
  EXPECT_EQ(0, 0);
}

}

int main(int argc, char **argv) {
  ::testing::InitGoogleTest(&argc, argv);
  return RUN_ALL_TESTS();
}
```

执行以下步骤以完成练习：

1. 在 `Eclipse CDT` 中打开我们的 `CMake` 项目。

2. 添加一个新的测试源文件 `tests/SumFuncTest.cpp`，内容如下：

   ```cpp
   #include "gtest/gtest.h"
   #include "../src/SumFunc.h"

   namespace {

   class SumFuncTest: public ::testing::Test {};

   TEST_F(SumFuncTest, CanSumCorrectly) {
     EXPECT_EQ(7, sum(3, 4));
   }

   }
   ```

   注意，此文件没有 `main()` 函数，因为 `CanTest.cpp` 已经有一个，它们将被链接在一起。其次，注意它包含了位于项目 `src` 文件夹中的 `SumFunc.h`，并在测试中将其用作 `sum(3, 4)`。这就是我们在测试中使用项目代码的方式。

3. 在 `tests/CMakeLists.txt` 文件中进行以下更改以构建测试：

   ```cmake
   include(GoogleTest)
   ```
