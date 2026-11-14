

# 右值引用与移动语义

**右值引用**（用双与号 `&&` 表示）是一种仅绑定到 **右值** 的引用，它会延长该右值的生命周期，直到 **右值引用** 本身结束使用为止。因此，**右值** 可以存活于定义它的表达式之外。有了 **右值引用**，我们现在可以通过移动构造函数和移动赋值运算符来实现移动语义。移动语义的目的是从被引用对象“窃取”资源，从而避免昂贵的拷贝操作。移动完成后，被引用对象必须处于稳定状态。换句话说，被移动的对象必须保持在一种状态下，使其在销毁时不会导致任何未定义行为或程序崩溃，也不应影响已被窃取的资源。

C++11 还引入了一个转型运算符 `std::move()`，它将 **左值** 转换为 **右值**，以便调用移动构造函数或移动赋值运算符来“移动”资源。`std::move()` 方法本身并不实际移动数据。

值得注意的是，在移动构造函数和移动赋值运算符内部，**右值** 引用实际上是一个 **左值**。这意味着，如果你想确保在方法内部发生移动语义，可能需要再次对成员变量使用 `std::move()`。

随着 C++11 引入移动语义，标准库也随之更新以利用这一新能力。例如，`std::string` 和 `std::vector` 已更新以包含移动语义。要获得移动语义的好处，只需使用最新的 C++ 编译器重新编译代码即可。

## 实现智能指针

智能指针是一种资源管理类，它持有指向资源的指针，并在超出作用域时释放该资源。在本节中，我们将实现一个智能指针，观察其作为支持拷贝的类的行为，使其演进以支持移动语义，最后移除其对拷贝操作的支持：

```cpp
#include <iostream>

template<class T>
class smart_ptr
{
public:
    smart_ptr(T* ptr = nullptr) : m_ptr(ptr)
    {
    }
    ~smart_ptr()
    {
        delete m_ptr;
    }
    // 拷贝构造函数 --> 执行深拷贝
    smart_ptr(const smart_ptr& a)
    {
        m_ptr = new T;
        *m_ptr = *a.m_ptr;        // 使用 operator=() 执行深拷贝
    }
    // 拷贝赋值 --> 执行深拷贝
    smart_ptr& operator=(const smart_ptr& a)
    {
        // 自赋值检测
        if (&a == this)
            return *this;
        // 释放当前持有的任何资源
        delete m_ptr;
        // 拷贝资源
        m_ptr = new T;
        *m_ptr = *a.m_ptr;
        return *this;
    }
    T& operator*() const { return *m_ptr; }
    T* operator->() const { return m_ptr; }
    bool is_null() const { return m_ptr == nullptr; }

private:
    T* m_ptr{nullptr};
};

class Resource
{
public:
    Resource() { std::cout << "Resource acquired\n"; }
    ~Resource() { std::cout << "Resource released\n"; }
};

smart_ptr<Resource> createResource()
{
    smart_ptr<Resource> res(new Resource);                       // 步骤 1
    return res; // 返回值调用拷贝构造函数                       // 步骤 2
}

int main()
{
    smart_ptr<Resource> the_res;
    the_res = createResource(); // 赋值调用拷贝赋值 步骤 3/4
    return 0; // 步骤 5
}
```

运行该程序时，将生成以下输出：

![Figure 3.23: Smart Pointer Program output](image/C14583_03_23.jpg)

**图 3.23：智能指针程序输出**

对于这样一个简单的程序，却发生了大量的资源获取和释放。让我们逐步分析：

1.  `createResource()` 内部的局部变量 `res` 在堆上创建并初始化（动态生命周期），导致第一条 "**Resource acquired**"（资源已获取）消息输出。

2.  编译器可能会创建另一个临时对象来返回值。然而，编译器执行了 **拷贝省略** 以移除该拷贝（即它能直接在调用函数分配的栈位置上构建对象）。编译器具有 **返回值优化** (**RVO**) 和 **命名返回值优化** (**NRVO**) 优化，在 C++17 中，这些在特定情况下已成为强制性的。

3.  临时对象通过拷贝赋值赋给 `main()` 函数中的 `the_res` 变量。由于拷贝赋值执行深拷贝，因此又获取了一份资源副本。

4.  赋值完成后，临时对象超出作用域，我们得到第一条 "Resource released"（资源已释放）消息。

5.  当 `main()` 函数返回时，`the_res` 超出作用域，释放第二个 Resource。

因此，如果资源很大，由于拷贝赋值中的深拷贝，我们在 `main()` 中创建 `the_res` 局部变量的方式效率极低，因为我们在大量创建和拷贝大块内存。然而，我们知道当 `createResource()` 创建的临时变量不再需要时，我们将丢弃它并释放其资源。在这些场景下，将资源从临时对象转移（或移动）给该类型的另一个实例会更高效。移动语义使我们能够重写 `smart_ptr` 模板，使其不执行深拷贝，而是转移资源。

让我们为 `smart_ptr` 类添加移动语义：

```cpp
// 移动构造函数 --> 转移资源
smart_ptr(smart_ptr&& a) : m_ptr(a.m_ptr)
{
    a.m_ptr = nullptr;    // 置于安全状态
}

// 移动赋值 --> 转移资源
smart_ptr& operator=(smart_ptr&& a)
{
    // 自赋值检测
    if (&a == this)
        return *this;
    // 释放当前持有的任何资源
    delete m_ptr;
    // 转移资源
    m_ptr = a.m_ptr;
    a.m_ptr = nullptr;    // 置于安全状态
    return *this;
}
```

重新运行程序后，我们得到以下输出：

![Figure 3.24: Smart pointer program output using move semantics](image/C14583_03_24.jpg)

**图 3.24：使用移动语义的智能指针程序输出**

现在，因为移动赋值可用，编译器在以下行使用了它：

`the_res = createResource(); // 赋值调用拷贝赋值 步骤 3/4`

**步骤 3** 现在被移动赋值取代，意味着深拷贝已被移除。

**步骤 4** 不再释放资源，因为带注释 `// 置于安全状态` 的那一行代码——它不再拥有资源，因为所有权已被转移。

关于 **移动构造函数** 和 **移动赋值** 的另一点是，它们的参数在拷贝版本中是 `const`，而在移动版本中是 **非 const** 的。这被称为 **所有权转移**，意味着我们需要修改传入的参数。

移动构造函数的另一种实现可能如下所示：

```cpp
// 移动构造函数 --> 转移资源
smart_ptr(smart_ptr&& a)
{
    std::swap(this->m_ptr, a.m_ptr);
}
```

本质上，我们是在交换资源，C++ STL 为 swap 提供了模板支持并有许多特化版本。这之所以有效，是因为我们使用成员初始化将 `m_ptr` 设置为 `nullptr`。所以我们实际上是将一个 `nullptr` 与 `a` 中存储的值进行交换。

既然我们已经解决了不必要的深拷贝问题，实际上可以从 `smart_ptr()` 中移除拷贝操作，因为所有权转移才是我们想要的行为。如果我们将一个非临时 `smart_ptr` 实例拷贝到另一个非临时 `smart_ptr` 实例，那么我们将有两个对象在超出作用域时删除该资源，这不是期望的行为。要移除（深）拷贝操作，我们如下更改成员函数的定义：

```cpp
smart_ptr(const smart_ptr& a) = delete;
```

