## 第 4 章 - 关注点分离 - 软件架构、函数、可变参数模板

### 活动 1：实现一个多播事件处理器

- 1. 从 **Lesson4/Activity01** 文件夹加载准备好的项目，并将项目的 Current Builder 配置为 `CMake Build (Portable)`。构建项目，配置启动器并运行单元测试（这些测试会失败一个占位测试）。建议测试运行器的名称使用 `L4delegateTests`。
- 2. 在 **delegateTests.cpp** 中，将失败的占位测试替换为以下测试：

```
TEST_F(DelegateTest, BasicDelegate)
{
    Delegate delegate;
    ASSERT_NO_THROW(delegate.Notify(42));
}
```

- 3. 现在构建会失败，所以我们需要向 **Delegate** 添加一个新方法。由于这将会演变成一个模板，我们将在头文件中完成所有开发。在 **delegate.hpp** 中，添加以下定义：

```
class Delegate
{
public:
    Delegate() = default;
    void Notify(int value) const
    {
    }
};
```

测试现在可以运行并通过了。

- 4. 在现有测试中添加以下行：

```
ASSERT_NO_THROW(delegate(22));
```





- 5. 再次构建失败，因此我们按如下方式更新 `Delegate` 定义（我们本可以让 `Notify` 调用 `operator()`，但这样更易读）：

```cpp
void operator()(int value)
{
    Notify(value);
}
```

测试再次运行并通过。

- 6. 在添加下一个测试之前，我们将添加一些基础设施来帮助我们开发测试。对处理程序最简单的做法是让它们写入 `std::cout`，并且为了验证它们是否被调用，我们需要捕获输出。为此，通过按如下方式更改 `DelegateTest` 类，将标准输出流重定向到不同的缓冲区：

```cpp
class DelegateTest : public ::testing::Test
{
public:
    void SetUp() override;
    void TearDown() override;

    std::stringstream m_buffer;

    // Save cout's buffer here
    std::streambuf *m_savedBuf{};
};
```

```cpp
void DelegateTest::SetUp()
{
    // Save the cout buffer
    m_savedBuf = std::cout.rdbuf();
    // Redirect cout to our buffer
    std::cout.rdbuf(m_buffer.rdbuf());
}
```

```cpp
void DelegateTest::TearDown()
{
    // Restore cout buffer to original
    std::cout.rdbuf(m_savedBuf);
}
```

- 7. 还将 `<iostream>`、`<sstream>` 和 `<string>` 的包含语句添加到文件顶部。

```cpp
std::vector<std::function<void(int)>> m_delegates;
};
```

测试再次全部运行并通过。

- 14. 我们现在已经实现了基本的 multicast delegate 类。现在需要将其转换为基于模板的类。通过在三个测试中将所有 `Delegate` 的声明更改为 `Delegate<int>` 来更新现有测试。

- 15. 现在通过在类前添加 `template<class Arg>` 将 `Delegate` 类更新为模板，并用 `Arg` 替换四个出现的 `int`：

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

- 16. 所有测试现在都像以前一样运行并通过，因此它仍然适用于处理程序的 `int` 参数。

- 17. 添加以下测试并重新运行测试以确认模板转换是否正确：

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

- 18. 现在它作为接受一个参数的模板运行。我们需要将其转换为接受零个或多个参数的可变参数模板。使用上一个主题中的信息，将模板更新为如下所示：

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

    void Notify(ArgTypes&... args) const
    {
        for(auto& delegate : m_delegates)
        {
            delegate(std::forward<ArgTypes>(args)...);
        }
    }

    void operator()(ArgTypes&... args)
    {
        Notify(std::forward<ArgTypes>(args)...);
    }
private:
    std::vector<std::function<void(ArgTypes...)>> m_delegates;
};
```

- 19. 添加另外两个测试——零参数测试和多参数测试：

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
```

```cpp
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

所有测试都运行并通过，表明我们现在已经实现了所需的 `Delegate` 类。

- 20. 现在，将运行配置更改为执行程序 `L4delegate`。在编辑器中打开 `main.cpp` 文件并将文件顶部的定义更改为如下所示，然后运行程序：

```cpp
#define ACTIVITY_STEP 27
```

我们得到以下输出：

```text
------ Activity 1 ------

------ Delegate - no args ------
Hello, World
------ Delegate - string argument ------
hello!-hello?-hello@
------ Delegate - binary int argument ------
52 + 4 = 56
52 - 4 = 48
52 * 4 = 208
52 / 4 = 13

Complete.
```

*Figure 4.35: Output from the successful implementation of Delegate*

在本活动中，我们首先实现了一个提供基本单委托功能的类，然后添加了多播功能。在实现并建立单元测试后，我们能够快速将其转换为带有一个参数的模板，然后转换为可变参数模板版本。根据您正在开发的功能，具体实现从特定形式过渡到更一般形式，然后过渡到更一般形式的方法是正确的方法。可变参数模板的开发并不总是显而易见的。
