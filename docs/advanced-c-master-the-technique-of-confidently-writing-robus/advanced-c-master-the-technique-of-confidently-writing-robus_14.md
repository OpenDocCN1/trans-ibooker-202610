## 第 4 章 - 关注点分离 - 软件架构、函数、可变参数模板

## 活动 1：实现多播事件处理器

1. 从 `Lesson4/Activity01` 文件夹加载准备好的项目，并将项目的当前构建器配置为 CMake Build (Portable)。构建项目，配置启动器并运行单元测试（唯一的虚拟测试将失败）。建议测试运行器使用的名称为 `L4delegateTests`。

2. 在 `delegateTests.cpp` 中，用以下测试替换失败的虚拟测试：

```cpp
TEST_F(DelegateTest, BasicDelegate)
{
    Delegate delegate;
    ASSERT_NO_THROW(delegate.Notify(42));
}
```

3. 现在构建失败，因此我们需要向 `Delegate` 添加一个新方法。由于这将演变为模板，我们将在头文件中完成所有开发工作。在 `delegate.hpp` 中添加以下定义：

```cpp
class Delegate
{
public:
    Delegate() = default;
    void Notify(int value) const
    {
    }
};
```

测试现在运行并通过。

4. 向现有测试添加以下行：

```cpp
ASSERT_NO_THROW(delegate(22));
```

5. 构建再次失败，因此我们按如下方式更新 `Delegate` 定义（我们本可以让 `Notify` 调用 `operator()`，但这样更易读）：

```cpp
void operator()(int value)
{
    Notify(value);
}
```

测试再次运行并通过。

6. 在添加下一个测试之前，我们将添加一些基础设施来帮助开发测试。处理程序最简单的做法是写入 `std::cout`，为了验证它们是否被调用，我们需要捕获输出。为此，通过更改 `DelegateTest` 类将标准输出流重定向到不同的缓冲区，如下所示：

```cpp
class DelegateTest : public ::testing::Test
{
public:
    void SetUp() override;
```




## 使用 Google Test 实现委托类

## 1. 测试固件设置（步骤 7–8）

测试固件 `DelegateTest` 保存并恢复 `std::cout` 的缓冲区。

```cpp
void TearDown() override;

std::stringstream m_buffer;

// Save cout's buffer here
std::streambuf *m_savedBuf{};
};

void DelegateTest::SetUp()
{
    // Save the cout buffer
    m_savedBuf = std::cout.rdbuf();
    // Redirect cout to our buffer
    std::cout.rdbuf(m_buffer.rdbuf());
}

void DelegateTest::TearDown()
{
    // Restore cout buffer to original
    std::cout.rdbuf(m_savedBuf);
}
```

7. 此外，在文件顶部添加 `<iostream>`、`<sstream>` 和 `<string>` 的 include 语句。

8. 有了这个支持框架，添加以下测试：

```cpp
TEST_F(DelegateTest, SingleCallback)
{
    Delegate delegate;
    delegate += [] (int value) { std::cout << "value = " << value; };
    delegate.Notify(42);
    std::string result = m_buffer.str();
    ASSERT_STREQ("value = 42", result.c_str());
}
```

## 2. 第一个测试和基本委托实现（步骤 9–11）

9. 为了让测试再次构建并运行，在 `delegate.h` 类中添加以下代码：

```cpp
Delegate& operator+=(const std::function<void(int)>& delegate)
{
    m_delegate = delegate;
    return *this;
}
```

以及以下代码：

```cpp
private:
    std::function<void(int)> m_delegate;
```

测试现在可以构建了，但我们的新测试失败了。

10. 将 `Notify()` 方法更新为：

```cpp
void Notify(int value) const
{
    m_delegate(value);
}
```

11. 测试现在可以构建且新测试通过了，但原来的测试现在失败了。调用委托时抛出了异常，所以我们需要在调用前检查委托是否为空。编写以下代码来实现这一点：

```cpp
void Notify(int value) const
{
    if(m_delegate)
        m_delegate(value);
}
```

现在所有测试都能运行并通过。

## 3. 添加多播支持（步骤 12–13）

12. 我们现在需要为 `Delegate` 类添加多播支持。添加新测试：

```cpp
TEST_F(DelegateTest, DualCallbacks)
{
    Delegate delegate;
    delegate += [] (int value) { std::cout << "1: = " << value << "\n"; };
    delegate += [] (int value) { std::cout << "2: = " << value << "\n"; };
    delegate.Notify(12);
    std::string result = m_buffer.str();
    ASSERT_STREQ("1: = 12\n2: = 12\n", result.c_str());
}
```

13. 当然，这个测试现在会失败，因为 `operator+=()` 只是赋值给成员变量。我们需要添加一个列表来存储我们的委托。我们选择 `vector`，以便我们可以向列表末尾添加元素，因为我们希望按照添加顺序调用委托。在 `delegate.hpp` 顶部添加 `#include <vector>`，并更新 `Delegate`，用回调的 `m_delegates` vector 替换 `m_delegate`：

```cpp
class Delegate
{
public:
    Delegate() = default;
    Delegate& operator+=(const std::function<void(int)>& delegate)
    {
        m_delegates.push_back(delegate);
        return *this;
    }
    void Notify(int value) const
    {
        for(auto& delegate : m_delegates)
        {
            delegate(value);
        }
    }
    void operator()(int value)
    {
        Notify(value);
    }
private:
    std::vector<std::function<void(int)>> m_delegates;
};
```

测试再次全部运行并通过。

## 4. 转换为模板类（步骤 14–18）

14. 我们现在已经实现了基本的多播 `delegate` 类。现在需要将其转换为基于模板的类。更新现有测试，将三个测试中所有 `Delegate` 的声明改为 `Delegate<int>`。

15. 现在通过在类前添加 `template<class Arg>` 来更新 `Delegate` 类以将其转换为模板，并将四处 `int` 替换为 `Arg`：

```cpp
template<class Arg>
class Delegate
{
public:
    Delegate() = default;
    Delegate& operator+=(const std::function<void(Arg)>& delegate)
    {
        m_delegates.push_back(delegate);
        return *this;
    }
    void Notify(Arg value) const
    {
        for(auto& delegate : m_delegates)
        {
            delegate(value);
        }
    }
    void operator()(Arg value)
    {
        Notify(value);
    }
private:
    std::vector<std::function<void(Arg)>> m_delegates;
};
```

16. 现在所有测试都能像以前一样运行并通过，所以它仍然适用于处理程序的 `int` 参数。

17. 添加以下测试并重新运行测试以确认模板转换是正确的：

```cpp
TEST_F(DelegateTest, DualCallbacksString)
{
    Delegate<std::string&> delegate;
    delegate += [] (std::string value) { std::cout << "1: = " << value << "\n"; };
    delegate += [] (std::string value) { std::cout << "2: = " << value << "\n"; };
    std::string hi{"hi"};
    delegate.Notify(hi);
    std::string result = m_buffer.str();
    ASSERT_STREQ("1: = hi\n2: = hi\n", result.c_str());
}
```

18. 现在它作为一个接受一个参数的模板运行。我们需要将其转换为接受零个或多个参数的可变参数模板。使用上一主题的信息，将模板更新为以下内容：

```cpp
template<typename... ArgTypes>
class Delegate
{
public:
    Delegate() = default;
    Delegate& operator+=(const std::function<void(ArgTypes...)>& delegate)
    {
        m_delegates.push_back(delegate);
        return *this;
    }
    void Notify(ArgTypes&&... args) const
    {
        for(auto& delegate : m_delegates)
        {
            delegate(std::forward<ArgTypes>(args)...);
        }
    }
    void operator()(ArgTypes&&... args)
    {
        Notify(std::forward<ArgTypes>(args)...);
    }
private:
    std::vector<std::function<void(ArgTypes...)>> m_delegates;
};
```

测试应该仍然能运行并通过。

## 5. 可变参数模板和最终测试（步骤 19–20）

19. 再添加两个测试——零参数测试和多参数测试：

```cpp
TEST_F(DelegateTest, DualCallbacksNoArgs)
{
    Delegate delegate;
    delegate += [] () { std::cout << "CB1\n"; };
    delegate += [] () { std::cout << "CB2\n"; };
    delegate.Notify();
    std::string result = m_buffer.str();
    ASSERT_STREQ("CB1\nCB2\n", result.c_str());
}

TEST_F(DelegateTest, DualCallbacksStringAndInt)
{
    Delegate<std::string&, int> delegate;
    delegate += [] (std::string& value, int i) {
        std::cout << "1: = " << value << "," << i << "\n"; };
    delegate += [] (std::string& value, int i) {
        std::cout << "2: = " << value << "," << i << "\n"; };
    std::string hi{"hi"};
    delegate.Notify(hi, 52);
    std::string result = m_buffer.str();
    ASSERT_STREQ("1: = hi,52\n2: = hi,52\n", result.c_str());
}
```

所有测试运行并通过，表明我们现在已经实现了所需的 `Delegate` 类。

20. 现在，更改运行配置以执行程序 `L4delegate`。在编辑器中打开 `main.cpp` 文件，将文件顶部的定义更改为以下内容并运行程序：

```cpp
#define ACTIVITY_STEP 27
```

我们得到以下输出：

![图 4.35：Delegate 成功实现的输出](img/C14583_04_35.jpg)

###### 图 4.35：Delegate 成功实现的输出



在本活动中，我们首先实现了一个提供基本单播委托功能的类，然后添加了多播能力。完成这些实现并配备单元测试后，我们能够迅速将其转换为带有一个参数的模板，进而转换为可变参数模板版本。根据你正在开发的功能，从具体实现过渡到通用形式，再过渡到更通用的形式，这种方法是正确的。可变参数模板的开发并不总是显而易见的。
