

```
BufferedWriter(const char* filename);
~BufferedWriter();

bool write(const char* data, size_t length);

private:
    const size_t BufferSize{4096};
    FILE* m_file{nullptr};
    size size_t m_writePos{0};
    char* m_buffer{new char[BufferSize]};
};
```

该类用于缓冲对文件的写入。

> Note
> 使用派生自 iostream 的类时通常不需要此操作，因为它们本身已提供缓冲。

每次调用 write() 函数时，数据都会被添加到分配的缓冲区中，直到达到 BufferSize 时，数据才会被真正写入文件，并且缓冲区会被重置。

但如果我们想将这个 BufferedWriter 实例赋值给另一个实例或复制它呢？正确的行为是什么？

如果我们只是让默认的拷贝构造函数/拷贝赋值按其方式执行，我们会得到成员逐个的拷贝。这意味着我们有两个 BufferedWriter 实例，它们持有对同一文件的句柄和指向缓冲区的指针。当第一个对象实例被销毁时，作为优秀的程序员，我们会清理文件并关闭它，并通过删除来释放内存。第二个实例现在拥有一个已失效的文件句柄和指向我们已告知操作系统为下一个用户回收的内存的指针。任何尝试使用这些资源的行为，包括销毁它们，都会导致未定义行为，并且很可能导致程序崩溃。默认的拷贝构造函数/拷贝赋值运算符执行的是所谓的浅拷贝——也就是说，它按位逐位复制所有成员，但不复制它们所引用的内容。

我们拥有的两个资源可以被不同地处理。首先，只应有一个类拥有 m_buffer。有两种处理方式：

- 阻止类的拷贝，从而阻止内存的拷贝
- 执行深拷贝，其中第二个实例中的缓冲区由构造函数分配，并且第一个缓冲区的內容被复制

其次，只应有一个类拥有文件句柄 m_file。有两种处理方式：

- 阻止类的拷贝，从而阻止文件句柄的拷贝
- 将所有权从原始实例转移到第二个实例，并将原始实例标记为无效或空

实现深拷贝很容易，但我们如何转移资源的所有权？要回答这个问题，我们需要再次查看临时对象和值类别。

## Temporary Objects

临时对象是在表达式的结果被存入变量之前，或被遗忘之前，用于存储表达式的中间结果。表达式是返回值的任何代码，包括按值传递给函数、从函数返回值、隐式转换、字面量和二元运算符。临时对象是右值表达式，它们为表达式结果提供了一个临时分配的内存位置。在 C++11 之前，正是临时对象的创建以及数据在它们之间的拷贝导致了一些性能问题。为了解决这个问题，C++11 引入了右值引用，以实现所谓的移动语义。

## Move Semantics

右值引用（用双与号 '&&' 表示）是一种仅被赋予右值的引用，它会延长右值的生命周期，直到右值引用使用完毕。因此，右值可以活得比定义它的表达式更久。有了右值引用，我们现在可以通过移动构造函数和移动赋值运算符来实现移动语义。移动语义的目的是从被引用的对象窃取资源，从而避免昂贵的拷贝操作。移动完成后，被引用的对象必须被置于稳定状态。换句话说，被移动的对象必须被置于一种状态，在销毁时不会导致任何未定义行为或程序崩溃，并且不应影响从其窃取的资源。

```
smart_ptr(const smart_ptr& a)
{
    m_ptr = new T;
    *m_ptr = *a.m_ptr;      // use operator=() to do deep copy
}

// Copy assignment --> Do deep copy
smart_ptr& operator=(const smart_ptr& a)
{
    // Self-assignment detection
    if (&a == this)
        return *this;

    // Release any resource we're holding
    delete m_ptr;

    // Copy the resource
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
```

```
{
public:
    Resource() { std::cout << "Resource acquired\n"; }
    ~Resource() { std::cout << "Resource released\n"; }
};
```

```
smart_ptr<Resource> createResource()
{
    smart_ptr<Resource> res(new Resource);                     // Step 1
    return res; // return value invokes the copy constructor      // Step 2
}
```

```
int main()
{
    smart_ptr<Resource> the_res;
    the_res = createResource(); // assignment invokes the copy assignment Step 3/4

    return 0; // Step 5
}
```

运行此程序时，生成以下输出：

```
$ ./smart_ptr.exe
Resource acquired
Resource acquired
Resource released
Resource released
$
```

Figure 3.23: Smart Pointer Program output

对于这样一个简单的程序，资源的获取和释放次数很多。让我们拆解一下：

- 1. createResource() 内部的局部变量 res 被创建并在堆上初始化（动态生命周期），导致第一个 "Resource acquired" 消息。

- 2. 编译器可能会为返回值创建另一个临时对象。然而，编译器已执行 **copy elision** 以移除拷贝（也就是说，它能够直接在调用函数分配的栈位置上构建对象）。编译器具有 **Return-Value-Optimization (RVO)** 和 **Named- Return-Value-Optimization (NRVO)** 优化，在 C++17 下在某些情况下这些优化是强制性的。

- 3. 临时对象通过拷贝赋值被赋给 main() 函数中的 `the_res` 变量。由于拷贝赋值执行深拷贝，又获取了资源的另一个拷贝。

- 4. 当赋值完成后，临时对象超出作用域，我们得到第一个 "Resource released" 消息。

- 5. 当 main() 函数返回时，`the_res` 超出作用域，从而释放第二个 Resource。

因此，如果资源很大，我们在 main() 中创建 **the_res** 局部变量的方法非常低效，因为由于拷贝赋值中的深拷贝，我们在创建和拷贝大块内存。然而，我们知道当 **createResource()** 创建的临时变量不再需要时，我们就会丢弃它并释放其资源。在这些场景下，将资源从临时对象转移（或移动）到类型的另一个实例会更高效。移动语义使得我们可以重写 **smart_ptr** 模板，使其不执行深拷贝而是转移资源。

让我们为 **smart_ptr** 类添加移动语义：

// Move constructor --> transfer resource
smart_ptr(smart_ptr&& a) : m_ptr(a.m_ptr)
{
    a.m_ptr = nullptr;    // Put into safe state
}

// Move assignment --> transfer resource
smart_ptr& operator=(smart_ptr&& a)
{
    // Self-assignment detection
    if (&a == this)
        return *this;

    // Release any resource we're holding

```
delete m_ptr;

// Transfer the resource
m_ptr = a.m_ptr;
a.m_ptr = nullptr;  // Put into safe state
return *this;
}
```

重新运行程序后，我们得到以下输出：

![](img/6507d435e2edfa3c00c220d0ccfcaf94_256_0.png)

Figure 3.24: Smart pointer program output using move semantics

现在，由于移动赋值可用，编译器在以下行使用它：

```
the_res = createResource(); // assignment invokes the copy assignment Step 3/4
```

步骤 3 现在被移动赋值取代，意味着深拷贝已被移除。

步骤 4 不再释放资源，因为带有注释 "//" 的行将其置于安全状态——它不再拥有资源，因为所有权已被转移。

移动构造函数和移动赋值的一个需要注意的点是，与其拷贝版本的参数为 const 不同，它们的移动版本参数为非 const。这被称为所有权的转移，这意味着我们需要修改传入的参数。

移动构造函数的另一种实现可能如下所示：

// Move constructor --> transfer resource
smart_ptr(smart_ptr&& a)
{
    std::swap(this->m_ptr, a.m_ptr);
}

本质上，我们正在交换资源，并且 C++ STL 对 swap 提供了模板支持并有许多特化。这之所以可行，是因为我们使用成员初始化将 m_ptr 设置为 nullptr。因此，我们正在交换一个 nullptr 与 a 中存储的值。

