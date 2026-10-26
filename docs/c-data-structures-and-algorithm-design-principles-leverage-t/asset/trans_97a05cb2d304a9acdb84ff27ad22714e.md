

从第一次初始化可以看出，提供大小并不是强制性的。如果我们没有显式指定大小，并且也没有通过指定其元素来推断大小，vector 将根据编译器实现初始化为取决于元素的容量。术语“size”指的是 vector 中实际存在的元素数量，这可能与其容量不同。因此，对于第一次初始化，size 将为零，但 capacity 可能是某个较小数字或零。

我们可以使用 `push_back` 或 `insert` 函数在 vector 中插入元素。`push_back` 会在末尾插入元素。`insert` 将迭代器作为第一个参数用于位置，并且可用于在任何位置插入元素。`push_back` 是 vector 中使用频率非常高的函数，因为其性能。`push_back` 算法的伪代码如下：

```
push_back(val):
    if size < capacity
        // If vector has enough space to accommodate this element
        - Set element after the current last element = val
        - Increment size
        - return;

    if vector is already full
        - Allocate memory of size 2*size
        - Copy/Move elements to newly allocated memory
        - Make original data point to new memory
        - Insert the element at the end
```

实际实现可能会略有不同，但逻辑保持不变。如我们所见，如果有足够的空间，在末尾插入东西只需要 O(1) 时间。然而，如果空间不足，它将不得不复制/移动所有元素，这将花费 O(n) 时间。大多数实现在耗尽容量时都会将 vector 的大小加倍。因此，O(n) 时间操作是在 n 个元素之后执行的。所以，平均而言，它只需要多一步，使其平均时间复杂度更接近 O(1)。这在实践中提供了相当好的性能，因此它是一个使用频率很高的容器。

对于 `insert` 函数，你别无选择，只能将给定迭代器之后的元素向右移动。`insert` 函数为我们做了这件事。它还在需要时处理重新分配。由于需要移动元素，它花费 O(n) 时间。以下示例演示如何实现 vector 插入函数。

考虑一个包含前五个自然数的 vector：

```
std::vector<int> vec = {1, 2, 3, 4, 5};
```

> **注意：** Vector 没有 `push_front` 函数。它有一个通用的 `insert` 函数，该函数将迭代器作为位置参数。

通用的 `insert` 函数可用于在前面插入元素，如下所示：

```
vec.insert(vec.begin(), 0);
```

让我们再看几个 `push_back` 和 `insert` 函数的示例：

```
std::vector<int> vec;

// Empty vector {}

vec.push_back(1);

// Vector has one element {1}

vec.push_back(2);

// Vector has 2 elements {1, 2}

vec.insert(vec.begin(), 0);

// Vector has 3 elements {0, 1, 2}

vec.insert(find(vec.begin(), vec.end(), 1), 4);

// Vector has 4 elements {0, 4, 1, 2}
```

如前面的代码所示，`push_back` 在末尾插入元素。此外，`insert` 函数将插入位置作为参数。它以迭代器的形式接收。因此，`begin()` 函数允许我们在开头插入元素。

现在我们已经了解了正常的插入函数，让我们来看一下与 `push_back` 和 `insert` 函数相比，vector 可用的一些更好的替代方案。`push_back` 和 `insert` 的一个缺点是它们首先构造元素，然后将元素复制或移动到 vector 缓冲区内的新位置。此操作可以通过调用新位置本身的新元素的构造函数来优化，这可以通过 `emplace_back` 和 `emplace` 函数完成。建议你为获得更好的性能而使用这些函数而不是正常的插入函数。由于我们是在原地构造元素，我们只需要传递构造函数参数，而不是构造好的值本身。然后，函数将负责将参数转发到适当位置的构造函数。

`std::vector` 还提供了 `pop_back` 和 `erase` 函数来从中删除元素。`pop_back` 从 vector 中删除最后一个元素，有效地将大小减少一个。`erase` 有两个重载——通过指向它的迭代器删除单个元素，以及通过迭代器删除一系列元素，其中范围由要删除的第一个元素（包含）和要删除的最后一个元素（不包含）定义。C++ 标准不要求这些函数减少 vector 的容量。它完全取决于编译器实现。`pop_back` 不需要重新排列元素，因此可以非常快速地完成。它的复杂度为 O(1)。然而，`erase` 需要移动元素，因此花费 O(n) 时间。在下面的练习中，我们将看到这些函数是如何实现的。

现在，让我们来看一下关于以不同方式从 vector 中删除元素的示例：

考虑一个包含 10 个元素的 vector - {0, 1, 2, 3, 4, 5, 6, 7, 8, 9}：

```
vec.pop_back();
// Vector has now 9 elements {0, 1, 2, 3, 4, 5, 6, 7, 8}
```

```
vec.erase(vec.begin());
// Vector has now 8 elements {1, 2, 3, 4, 5, 6, 7, 8}
```

```
vec.erase(vec.begin() + 1, vec.begin() + 4);
// Now, vector has 5 elements {1, 5, 6, 7, 8}
```

## std::forward_list

到目前为止，我们只看到类数组结构，但正如我们所见，在连续数据结构中间进行插入和删除是非常低效的操作。而这正是链表类结构发挥作用的地方。许多应用程序需要在数据结构中间频繁进行插入和删除。例如，任何具有多个标签页的浏览器都可以在任何时间点和任何位置添加额外的标签页。同样，任何音乐播放器都会有一系列循环播放的歌曲，你也可以在中间插入任何歌曲。在这种情况下，我们可以使用链表结构以获得良好的性能。我们将在活动 1 *实现歌曲播放列表* 中看到音乐播放器的用例。现在，让我们探索 C++ 为我们提供了哪些容器。

链表的基本结构要求我们拥有一个指针，并使用 **new** 和 **delete** 运算符手动管理内存分配和释放。虽然并不困难，但它可能导致难以追踪的错误。因此，就像 `std::array` 为 C 风格数组提供了一个薄包装一样，`std::forward_list` 为基本的链表提供了一个薄包装。

`std::forward_list` 的目的是在不影响与基本链表相比的性能的情况下提供一些附加功能。为了保持性能，它不提供获取列表大小或直接获取除第一个元素之外的任何元素的函数。因此，它有一个名为 `front()` 的函数来获取对第一个元素的引用，但没有像 `back()` 这样访问最后一个元素的函数。它确实提供了常见操作的函数，例如插入、删除、反转和拼接。这些函数不会影响基本链表的内存需求或性能。

此外，就像 `std::vector` 一样，`std::forward_list` 如果需要也可以接受第二个模板参数作为自定义分配器。因此，我们可以轻松地将其用于从自定义内存管理中受益的高级应用程序。

### 在 forward_list 中插入和删除元素

`std::forward_list` 提供了 `push_front` 和 `insert_after` 函数，可用于在链表中插入元素。这两者与 vector 的插入函数略有不同。`push_front` 用于在前面插入元素。由于 `forward_list` 没有直接访问最后一个元素的权限，因此它不提供 `push_back` 函数。对于在特定位置插入，我们使用 `insert_after` 而不是 `insert`。这是因为在链表中插入元素需要更新我们想要在之后插入新元素的元素的 next 指针。如果我们只提供想要插入新元素的迭代器，我们就无法快速访问前一个元素，因为在 `forward_list` 中向后遍历是不允许的。

