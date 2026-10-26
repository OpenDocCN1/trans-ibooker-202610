## 第 1 章：列表、栈和队列

### 活动 1：实现歌曲播放列表

在本活动中，我们将实现一个经过调整的双向链表版本，它可以用来存储歌曲播放列表，并支持必要的功能。请按照以下步骤完成本活动：

- 1. 首先，我们包含头文件并编写带有所需数据成员的节点结构：

```
#include <iostream>

template <typename T>
struct cir_list_node
{
    T* data;
    cir_list_node *next, *prev;

    ~cir_list_node()
    {
        delete data;
    }
};

template <typename T>
struct cir_list
{
    public:
        using node = cir_list_node<T>;
        using node_ptr = node*;
    private:
        node_ptr head;
        size_t n;
```

- 2. 现在，让我们编写一个基本的构造函数和大小函数：

```
public:
cir_list(): n(0)
{
    head = new node{NULL, NULL, NULL};  // Dummy node - having NULL data
    head->next = head;
    head->prev = head;
}

size_t size() const
{
    return n;
}
```

稍后，在讨论使用迭代器进行迭代时，我们会讨论为什么在第一个节点和最后一个节点之间需要一个哑节点。

- 3. 现在，让我们编写**插入**和**删除**函数。两者都将接收一个要插入或删除的值：

```
void insert(const T& value)
{
    node_ptr newNode = new node{new T(value), NULL, NULL};
    n++;
    auto dummy = head->prev;
    dummy->next = newNode;
    newNode->prev = dummy;
    if(head == dummy)
    {
        dummy->prev = newNode;
        newNode->next = dummy;
        head = newNode;
        return;
    }
    newNode->next = head;
    head->prev = newNode;
    head = newNode;
}
```

```
void erase(const T& value)
{
    auto cur = head, dummy = head->prev;
    while(cur != dummy)
    {
        if(*(cur->data) == value)
        {
            cur->prev->next = cur->next;
            cur->next->prev = cur->prev;
            if(cur == head)
                head = head->next;
            delete cur;
            n--;
            return;
        }
        cur = cur->next;
    }
}
```

- 4. 现在，让我们为所需的迭代器编写一个基本结构，并添加成员以访问实际数据：

```
struct cir_list_it
{
private:
    node_ptr ptr;
public:
    cir_list_it(node_ptr p) : ptr(p)
    {}

    T& operator*()
    {
        return *(ptr->data);
    }

    node_ptr get()
    {
        return ptr;
    }
```

- 5. 现在，让我们实现迭代器的核心函数——前置递增和后置递增：

```
cir_list_it& operator++()
{
    ptr = ptr->next;
    return *this;
}

cir_list_it operator++(int)
{
    cir_list_it it = *this;
    ++(*this);
    return it;
}
```

- 6. 让我们添加与递减相关的操作，使其成为双向迭代器：

```
cir_list_it& operator--()
{
    ptr = ptr->prev;
    return *this;
}
```

```
cir_list_it operator--(int)
{
    cir_list_it it = *this;
    --(*this);
    return it;
}
```

- 7. 让我们为迭代器实现与相等性相关的运算符，这些运算符对于基于范围的循环至关重要：

```
friend bool operator==(const cir_list_it& it1, const cir_list_it& it2)
{
    return it1.ptr == it2.ptr;
}
```

```
friend bool operator!=(const cir_list_it& it1, const cir_list_it& it2)
{
    return it1.ptr != it2.ptr;
}
```

- 8. 现在，让我们编写 begin 和 end 函数，同时提供它们的 const 版本：

```
cir_list_it begin()
{
    return cir_list_it{head};
}
```

```
cir_list_it begin() const
{
    cir_list_it it = *this;
    ++(*this);
    return it;
}
```

- 3. 现在，我们可以创建一副扑克牌并洗牌，以将牌随机发给四位玩家。我们将把这个逻辑写在**游戏**类中，并在**主**函数中稍后调用这些函数：

```
struct game
{
    std::array<card, 52> deck;
    std::vector<card> player1, player2, player3, player4;
    void buildDeck()
    {
        for(int i = 0; i < 13; i++)
            deck[i] = card{i + 1, card::HEART};
        for(int i = 0; i < 13; i++)
            deck[i + 13] = card{i + 1, card::SPADE};
        for(int i = 0; i < 13; i++)
            deck[i + 26] = card{i + 1, card::CLUB};
        for(int i = 0; i < 13; i++)
            deck[i + 39] = card{i + 1, card::DIAMOND};
    }

    void dealCards()
    {
        unsigned seed = std::chrono::system_clock::now().time_since_epoch().count();
        std::shuffle(deck.begin(), deck.end(), std::default_random_engine(seed));
        player1 = {deck.begin(), deck.begin() + 13};
        player2 = {deck.begin() + 13, deck.begin() + 26};
        player3 = {deck.begin() + 26, deck.begin() + 39};
        player4 = {deck.begin() + 39, deck.end()};
    }
};
```

- 4. 让我们编写玩一局的核心逻辑。为了避免重复代码，我们将编写一个实用函数，用于比较两位玩家的手牌并在需要时移除两张牌：

```
bool compareAndRemove(std::vector<card>& p1, std::vector<card>& p2)
{
    if(p1.back().number == p2.back().number)
    {
        p1.pop_back();
        p2.pop_back();
        return true;
    }
    return false;
}

void playOneRound()
{
    if(compareAndRemove(player1, player2))
    {
        compareAndRemove(player3, player4);
        return;
    }
    else if(compareAndRemove(player1, player3))
    {
        compareAndRemove(player2, player4);
        return;
    }
    else if(compareAndRemove(player1, player4))
    {
        compareAndRemove(player2, player3);
        return;
    }
    else if(compareAndRemove(player2, player3))
    {
        return;
    }
    else if(compareAndRemove(player2, player4))
    {
        return;
    }
    else if(compareAndRemove(player3, player4))
    {
        return;
    }
    unsigned seed = std::chrono::system_clock::now().time_since_epoch().count();
    std::shuffle(player1.begin(), player1.end(), std::default_random_engine(seed));
    std::shuffle(player2.begin(), player2.end(), std::default_random_engine(seed));
}
```

```
friend std::ostream& operator<<(std::ostream& os, const Job& j)
{
    os << "id: " << id << ", user: " << user << ", time: " << time << " seconds" << std::endl;
    return os;
}
};
int Job::count = 0;
```

- 3. 现在，让我们实现**打印机**类。我们将使用 `std::queue` 来实现作业的先到先服务策略。我们将根据类可以在内存中存储的最大作业数来保持类的模板化：

```
template <size_t N>
class Printer
{
    std::queue<Job> jobs;
public:
    bool addNewJob(const Job& job)
    {
        if(jobs.size() == N)
            return false;
        std::cout << "Added job in the queue: " << job;
        jobs.push(job);
        return true;
    }
};
```

- 4. 现在，让我们实现另一个主要功能——打印作业：

```
void startPrinting()
{
    while(not jobs.empty())
    {
        std::cout << "Processing job: " << jobs.front();
        jobs.pop();
    }
}
};
```

- 5. 现在，让我们使用这些类来模拟该场景：

```
int main()
{
    Printer<5> printer;
    Job j1("John", 10);
    Job j2("Jerry", 4);
    Job j3("Jimmy", 5);
    Job j4("George", 7);
    Job j5("Bill", 8);
    Job j6("Kenny", 10);
    printer.addNewJob(j1);
    printer.addNewJob(j2);
    printer.addNewJob(j3);
    printer.addNewJob(j4);
    printer.addNewJob(j5);

    if(not printer.addNewJob(j6)) // Can't add as queue is full.
    {
        std::cout << "Couldn't add 6th job" << std::endl;
    }

    printer.startPrinting();

    printer.addNewJob(j6); // Can add now, as queue got emptied
    printer.startPrinting();
}
```

- 6. 以下是上述代码的输出：

```
Added job in the queue: id: 1, user: John, time: 10 seconds
Added job in the queue: id: 2, user: Jerry, time: 4 seconds
Added job in the queue: id: 3, user: Jimmy, time: 5 seconds
Added job in the queue: id: 4, user: George, time: 7 seconds
Added job in the queue: id: 5, user: Bill, time: 8 seconds
Couldn't add 6th job
Processing job: id: 1, user: John, time: 10 seconds
Processing job: id: 2, user: Jerry, time: 4 seconds
Processing job: id: 3, user: Jimmy, time: 5 seconds
Processing job: id: 4, user: George, time: 7 seconds
Processing job: id: 5, user: Bill, time: 8 seconds
Added job in the queue: id: 6, user: Kenny, time: 10 seconds
Processing job: id: 6, user: Kenny, time: 10 seconds
```





```cpp
return;
}

if(found->is_dir)
{
    for(auto child: found->children)
    {
        std::cout << (child->is_dir ? "d " : "- ") << child->name << std::endl;
    }
}
else
{
    std::cout << "- " << found->name << std::endl;
}
};
};
```

9. 编写一个 `main` 函数，以便我们可以使用上述函数：

```cpp
int main()
{
    file_system fs;
    fs.add("usr", true);  // Add directory usr in "/"
    fs.add("etc", true);  // Add directory etc in "/"
    fs.add("var", true);  // Add directory var in "/"
    fs.add("tmp_file", false);  // Add file tmp_file in "/"

    std::cout << "Files/Directories under \"/\"" << std::endl;
    fs.show_path("/");  // List files/directories in "/"

    std::cout << std::endl;
    fs.change_dir("usr");
    fs.add("Packt", true);
    fs.add("Packt/Downloads", true);
    fs.add("Packt/Downloads/newFile.cpp", false);

    std::cout << "Let's see the contents of dir usr: " << std::endl;
    fs.show_path("usr");  // This will not print the path successfully, since we're already inside the dir usr. And there's no directory named usr inside it.

    std::cout << "Let's see the contents of \"/usr\"" << std::endl;
    fs.show_path("/usr");

    std::cout << "Let's see the contents of \"/usr/Packt/Downloads\"" <<
    std::endl;
    fs.show_path("/usr/Packt/Downloads");
}
```

上述代码的输出如下：

```
Files/Directories under "/"
d usr
d etc
d var
- tmp_file
Current working directory changed to usr.
Let's try to print the contents of usr:
No such path: usr.
Let's see the contents of "/usr"
d Packt
Contents of "/usr/Packt/Downloads"
- newFile.cpp
```

### 活动5：使用堆进行K路归并

在此活动中，我们将多个有序数组合并为一个有序数组。以下步骤将帮助你完成此活动：

1. 从所需头文件开始：

```cpp
#include <iostream>
#include <algorithm>
#include <vector>
```

2. 现在，实现归并的主要算法。它将接收一个由多个 int 向量组成的向量作为输入，其中包含所有已排序的向量。然后，它将返回归并后的 int 向量。首先，让我们构建堆节点：

```cpp
struct node
{
    int data;
    int listPosition;
    int dataPosition;
};

std::vector<int> merge(const std::vector<std::vector<int>>& input)
{
    auto comparator = [] (const node& left, const node& right)
    {
        if(left.data == right.data)
            return left.listPosition > right.listPosition;
        return left.data > right.data;
    };
```

正如我们所见，堆节点将包含三部分内容——数据、该列表在输入中的位置，以及该数据项在该列表中的位置。

3. 让我们构建堆。思路是建立一个包含所有列表中最小元素的最小堆。这样，当我们从堆中弹出元素时，可以保证得到最小的元素。移除该元素后，如果同一个列表中还有下一个元素，我们需要将其插入堆中：

```cpp
std::vector<node> heap;
for(int i = 0; i < input.size(); i++)
{
    heap.push_back({input[i][0], i, 0});
    std::push_heap(heap.begin(), heap.end(), comparator);
}
```

4. 现在，我们将构建结果向量。我们只需从堆中移除元素，直到堆为空，并且如果该元素所属的列表中还有下一个元素，则用下一个元素替换它：

```cpp
std::vector<int> result;
while(!heap.empty())
{
    std::pop_heap(heap.begin(), heap.end(), comparator);
    auto min = heap.back();
    heap.pop_back();

    result.push_back(min.data);
    int nextIndex = min.dataPosition + 1;
    if(nextIndex < input[min.listPosition].size())
    {
        heap.push_back({input[min.listPosition][nextIndex], min.listPosition, nextIndex});
        std::push_heap(heap.begin(), heap.end(), comparator);
    }
}

return result;
}
```

5. 让我们编写一个 **main** 函数，以便我们可以使用上述函数：

```cpp
int main()
{
    std::vector<int> v1 = {1, 3, 8, 15, 105};
    std::vector<int> v2 = {2, 3, 10, 11, 16, 20, 25};
    std::vector<int> v3 = {-2, 100, 1000};
    std::vector<int> v4 = {-1, 0, 14, 18};
    auto result = merge({v1, v2, v3, v4});
    for(auto i : result)
        std::cout << i << ' ';
    return 0;
}
```

你应该会看到以下输出：

```
-2 -1 0 1 2 3 3 8 10 11 14 15 16 18 20 25 100 105 1000
```
