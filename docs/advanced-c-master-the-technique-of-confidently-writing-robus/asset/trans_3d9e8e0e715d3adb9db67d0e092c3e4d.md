
# 线程同步机制

## 资源同步

为防止竞争和数据冒险，存在一种共享数据锁定机制，当某个流意图更改或读取这些数据时会用到它。这种机制称为 `resource synchronization`。为了同步，我们需要分配更改或读取共享资源的代码片段。这类代码片段称为 `critical sections`。同步的做法是：当某个线程进入临界区时，阻塞该临界区。其他同样意图执行该临界区代码的线程将被阻塞。当执行临界区的线程离开时，锁被释放。然后，下一个线程将重复这一过程。

### 同步递增示例

考虑之前的递增示例，但现在采用同步访问。回想一下，我们有 `Thread A` 和 `Thread B`，还有一个值为 0 的变量。

`Thread A` 开始递增：

1. 进入临界区并将其锁定。
2. 读取变量的值（`var = 0`）。
3. 将其递增（`tmp = 1`）。
4. 被操作系统中断。

`Thread B` 开始递增：

1. 尝试进入临界区；但它已锁定，所以线程等待。

`Thread A` 继续递增：

1. 写入新值（`var = 1`）。

`Thread B` 继续递增：

1. 进入临界区并将其锁定。
2. 读取变量的值（`var = 1`）。
3. 将其递增（`tmp = 2`）。
4. 写入新值（`var = 2`）。

两个线程完成后，变量包含正确的结果。因此，同步确保共享数据不会被破坏。请看下图，以更好地理解此示例：

![Figure 5.14: Two threads increment the same shared variable in a synchronized way](image/C14583_05_14.jpg)

*图 5.14：两个线程以同步方式递增同一个共享变量*

标识临界区并预判非同步访问的可能后果是一项非常困难的任务。因为过度同步违背了多线程工作的初衷。然而，如果两三个线程在一个临界区中工作得相当快，程序中可能有几十个线程全都阻塞在该临界区中。这将极大地拖慢程序速度。

## 事件同步

还有一种同步线程工作的机制——`event synchronization`。这意味着当一个线程挂起工作，直到另一个线程发出某事件发生的信号时，对线程工作进行同步。例如，有 `Thread A`，它从另一个进程接收消息。它将消息写入队列并等待新消息。还有另一个线程 `Thread B`，处理这些消息。它从队列读取消息并对其执行某些操作。当没有消息时，`Thread B` 处于休眠状态。当 `Thread A` 接收到新消息时，它唤醒 `Thread B` 并处理它。下图清晰地展示了两个线程的事件同步：

![Figure 5.15: Event synchronization of two threads](image/C14583_05_15.jpg)

*图 5.15：两个线程的事件同步*

## 有缺陷的接口设计与竞态条件

然而，即使在同步代码中，也可能出现竞态条件的另一个原因——类的有缺陷接口。为了理解这是什么，让我们看看下面的例子：

```cpp
class Messages
{
public:
    Messages(const int& size)
    : ArraySize(size)
    , currentIdx(0)
    , msgArray(new std::string[ArraySize])
    {}

    void push(const std::string& msg)
    {
        msgArray[currentIdx++] = msg;
    }

    std::string pop()
    {
        auto msg = msgArray[currentIdx - 1];
        msgArray[currentIdx - 1] = "";
        --currentIdx;
        return msg;
    }

    bool full()
    {
        return ArraySize == currentIdx;
    }

    bool empty()
    {
        return 0 == currentIdx;
    }

private:
    const int ArraySize;
    int currentIdx;
    std::string * msgArray;
};
```

这里，我们有一个名为 `Messages` 的类，它包含一个动态分配的字符串数组。在构造函数中，它接受数组大小并创建一个指定大小的数组。它有一个函数 `full()`，如果数组已满则返回 `true`，否则返回 `false`。它还有一个 `empty()` 函数，如果数组为空则返回 `true`，否则返回 `false`。用户有责任在压入新值前检查数组是否已满，以及在从数组弹出新值前检查数组是否为空。这是一个导致竞态条件的不良类接口示例。即使我们用锁保护 `push()` 和 `pop()` 函数，竞态条件也不会消失。让我们看看使用 `Messages` 类的下面示例：

```cpp
int main()
{
    Messages msgs(10);
    std::thread thr1([&msgs](){
        while(true)
        {
            if (!msgs.full())
            {
                msgs.push("Hello");
            }
            else
            {
                break;
            }
        }
    });
    std::thread thr2([&msgs](){
        while(true)
        {
            if (!msgs.empty())
            {
                std::cout << msgs.pop() << std::endl;
            }
            else
            {
                break;
            }
        }
    });
    thr1.detach();
    thr2.detach();
    using namespace std::chrono_literals;
    std::this_thread::sleep_for(2s);
    return 0;
}
```

这里，我们创建了一个 `msgs` 变量，然后创建了第一个线程，向 `msgs` 压入值。然后，我们创建了第二个线程，从数组弹出值并将它们分离。即使我们使用锁机制保护所有函数，其中一个线程也可能检查数组大小并被操作系统中断。此时，另一个线程可以更改数组。当第一个线程继续工作时，它可能会尝试向已满数组压入数据或从空数组弹出数据。因此，同步只有配合良好的设计才有效。

## 死锁

还有一个同步问题。让我们回到哲学家进餐的例子。最初的问题是每个哲学家只有一根筷子。所以，他们可以通过互相共享筷子，一个接一个地吃寿司。虽然他们吃完寿司需要很长时间，但大家都能吃饱。但如果他们每个人同时抓起一根筷子且不愿共享第二根筷子，他们就无法吃寿司，因为每个人都会永远等待第二根筷子。这导致 `deadlock`。这发生在两个线程都在等待另一个线程继续其工作时。死锁的原因之一是一个线程 join 另一个线程，但另一个线程也 join 第一个线程。所以，当两个线程相互 join 时，它们都无法继续工作。让我们看看下面的死锁示例：

```cpp
#include <thread>

std::thread* thr1;
std::thread* thr2;

void someStuff()
{
    thr1->join();
}

void someAnotherStuff()
{
    thr2->join();
}

int main()
{
    std::thread t1(someStuff);
    std::thread t2(someAnotherStuff);
    thr1 = &t1;
    thr2 = &t2;
    using namespace std::chrono_literals;
    std::this_thread::sleep_for(2s);
    return 0;
}
```

在 main 函数中，我们有两个线程 `t1` 和 `t2`。我们用 `someStuff()` 函数初始化 `t1` 线程，该函数做一些有用的工作。我们也用 `someAnotherStuff()` 函数初始化 `t2` 线程，该函数做一些更有用的工作。我们有指向这些线程的全局指针，并在 `t2` 执行的函数中 join 指向 `t1` 的指针。我们也在 `t1` 执行的函数中 join 指向 `t2` 的指针。这样做，它们相互 join 了。这导致死锁。
