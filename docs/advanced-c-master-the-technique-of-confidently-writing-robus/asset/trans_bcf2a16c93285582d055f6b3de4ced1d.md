

## 智能指针与函数调用

执行时，它输出以下内容：

![图 3.25：weak_ptr 程序输出](`image/C14583_03_25.jpg`)

*图 3.25：weak_ptr 程序输出*

这表明当应用程序退出时，面板和 `textwindow` 并未被销毁。这是因为它们都持有指向对方的 `shared_ptr`，因此两者的引用计数都不会降为零，从而不会触发析构。如果我们用图表描绘该结构，可以看到它存在一个 `shared_ptr` 循环：

![图 3.26：weak_ptr 与 shared_ptr 循环](`image/C14583_03_26.jpg`)

*图 3.26：weak_ptr 与 shared_ptr 循环*

既然我们可以管理资源了，该如何使用它们？我们要到处传递智能指针吗？当我们拥有一个智能指针（`unique_ptr` 或 `shared_ptr`）时，调用函数有四种选择：

*   通过值传递智能指针
*   通过引用传递智能指针
*   通过指针传递被管理的资源
*   通过引用传递被管理的资源

这并非详尽列表，但却是主要考虑的选项。如何传递智能指针或其资源，取决于我们调用函数的**意图**：

*   函数的意图仅仅是**使用资源**吗？
*   函数是否**获取资源的所有权**？
*   函数是否**替换被管理的对象**？

如果函数仅仅是要**使用资源**，那么它甚至不需要知道自己被传递的是一个被管理的资源。它只需使用该资源，因此应通过资源指针、资源引用（甚至资源值）来调用：

```
do_something(Resource* resource);
do_something(Resource& resource);
do_something(Resource resource);
```

如果你想将资源的**所有权传递**给函数，则函数应通过值接收智能指针，并使用 `std::move()` 进行调用：

```
do_something(std::unique_ptr<Resource> resource);
auto res = std::make_unique<Resource>();
do_something(std::move(res));
```

当 `do_something()` 返回时，`res` 变量将为空，资源的所有权现归 `do_something()` 所有。

如果你想**替换被管理的对象**（这一过程称为**重置/重绑定 reseating**），则通过引用传递智能指针：

```
do_something(std::unique_ptr<Resource>& resource);
```

下面的程序将所有这些场景整合在一起，演示了每种情况及如何调用函数：

```cpp
#include <iostream>
#include <memory>
#include <string>
#include <sstream>

class Resource
{
public:
    Resource() { std::cout << "+++Resource acquired [" << m_id << "]\n"; }
    ~Resource() { std::cout << "---Resource released [" << m_id << "]\n"; }
    std::string name() const {
        std::ostringstream ss;
        ss << "the resource [" << m_id << "]";
        return ss.str();
    }
    int m_id{++m_count};
    static int m_count;
};

int Resource::m_count{0};

void use_resource(Resource& res)
{
    std::cout << "Enter use_resource\n";
    std::cout << "...using " << res.name() << "\n";
    std::cout << "Exit use_resource\n";
}

void take_ownership(std::unique_ptr<Resource> res)
{
    std::cout << "Enter take_ownership\n";
    if (res)
        std::cout << "...taken " << res->name() << "\n";
    std::cout << "Exit take_ownership\n";
}

void reseat(std::unique_ptr<Resource>& res)
{
    std::cout << "Enter reseat\n";
    res.reset(new Resource);
    if (res)
        std::cout << "...reseated " << res->name() << "\n";
    std::cout << "Exit reseat\n";
}

int main()
{
    std::cout << "Starting...\n";
    auto res = std::make_unique<Resource>();
    // Use - pass resource by reference
    use_resource(*res);
    if (res)
        std::cout << "We HAVE the resource " << res->name() << "\n\n";
    else
        std::cout << "We have LOST the resource\n\n";
    // Pass ownership - pass smart pointer by value
    take_ownership(std::move(res));
    if (res)
        std::cout << "We HAVE the resource " << res->name() << "\n\n";
    else
        std::cout << "We have LOST the resource\n\n";
    // Replace (reseat) resource - pass smart pointer by reference
    reseat(res);
    if (res)
        std::cout << "We HAVE the resource " << res->name() << "\n\n";
    else
        std::cout << "We have LOST the resource\n\n";
    std::cout << "Exiting...\n";
    return 0;
}
```

运行此程序时，我们得到以下输出：

![图 3.27：所有权传递程序输出](`image/C14583_03_27.jpg`)

*图 3.27：所有权传递程序输出*

### 注意

*C++ 核心指南* 专门有一个章节讲解*资源管理*、智能指针及其用法，详见：<http://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines#S-resource>。我们仅涉及了指南中最重要的几个方面。

## 练习 4：使用 STL 智能指针实现 RAII

在本练习中，我们将实现一个传感器工厂方法，该方法通过 `unique_ptr` 返回传感器资源。我们将实现一个持有数组的 `unique_ptr`，然后开发代码将 `unique_ptr` 转换为共享指针并进一步共享它。请遵循以下步骤实现此练习：

1.  在 Eclipse 中打开 `Lesson3` 项目。然后在 `Project Explorer` 中展开 `Lesson3`，再展开 `Exercise04`，双击 `Exercise4.cpp` 在编辑器中打开该练习文件。
2.  点击 `Launch Configuration` 下拉菜单并选择 `New Launch Configuration…`。从 `Search Project` 菜单配置 `L3Exercise4` 应用程序，使其以名称 `L3Exercise4` 运行。
3.  点击 `Run` 按钮运行练习 4。这将产生以下输出：

    ![图 3.28：练习 4 输出](`image/C14583_03_28.jpg`)

    *图 3.28：练习 4 输出*

4.  在编辑器中检查代码，特别是工厂方法，即 `createSensor(type)`：

    ```cpp
    std::unique_ptr<ISensor> createSensor(SensorType type)
    {
        std::unique_ptr<ISensor> sensor;
        if (type == SensorType::Light)
        {
            sensor.reset(new LightSensor);
        }
        else if (type == SensorType::Temperature)
        {
            sensor.reset(new TemperatureSensor);
        }
        else if (type == SensorType::Pressure)
        {
            sensor.reset(new PressureSensor);
        }
        return sensor;
    }
    ```

    这创建了一个名为 `sensor` 的空 unique_ptr，然后根据传入的 `type` 重置其内部指针为所需的传感器。

5.  在编辑器中打开 `Exercise4.cpp` 并将文件顶部附近的行修改为如下内容：

    ```cpp
    #define EXERCISE4_STEP  5
    ```

6.  点击 `Run` 按钮编译代码，编译将失败并报出以下错误：

    ![图 3.29：步骤 5 的编译器错误](`image/C14583_03_29.jpg`)

    *图 3.29：步骤 5 的编译器错误*

    完整错误信息如下：

    > error: conversion from 'std::unique_ptr<ISensor>' to non-scalar type 'SensorSPtr {aka std::shared_ptr<ISensor>}' requested

    根据错误提示，我们试图将 `unique_ptr` 赋值给 `shared_ptr`，这是不允许的。

7.  找到报错的那一行并将其修改为如下内容：

    ```cpp
    SensorSPtr light2 = std::move(light);
    ```

8.  点击 `Run` 按钮编译并运行程序。输出如下：

    ![图 3.30：练习 4 成功输出（EXERCISE4_STEP = 5 后）](`image/C14583_03_30.jpg`)

    *图 3.30：练习 4 成功输出（EXERCISE4_STEP = 5 后）*

