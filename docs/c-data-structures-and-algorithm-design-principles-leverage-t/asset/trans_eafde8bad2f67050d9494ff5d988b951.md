
下面的示例将删除与前一个有效元素相比差值小于2的元素：

```
list1.unique([](int a, int b) { return (b - a) < 2; });
```

```
// list becomes {-1, 1, 5, 10}
```

## 注意

在调用 unique 函数之前，程序员必须确保数据已经排序。因此，我们在它之前调用 sort 函数。unique 函数会将当前元素与前一个已经满足条件的元素进行比较。此外，它始终保留原始列表的第一个元素。因此，总有一个元素可以进行比较。

在下一节中，我们将了解 forward_list 迭代器与 vector/array 迭代器的不同之处。

## 迭代器

你可能已经注意到，在数组和 vector 的某些示例中，我们会向迭代器加上数字。迭代器类似于指针，但它们还为 STL 容器提供了通用接口。对这些迭代器的操作严格基于迭代器的类型，而类型又取决于容器。vector 和数组的迭代器在功能上最为灵活。由于数据的连续性，我们可以使用 operator[] 根据位置直接从容器中访问任意元素。这种迭代器也称为随机访问迭代器。然而，对于 forward_list，没有直接的方法可以向后遍历，甚至无法在不从头开始的情况下从一个节点移动到其前一个节点。因此，这种迭代器唯一允许的算术运算符是递增。这种迭代器也称为前向迭代器。

我们还可以使用其他实用函数，例如 advance、next 和 prev，具体取决于迭代器的类型。next 和 prev 接收一个迭代器和一个距离值，然后返回指向与给定迭代器相距指定距离的元素的迭代器。只要给定的迭代器支持相应操作，它们就会按预期工作。例如，如果我们尝试将 prev 函数用于前向迭代器，则会触发编译错误，因为这种迭代器是前向迭代器，只能向前移动。这些函数所需的时间取决于所使用的迭代器类型。对于随机访问迭代器，它们都是常数时间函数，因为加法和减法是常数时间操作。对于其余迭代器，它们的时间与需要向前或向后遍历的距离呈线性关系。我们将在下面的练习中使用这些迭代器。

## 练习 4：探索不同类型的迭代器

假设我们有一个过去几年新加坡F1大奖赛冠军的列表。借助 vector 迭代器，我们将了解如何从这些数据中获取有用信息。之后，我们将尝试用 forward_list 做同样的事情，并观察它与 vector 迭代器的不同之处。

- 1. 首先包含所需的头文件：

```
```cpp
#include <iostream>
#include <forward_list>
#include <vector>

int main()
{
```
```

- 2. 编写一个包含冠军列表的 vector：

```
```cpp
std::vector<std::string> vec = {"Lewis Hamilton", "Lewis Hamilton", "Nico Rosberg", "Sebastian Vettel", "Lewis Hamilton", "Sebastian Vettel", "Sebastian Vettel", "Sebastian Vettel", "Fernando Alonso"};

auto it = vec.begin();        // Constant time
std::cout << "Latest winner is: " << *it << std::endl;

it += 8;                      // Constant time
std::cout << "Winner before 8 years was: " << *it << std::endl;

advance(it, -3);              // Constant time
std::cout << "Winner before 3 years of that was: " << *it << std::endl;
```
```

- 3. 尝试对 **forward_list** 迭代器做同样的操作，看看它们与 vector 迭代器有何不同：

```
```cpp
std::forward_list<std::string> fwd(vec.begin(), vec.end());

auto it1 = fwd.begin();
std::cout << "Latest winner is: " << *it << std::endl;

advance(it1, 5);   // Time taken is proportional to the number of elements

std::cout << "Winner before 5 years was: " << *it << std::endl;
```
```

```
// Going back will result in compile time error as forward_list only allows us to move towards the end.

// advance(it1, -2);    // Compiler error
}
```

4. 运行本练习应产生以下输出：

```
Latest winner is : Lewis Hamilton
Winner before 8 years was : Fernando Alonso
Winner before 3 years of that was : Sebastian Vettel
Latest winner is : Sebastian Vettel
Winner before 5 years was : Sebastian Vettel
```

5. 现在，让我们看看如果将下面这一行添加到 main 函数末尾，给这个迭代器加上一个数字会发生什么：

```
it1 += 2;
```

我们会得到类似这样的错误消息：

```
no match for 'operator+=' (operand types are std::_Fwd_list_iterator<int>' and 'int')
```

我们在本练习中探索的各种迭代器对于轻松地从数据集中获取任何数据非常有用。

正如我们所见，std::array 是 C 风格数组的轻量封装，而 std::forward_list 只是单链表的轻量封装。它提供了一个简单且不易出错的接口，同时不会影响性能和内存。

此外，由于我们可以在 vector 中立即访问任何元素，因此 vector 迭代器上的加法和减法操作的时间复杂度为 O(1)。另一方面，forward_list 只能通过遍历来访问元素。因此，其迭代器的加法操作的时间复杂度为 O(n)，其中 n 是前进的步数。

在下面的练习中，我们将创建一个自定义容器，其工作方式与 std::forward_list 类似，但进行了一些改进。我们将定义许多与 forward_list 函数等效的函数。这也有助于你理解这些函数在底层是如何工作的。

## 练习 5：构建一个基本的自定义容器

在本练习中，我们将实现一个与 std::forward_list 等效的容器，并做出一些改进。我们将从一个名为 singly_ll 的基本实现开始，并逐步进行改进：

- 1. 添加所需的头文件，然后从包含单个节点的 singly_ll 基本实现开始：

```
```cpp
#include <iostream>
#include <algorithm>

struct singly_ll_node
{
    int data;
    singly_ll_node* next;
};
```
```

- 2. 现在，我们将实现真正的 singly_ll 类，它将节点封装起来以便于接口交互：

```
```cpp
class singly_ll
{
public:
    using node = singly_ll_node;
    using node_ptr = node*;

private:
    node_ptr head;
```
```

- 3. 现在，让我们像 forward_list 中那样添加 push_front 和 pop_front：

```
```cpp
public:
void push_front(int val)
{
    auto new_node = new node{val, NULL};
    if(head != NULL)
        new_node->next = head;
    head = new_node;
}

void pop_front()
{
    auto first = head;
    if(head)
```

6. 让我们添加作为友元函数的相等性操作：

```
friend bool operator==(const singly_ll_iterator& left, const singly_ll_iterator& right)
{
    return left.ptr == right.ptr;
}

friend bool operator!=(const singly_ll_iterator& left, const singly_ll_iterator& right)
{
    return left.ptr != right.ptr;
};
```

7. 让我们回到链表类。既然我们已经有了迭代器类，接下来实现 begin 和 end 函数以简化遍历。我们还将为两者添加 const 版本：

```
singly_ll_iterator begin()
{
    return singly_ll_iterator(head);
}

singly_ll_iterator end()
{
    return singly_ll_iterator(NULL);
}

singly_ll_iterator begin() const
{
    return singly_ll_iterator(head);
}

singly_ll_iterator end() const
{
    return singly_ll_iterator(NULL);
}
```

8. 让我们实现一个默认构造函数、一个用于深拷贝的拷贝构造函数，以及一个带有 initializer_list 的构造函数：

```
singly_ll() = default;

singly_ll(const singly_ll& other) : head(NULL)
{
    if(other.head)
    {
        head = new node;
        auto cur = head;
        auto it = other.begin();
        while(true)
        {
            cur->data = *it;

            auto tmp = it;
            ++tmp;
            if(tmp == other.end())
                break;

            cur->next = new node;
            cur = cur->next;
            it = tmp;
        }
    }
}

singly_ll(const std::initializer_list<int>& ilist) : head(NULL)
{
    for(auto it = std::rbegin(ilist); it != std::rend(ilist); it++)
        push_front(*it);
};
```
