

## 练习场景

在本练习中，我们将继续上一节讨论的场景：开发人员需要编写一个函数来解析**配置文件**。配置文件中会传入不同的有效参数，例如**产品可执行文件名**、**版本号**、**数据库连接信息**、**连接服务器的 IP 地址**等。假设开发人员会将解析文件、设置和测试各个属性参数的功能拆分到单独的函数中。在本例中，我们假设开发人员正在编写这样一个功能：已将 **IP 地址** 解析为 **字符串**，并想判断该 **字符串** 是否为有效的 **IP 地址**。目前，判断 **IP 地址** 有效的标准需满足以下条件：

*   **字符串** 不应为空（`NULL`）。
*   **字符串** 长度不应超过 **16** 个字符。
*   **字符串** 应符合 `XXX.XXX.XXX.XXX` 格式，其中 `X` 必须是 `0`-`9` 之间的数字。

## 实现步骤

### 1. 创建 `CheckIp.h` 头文件

创建 `CheckIp.h` 头文件，并在其中编写以下代码：

```cpp
#ifndef _CHECK_IP_H_
#define _CHECK_IP_H_

#include <iostream>
#include <cstring>

using namespace std;

bool checkValidIp(const char *);

#endif
```

在上述代码中，我们编写了一个名为 `checkValidIp()` 的函数来检查 **IP 地址** 是否有效。同样，为了理解 **Google 单元测试**，我们将编写最少量的代码来理解该特性。

### 2. 创建 `CheckIp.cpp` 文件

创建 `CheckIp.cpp` 文件，并编写以下代码，用于检查 `ip` 是否 **不为 NULL** 且长度小于 **16**：

```cpp
#include "CheckIp.h"
#include <string>
#include <sstream>

bool checkValidIp(const char * ip) {
    if (ip == NULL) {
        cout << "Error : IP passes is NULL " << endl;
        return false;
    }
    if (strlen(ip) > 15) {
        cout << "Error: IP size is greater than 15" << endl;
        return false;
    }
    cout << "strlen: " << strlen(ip) << endl;
    return true;
}
```

在上述代码中，如果两个条件均不满足，函数返回 `false`。

### 3. 创建 `MainIp.cpp` 调用 `checkValidIp()` 函数

创建一个名为 `MainIp.cpp` 的新文件来调用 `checkValidIp()` 函数。该文件通常包含项目的主流程，但为练习目的，我们仅调用 `checkValidIp()` 函数。在其中添加以下代码：

```cpp
#include "CheckIp.h"

int main() {
    const char * ip;
    // ip = "111.111.111.111";
    ip = "111.111.111.11111";
    if (checkValidIp(ip))
        cout << "IP address is correctly validated" << endl;
    else {
        /// work on error condition
        // if needed exit program gracefully.
        cout << " Got error in valid ip " << endl;
        return -1;
    }
    return 0;
}
```

### 4. 创建 `TestCases.cpp` 编写测试用例

为了创建测试代码，我们将创建第一个 `.cpp` 文件 `TestCases.cpp`。它将包含 `checkValidIp` 函数的测试用例。在其中编写以下代码：

```cpp
#include "CheckIp.h"
#include <gtest/gtest.h>

using namespace std;

const char * testIp;

TEST(CheckIp, testNull) {
    testIp = NULL;
    ASSERT_FALSE(checkValidIp(testIp));
}

TEST(CheckIp, BadLength) {
    testIp = "232.13.1231.1321.123";
    ASSERT_FALSE(checkValidIp(testIp));
}
```

在上述代码的第二行，我们包含了 `gtest.h` 文件。我们还使用 `TEST` 函数调用测试用例，该函数接受两个参数：第一个是 **测试套件** 名称，第二个是 **测试用例** 名称。在本例中，我们创建了 **测试套件** `CheckIp`。在 `TEST` 代码块中，你会看到 **Google Test** 定义了一个名为 `ASSERT_FALSE` 的 **断言**，用于检查条件是否为 `false`。如果不为 `false`，测试用例将失败并在结果中显示。

#### 注意

通常，对于 **Google Test** 的测试用例和测试套件，可以将它们归组在一个公共命名空间中，并调用 `RUN_ALL_TESTS` 宏，该宏运行附加到测试二进制文件的所有测试用例。对于每个测试用例，它会调用 `SetUp` 函数进行初始化（类似于类的构造函数），然后调用实际的测试用例，最后调用 `TearDown` 函数（类似于类的析构函数）。除非你需要为测试用例初始化某些内容，否则不必编写 `SetUp` 和 `TearDown` 函数。

### 5. 编译 `CheckIp.cpp` 生成目标文件

要运行测试用例，我们将创建测试用例的主 `.cpp` 文件并调用 `RUN_ALL_TESTS` 宏。或者，我们可以通过链接 **Google Test 库** 来创建一个调用 `RUN_ALL_TESTS` 的可执行文件。在本例中，我们采用后者。打开终端并运行以下命令以创建测试运行二进制文件：

```bash
g++ -c CheckIp.cpp
```

这将包含 `CheckIp.cpp` 的目标文件，因为 `checkValidIp` 函数是在其中定义的。

### 6. 链接库创建测试二进制文件

现在，输入以下命令以添加创建二进制文件所需的链接库：

```bash
g++ CheckIp.o TestCases.cpp -lgtest -lgtest_main -pthread -o TestRun
```

### 7. 运行测试二进制文件

现在，使用以下命令运行二进制文件：

```bash
./TestRun
```

这将显示 `CheckIp` **测试套件** 的两个通过测试用例。第一个测试用例 `CheckIp.testNull` 被调用并通过。第二个测试用例 `CheckIp.BadLength` 也被调用并通过。此结果可见于下图：

![Figure 7.11: Compiling and executing test cases](image/C14583_07_11.jpg)

###### Figure 7.11: Compiling and executing test cases

#### 注意

在 **Google Test** 中，我们也可以使用其他断言，但对于我们的测试用例，`ASSERT_FALSE` 已足够，因为我们仅检查传入的 IP 地址的 `false` 条件。

### 8. 添加更多测试用例以增强代码健壮性

现在，我们将添加更多测试用例以使代码更健壮。这是编写代码的良好实践。首先创建测试用例，确保代码对新旧测试用例均能正确运行，且功能正确。要添加更多测试用例，请将以下代码添加到 `TestCases.cpp` 文件中：

```cpp
TEST(CheckIp, WrongTokenCount) {
    testIp = "22.13111.11";
    ASSERT_FALSE(checkValidIp(testIp));
}

TEST(CheckIp, WrongTokenEmpty) {
    testIp = "22.131..11";
    ASSERT_FALSE(checkValidIp(testIp));
}

TEST(CheckIp, WrongTokenStart) {
    testIp = ".2.1.31.11";
    ASSERT_FALSE(checkValidIp(testIp));
}

TEST(CheckIp, WrongTokenEnd) {
    testIp = "2.13.11.1.";
    ASSERT_FALSE(checkValidIp(testIp));
}

TEST(CheckIp, SpaceToken) {
    testIp = "2.13.11. 1";
    ASSERT_FALSE(checkValidIp(testIp));
}

TEST(CheckIp, NonDigit) {
    testIp = "2.13.b1.A1";
    ASSERT_FALSE(checkValidIp(testIp));
}

TEST(CheckIp, NonValidDigit) {
    testIp = "2.13.521.61";
    ASSERT_FALSE(checkValidIp(testIp));
}

TEST(CheckIp, CorrectIp) {
    testIp = "232.13.123.1";
    ASSERT_FALSE(checkValidIp(testIp));
}
```

在上述代码中：
- 前两个用例应因令牌数量不正确而失败。
- 第三个用例应因 **IP** 以 `.` 开头而失败。
- 第四个用例应因 **IP** 以 `.` 结尾而失败。
- 第五个用例应因 **IP** 中间包含空格而失败。
- 第六个用例应因 **IP** 包含非数字字符而失败。
- 第七个用例应因 **IP** 令牌值小于 `0` 或大于 `255` 而失败。
- 最后一个用例应因 **IP** 令牌数量错误而失败。

### 9. 更新 `CheckIp.cpp` 中的 `checkValidIp()` 函数

现在，在 `CheckIp.cpp` 文件的 `checkValidIp()` 函数中添加以下代码，以处理新增的测试用例：

