## 第 2B 章 – 禁止鸭子类型：模板与推导

## 活动 1：开发通用的 "contains" 模板函数

在本活动中，我们将实现几个辅助类，用于检测 `std::string` 类情况和 `std::set` 情况，然后使用它们来针对特定容器定制 `contains` 函数。请按以下步骤实现此活动：

### 步骤 1
从 `Lesson2B/Activity01` 文件夹加载准备好的项目。构建并配置启动器，运行单元测试（会失败一个虚拟测试）。建议测试运行器使用的名称为 `L2BA1tests`。

### 步骤 2
打开 `containsTests.cpp` 文件，将现有测试替换为以下内容：

```cpp
TEST_F(containsTest, DetectNpos)
{
    ASSERT_TRUE(has_npos_v<std::string>);
    ASSERT_FALSE(has_npos_v<std::set<int>>);
    ASSERT_FALSE(has_npos_v<std::vector<int>>);
}
```

此测试要求我们编写一组辅助模板，以检测容器类是否支持名为 `npos` 的静态成员变量。

### 步骤 3
将以下代码添加到 `contains.hpp` 文件：

```cpp
template <class T>
auto test_npos(int) -> decltype((void)T::npos, std::true_type{});

template <class T>
auto test_npos(long) -> std::false_type;

template <class T>
struct has_npos : decltype(test_npos<T>(0)) {};

template <class T>
inline constexpr bool has_npos_v = has_npos<T>::value;
```

测试现在运行并通过。

### 步骤 4
将以下测试添加到 `containsTest.cpp` 文件：

```cpp
TEST_F(containsTest, DetectFind)
{
    ASSERT_TRUE((has_find_v<std::string, char>));
    ASSERT_TRUE((has_find_v<std::set<int>, int>));
    ASSERT_FALSE((has_find_v<std::vector<int>, int>));
}
```

此测试要求我们编写一组辅助模板，以检测容器类是否具有接受一个参数的 `find()` 方法。

### 步骤 5
将以下代码添加到 `contains.hpp` 文件：

```cpp
template <class T, class A0>
auto test_find(int) ->
    decltype(void(std::declval<T>().find(std::declval<A0>())),
             std::true_type{});

template <class T, class A0>
auto test_find(long) -> std::false_type;

template <class T, class A0>
struct has_find : decltype(test_find<T, A0>(0)) {};

template <class T, class A0>
inline constexpr bool has_find_v = has_find<T, A0>::value;
```

测试现在运行并通过。

### 步骤 6
为通用容器（即 `vector`）添加实现。在 `containsTest.cpp` 文件中编写以下测试：

```cpp
TEST_F(containsTest, VectorContains)
{
    std::vector<int> container {1, 2, 3, 4, 5};
    ASSERT_TRUE(contains(container, 5));
    ASSERT_FALSE(contains(container, 15));
}
```

### 步骤 7
将 `contains` 的基本实现添加到 `contains.hpp` 文件：

```cpp
template <class C, class T>
auto contains(const C& c, const T& key) -> decltype(std::end(c), true)
{
    return std::end(c) != std::find(begin(c), end(c), key);
}
```

测试现在运行并通过。

### 步骤 8
下一步是将 `set` 特殊情况的测试添加到 `containsTest.cpp`：

```cpp
TEST_F(containsTest, SetContains)
{
    std::set<int> container {1, 2, 3, 4, 5};
    ASSERT_TRUE(contains(container, 5));
    ASSERT_FALSE(contains(container, 15));
}
```

### 步骤 9
更新 `contains` 的实现以测试内置的 `set::find()` 方法：

```cpp
template <class C, class T>
auto contains(const C& c, const T& key) -> decltype(std::end(c), true)
{
    if constexpr (has_find_v<C, T>)
    {
        return std::end(c) != c.find(key);
    }
    else
    {
        return std::end(c) != std::find(begin(c), end(c), key);
    }
}
```

测试现在运行并通过。

### 步骤 10
将 `string` 特殊情况的测试添加到 `containsTest.cpp` 文件：

```cpp
TEST_F(containsTest, StringContains)
{
    std::string container{"This is the message"};
    ASSERT_TRUE(contains(container, "the"));
    ASSERT_TRUE(contains(container, 'm'));
    ASSERT_FALSE(contains(container, "massage"));
    ASSERT_FALSE(contains(container, 'z'));
}
```

### 步骤 11
添加以下 `contains` 实现以测试 `npos` 的存在并定制 `find()` 方法的使用：

```cpp
template <class C, class T>
auto contains(const C& c, const T& key) -> decltype(std::end(c), true)
{
    if constexpr (has_npos_v<C>)
    {
        return C::npos != c.find(key);
    }
    else if constexpr (has_find_v<C, T>)
    {
        return std::end(c) != c.find(key);
    }
    else
    {
        return std::end(c) != std::find(begin(c), end(c), key);
    }
}
```

测试现在运行并通过。

### 步骤 12
构建并运行名为 `contains` 的应用程序。创建一个新的 Run Configuration。如果 `contains` 模板的实现正确，程序将显示以下输出：

![图 2B.36：contains 成功实现的输出](img/C14583_02B_36.jpg)

在本活动中，我们结合 SFINAE 使用了各种模板技术，根据容器类的能力选择 `contains()` 函数的适当实现。我们本可以使用通用模板函数和一些特化模板达到相同结果，但我们选择了少有人走的路，展示了我们新发现的模板技能。

---
