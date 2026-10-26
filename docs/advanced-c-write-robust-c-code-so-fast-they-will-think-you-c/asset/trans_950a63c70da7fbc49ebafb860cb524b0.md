

在前面的截图中，断言抛出错误，因为传入的 **ip** 长度大于 **16**。

- 8. 现在，为了满足 **assert** 条件使二进制程序正常运行，我们需要在 **main** 函数中更新 **ip** 的值。编写以下代码来实现此操作：

```
ip = "111.111.111.111";
```

再次打开编译器，编译 **.cpp** 文件，并运行生成的二进制文件。编译器中显示以下输出：

![](img/6507d435e2edfa3c00c220d0ccfcaf94_504_0.png)

图 7.3：在命令提示符上运行断言二进制文件

**注意：** 由于我们只是处理 **assert**，因此我们没有向我们的 **checkValidIP()** 函数添加任何额外功能。但是，我们将在 *异常处理* 和 *单元测试* 部分使用相同的示例，在其中我们将为我们的函数添加更多功能。

- 9. 如果我们不希望在生产或发布环境中由于断言而导致可执行文件中止，请从代码中删除 **assert** 宏调用。首先，我们将更新长度大于 **16** 的 **ip** 值。向文件添加以下代码：

```
ip = "111.111.111.11111";
```

- 10. 现在，在编译期间，传递 **-DNDEBUG** 宏。这将确保断言不会在二进制文件中被调用。在终端中编写以下命令来编译我们的 **.cpp** 文件：

```
g++ -DNDEBUG AssertSample.cpp
```

之后，当我们执行二进制文件时，会生成以下输出：

![](img/6507d435e2edfa3c00c220d0ccfcaf94_505_0.png)

**图 7.4：在命令提示符上运行断言二进制文件**

在前面的截图中，由于未调用 **assert**，它将显示字符串长度为 **17**，并且 **true** 值将验证 IP 地址。在本练习中，我们看到在执行二进制文件时调用了断言。我们也可以在代码编译期间进行断言。这是在 C++11 中引入的。它被称为 **静态断言**，我们将在下一节中探索它。

## 静态断言

有时，我们可以在编译时进行条件检查以避免任何未来的错误。例如，在一个项目中，我们可能会使用一个第三方库，其中声明了一些数据结构。我们可以从其头文件中得知该结构信息，例如其大小和 **成员变量**。利用这些信息，我们可以正确地分配或释放内存，以及操作其成员变量。该结构属性可能会在第三方库的不同版本中发生变化。但是，如果我们的项目代码仍在使用早期版本的结构，那么在使用它时就会出现问题。我们可能在运行二进制文件时在后期阶段遇到错误。我们可以使用 **静态断言** 在编译时本身捕获此错误。我们可以比较静态数据，例如库的版本号，从而确保我们的代码不会遇到任何问题。在下一节中，我们将基于此解决一个练习。

### 练习 2：测试静态断言

在本练习中，我们将通过进行静态断言来比较两个头文件的版本号。如果版本号小于 1，则会抛出静态断言错误。执行以下步骤来实现此练习：

- 1. 创建一个名为 PersonLibrary_ver1.h 的头文件并添加以下代码：

```
#ifndef __PERSON_H__
#define __PERSON_H__

#include<string>

using std::string;

#define PERSON_LIB_VERSION 1

struct person{
    string name;
    int age;
    string address;
};

#endif
```

在前面的代码中，struct person 被定义并包含以下属性：name、age 和 address。它还具有版本号 1。

- 2. 创建另一个名为 PersonLibrary_ver2.h 的头文件并添加以下代码：

```
#ifndef __PERSON_H__
#define __PERSON_H__

#include<string>

using std::string;

#define PERSON_LIB_VERSION 2

struct person{
    string name;
    int age;
    string address;
    string Mobile_No;
};

#endif
```

在前面的代码中，struct person 被定义并包含以下属性：name、age、address 和 Mobile_No。它还具有版本号 2。现在，版本 1 是旧版本，版本 2 是新版本。以下是两个头文件并排显示的截图：

![](img/6507d435e2edfa3c00c220d0ccfcaf94_507_0.png)

图 7.5：具有不同版本的库文件

- 3. 创建一个名为 StaticAssertionSample.cpp 的文件并添加以下代码：

```
#include<iostream>
#include"PersonLibrary.h"

void doSanityCheck(){
    static_assert(PERSON_LIB_VERSION > 1 , "PERSON LIBRARY VERSION not greater than 1");
    // Do any more sanity check before starting app ...
}

int main(){
    doSanityCheck();
    return 0;
}
```

在前面的代码中，我们在构建和执行项目之前对其进行健全性检查。我们创建了一个名为 doSanityCheck() 的函数来执行库的版本检查。它使用静态断言完成，并在编译时执行。代码的第二行显示包含 PersonLibrary.h 文件。在 doSanityCheck() 函数内部，static_assert() 函数检查此库版本是否大于 1。

**注意：** 如果你的项目需要在版本 2 或更高版本的库中定义 person 结构体才能正确执行它，我们需要匹配版本 2 的文件，即 PERSON_LIB_VERSION 至少应设置为 2。如果开发人员获取了库的版本 1 并尝试为项目创建二进制文件，它可能会在执行时造成问题。为了避免这种情况，在项目的 main 代码中，我们在构建和执行之前对项目进行健全性检查。

- 4. 要在我们的 .cpp 文件中包含库的版本 1，请打开终端并编写以下命令：

```
ln -s PersonLibrary_ver1.h PersonLibrary.h
```

前面的命令将创建一个名为 PersonLibrary.h 的 PersonLibrary_ver1.h 文件的软链接。这就像模拟使用版本 1 的 PersonLibrary.h 的环境。

- 5. 使用终端中的以下命令编译我们的 .cpp 文件：

```
g++ StaticAssertionSample.cpp
```

以下是终端中生成的输出：

![](img/6507d435e2edfa3c00c220d0ccfcaf94_508_0.png)

图 7.6：看到静态错误

在前面的截图中，有三个区域用红色圈出。第一个给出创建软链接的命令。第二个命令显示我们创建的 **PersonLibrary.h** 文件。第三个区域显示由于库版本不匹配而抛出的 **static_assert** 错误。

- 6. 现在，为了正确编译程序，请删除 **PersonLibrary** 的软链接并创建一个新的软链接指向 **版本 2**，然后再次编译。这次它将编译正常。在终端中输入以下命令以删除软链接：

```
rm PersonLibrary.h
ln -s PersonLibrary_ver2.h PersonLibrary.h
g++ StaticAssertionSample.cpp
```

以下是相同的截图：

![](img/6507d435e2edfa3c00c220d0ccfcaf94_509_0.png)

图 7.7：静态断言编译文件

正如你所见，一个用红色标记的区域显示使用了正确的 **PersonLibrary** 版本，编译顺利进行，没有任何问题。编译后，会创建一个名为 "**a.exe**" 的二进制文件。在本练习中，我们通过比较两个头文件的版本号执行了静态断言。在下一节中，我们将探索异常处理的概念。

## 理解异常处理

正如我们在调试模式二进制文件中看到的，我们可以使用运行时断言在某些条件不满足时中止程序。但在发布模式二进制文件或生产环境中，当客户端使用此产品时，突然中止程序并不是一个好主意。最好处理此类错误条件并继续执行二进制文件的下一部分。

