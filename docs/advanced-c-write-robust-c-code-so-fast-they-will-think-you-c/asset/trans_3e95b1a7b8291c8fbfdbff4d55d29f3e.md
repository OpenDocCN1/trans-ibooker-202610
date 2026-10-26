

```cpp
std::vector<Person> m_Persons;
};

#endif // PERSONS_HPP
```

- **5.** 在源文件中，声明用户自定义的构造函数，我们在其中将向量的大小保留为 50 个元素（以避免在增长过程中进行调整）：

```cpp
Persons::Persons()
{
    m_Persons.reserve(CountPeopleInside);
}
```

- **6.** 声明 `add()` 函数，它接受 `Person` 类型的右值参数，锁定互斥锁，并使用 `std::move()` 函数将 `Person` 添加到向量中：

```cpp
void Persons::add(Person&& person)
{
    std::lock_guard<std::mutex> m_lock(m_Mutex);
    m_Persons.emplace_back(std::move(person));
}
```

- **7.** 声明 `get()` 函数，它锁定互斥锁并返回最后一个元素然后将其从向量中移除。如果向量为空，则会抛出异常：

```cpp
Person Persons::get()
{
    std::lock_guard<std::mutex> m_lock(m_Mutex);
    if (m_Persons.empty())
    {
        throw "Empty Persons storage";
    }

    Person result = std::move(m_Persons.back());
    m_Persons.pop_back();
    return result;
}
```

- **8.** 声明 `size()` 函数，它返回向量的大小：

```cpp
size_t Persons::size() const
{
    return m_Persons.size();
}
```

- **9.** 最后，声明 `removePerson()` 函数，它锁定互斥锁并从向量中移除最后一项：

```cpp
void Persons::removePerson()
{
    std::lock_guard<std::mutex> m_lock(m_Mutex);
    m_Persons.pop_back();
    std::cout << "Persons | removePerson | removed" << std::endl;
}
```

- **10.** 现在，实现 `PersonGenerator` 类，它负责创建和移除 `Person` 项。创建相应的头文件和源文件，即 `PersonGenerator.hpp` 和 `PersonGenerator.cpp`。同时，添加包含保护。包含 `"Persons.hpp"`、`<thread>` 和 `<condition_variable>` 头文件。定义 `PersonGenerator` 类。在私有部分，定义两个 `std::thread` 变量，分别为 `m_CreateThread` 和 `m_RemoveThread`。在一个线程中，我们将创建新的 `Person` 对象，并在另一个线程中异步通知用户移除 `Person` 对象。定义一个对 `Persons` 类型共享变量的引用，即 `m_CreatedPersons`。我们将把每个新人员放入其中。`m_CreatedPersons` 将在多个线程之间共享。定义两个对 `std::condition_variable` 的引用，即 `m_CondVarAddPerson` 和 `m_CondVarRemovePerson`。它们将用于线程间的通信。定义两个对 `std::mutex` 变量的引用，即 `m_AddLock` 和 `m_RemoveLock`。它们将用于接收对条件变量的访问。最后，定义两个对 `bool` 值的引用，即 `m_AddNotified` 和 `m_RemoveNotified`。它们将用于检查通知是否为真或假。此外，在私有部分，定义两个将作为线程启动函数的函数 – `runCreating()` 和 `runRemoving()`。接下来，定义两个将触发条件变量的函数，即 `notifyCreated()` 和 `notifyRemoved()`。在公共部分，定义一个构造函数，它将我们在私有部分定义的所有引用作为参数。最后，定义一个析构函数。这将确保其他默认生成的函数被删除：

```cpp
#ifndef PERSON_GENERATOR_HPP
#define PERSON_GENERATOR_HPP

#include "Persons.hpp"
#include <condition_variable>
#include <thread>

class PersonGenerator
{
public:
    PersonGenerator(Persons& persons,
```

- **11.** 现在，转到源文件。包含 `<stdlib.h>` 文件，以便我们可以访问用于随机数生成的 `srand()` 和 `rand()` 函数。包含 `<time.h>` 头文件，以便我们可以访问 `time()` 函数，以及 `std::chrono` 命名空间。它们用于我们在处理时间时使用。包含 `<ratio>` 文件，它用于 typedef，以便我们可以与时间库一起工作：

```cpp
#include "PersonGenerator.hpp"
#include <iostream>
#include <stdlib.h>    /* srand, rand */
#include <time.h>      /* time */
#include <chrono>      /* chrono */
#include <ratio>       /* std::milli */
```

- **12.** 声明构造函数并在初始化列表中初始化除线程之外的所有参数。在构造函数体中用适当的函数初始化线程：

```cpp
PersonGenerator::PersonGenerator(Persons& persons,
    std::condition_variable& add_person,
    std::condition_variable& remove_person,
    std::mutex& add_lock,
    std::mutex& remove_lock,
    bool& addNotified,
    bool& removeNotified)
: m_CreatedPersons(persons)
, m_CondVarAddPerson(add_person)
, m_AddLock(add_lock)
, m_AddNotified(addNotified)
, m_CondVarRemovePerson(remove_person)
, m_RemoveLock(remove_lock)
, m_RemoveNotified(removeNotified)
{
    m_CreateThread = std::thread(&PersonGenerator::runCreating, this);
    m_RemoveThread = std::thread(&PersonGenerator::runRemoving, this);
}
```

- **13.** 声明析构函数并检查线程是否可连接。如果不可连接则连接它们：

```cpp
PersonGenerator::~PersonGenerator()
{
    if (m_CreateThread.joinable())
    {
        m_CreateThread.join();
    }
    if (m_RemoveThread.joinable())
    {
        m_RemoveThread.join();
    }
}
```

- **14.** 声明 `runCreating()` 函数，它是 `m_CreateThread` 线程的启动函数。在此函数中，在无限循环中，我们将生成 1 到 10 之间的随机数并使当前线程休眠此时间。之后，创建一个 `Person` 值，将其添加到共享容器中，并通知其他线程：

```cpp
void PersonGenerator::runCreating()
{
    using namespace std::chrono_literals;
    srand(time(NULL));
    while (true)
    {
        std::chrono::duration<int, std::milli> duration((rand() % 10 + 1) * 1000);
        std::this_thread::sleep_for(duration);
        std::cout << "PersonGenerator | runCreating | new person:" << std::endl;
        m_CreatedPersons.add(std::move(Person()));
        notifyCreated();
    }
}
```

- **15.** 声明 `runRemoving()` 函数，它是 `m_RemoveThread` 线程的启动函数。在此函数中，在无限循环中，我们将生成 20 到 30 之间的随机数并使当前线程休眠此时间。之后，通知其他线程应移除部分访客：

```cpp
void PersonGenerator::runRemoving()
{
    using namespace std::chrono_literals;
    srand(time(NULL));
    while (true)
    {
        std::chrono::duration<int, std::milli> duration((rand() % 10 + 20) * 1000);
        std::this_thread::sleep_for(duration);
        std::cout << "PersonGenerator | runRemoving | somebody has left the gallery:" << std::endl;
    }
}
```

```cpp
#ifndef WATCHMAN_HPP
#define WATCHMAN_HPP
#include <mutex>
#include <thread>
#include <condition_variable>
#include "Persons.hpp"
class Watchman
{
public:
    Watchman(std::condition_variable&,
             std::condition_variable&,
             std::mutex&,
             std::mutex&,
             bool&,
             bool&,
             Persons&,
             Persons&,
             Persons&);
    ~Watchman();
    Watchman(const Watchman&) = delete;
    Watchman(Watchman&&) = delete;
    Watchman& operator=(const Watchman&) = delete;
    Watchman& operator=(Watchman&&) = delete;
private:
    void runAdd();
    void runRemove();
private:
    std::thread m_ThreadAdd;
    std::thread m_ThreadRemove;
    std::condition_variable& m_CondVarRemovePerson;
    std::condition_variable& m_CondVarAddPerson;
    std::mutex& m_AddMux;
    std::mutex& m_RemoveMux;
    bool& m_AddNotified;
    bool& m_RemoveNotified;
    Persons& m_PeopleInside;
    Persons& m_PeopleInQueue;
    Persons& m_CreatedPeople;
};
#endif // WATCHMAN_HPP
```

- **18.** 现在，转到源文件。包含 `"Common.hpp"` 头文件，以便我们可以访问 `CountPeopleInside` 变量和其他必要的头文件：

```cpp
#include "Watchman.hpp"
#include "Common.hpp"
#include <iostream>
```

- **19.** 声明构造函数并在初始化列表中初始化除线程之外的所有参数。在构造函数体中用适当的函数初始化线程：

```cpp
Watchman::Watchman(std::condition_variable& addPerson,
    std::condition_variable& removePerson,
    std::mutex& addMux,
    std::mutex& removeMux,
    bool& addNotified,
    bool& removeNotified,
    Persons& peopleInside,
    Persons& peopleInQueue,
    Persons& createdPeople)
: m_CondVarRemovePerson(removePerson)
, m_CondVarAddPerson(addPerson)
, m_AddMux(addMux)
, m_RemoveMux(removeMux)
, m_AddNotified(addNotified)
, m_RemoveNotified(removeNotified)
, m_PeopleInside(peopleInside)
, m_PeopleInQueue(peopleInQueue)
, m_CreatedPeople(createdPeople)
{
    m_ThreadAdd = std::thread(&Watchman::runAdd, this);
    m_ThreadRemove = std::thread(&Watchman::runRemove, this);
}
```

- **20.** 声明析构函数并检查线程是否可连接。如果不可连接则连接它们：

```cpp
Watchman::~Watchman()
{
    if (m_ThreadAdd.joinable())
    {
        m_ThreadAdd.join();
    }
    if (m_ThreadRemove.joinable())
    {
```

