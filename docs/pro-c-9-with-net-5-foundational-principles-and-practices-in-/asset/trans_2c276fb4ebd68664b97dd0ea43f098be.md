
在这里，你可以使用 `Queue<T>` 类的 `Enqueue()` 方法向其中插入三个项。调用 `Peek()` 可以查看（但不移除）当前队列中的第一项。最后，调用 `Dequeue()` 会将该项从队列中移除，并将其送入 `GetCoffee()` 辅助函数进行处理。请注意，如果试图从空队列中移除项，将会抛出运行时异常。以下是调用此方法时得到的输出：

```
***** Fun with Generic Collections *****
Homer is first in line!
Homer got coffee!
Marge got coffee!
Lisa got coffee!
Error! Queue empty.
```

## 使用 SortedSet<T> 类

`SortedSet<T>` 类非常有用，因为它会在插入或移除项时自动确保集合中的项保持排序。不过，你需要通过构造函数参数传入一个实现了泛型 `IComparer<T>` 接口的对象，来明确告知 `SortedSet<T>` 类希望如何对对象进行排序。

首先创建一个名为 `SortPeopleByAge` 的新类，它实现 `IComparer<T>`，其中 T 的类型为 Person。回顾一下，该接口定义了一个名为 `Compare()` 的方法，你可以在其中编写所需的任意比较逻辑。以下是该类的简单实现：

```
using System.Collections.Generic;

namespace FunWithGenericCollections
{
    class SortPeopleByAge : IComparer<Person>
    {
        public int Compare(Person firstPerson, Person secondPerson)
        {
            if (firstPerson?.Age > secondPerson?.Age)
            {
                return 1;
            }
            if (firstPerson?.Age < secondPerson?.Age)
            {
                return -1;
            }
            return 0;
        }
    }
}
```

现在添加下面这个新方法，演示如何使用 `SortedSet<Person>`：

```
static void UseSortedSet()
{
    // Make some people with different ages.
    SortedSet<Person> setOfPeople = new SortedSet<Person>(new SortPeopleByAge())
    {
        new Person {FirstName= "Homer", LastName="Simpson", Age=47},
        new Person {FirstName= "Marge", LastName="Simpson", Age=45},
        new Person {FirstName= "Lisa", LastName="Simpson", Age=9},
        new Person {FirstName= "Bart", LastName="Simpson", Age=8}
    };

    // Note the items are sorted by age!
    foreach (Person p in setOfPeople)
    {
        Console.WriteLine(p);
    }
    Console.WriteLine();

    // Add a few new people, with various ages.
    setOfPeople.Add(new Person { FirstName = "Saku", LastName = "Jones", Age = 1 });
    setOfPeople.Add(new Person { FirstName = "Mikko", LastName = "Jones", Age = 32 });

    // Still sorted by age!
    foreach (Person p in setOfPeople)
    {
        Console.WriteLine(p);
    }
}
```

当你运行应用程序时，对象列表将始终根据 `Age` 属性的值进行排序，无论你插入或移除对象的顺序如何。

```
***** Fun with Generic Collections *****
Name: Bart Simpson, Age: 8
Name: Lisa Simpson, Age: 9
Name: Marge Simpson, Age: 45
Name: Homer Simpson, Age: 47

Name: Saku Jones, Age: 1
Name: Bart Simpson, Age: 8
Name: Lisa Simpson, Age: 9
Name: Mikko Jones, Age: 32
Name: Marge Simpson, Age: 45
Name: Homer Simpson, Age: 47
```

## 使用 Dictionary<TKey, TValue> 类

另一个实用的泛型集合是 `Dictionary<TKey, TValue>` 类型，它允许你保存任意数量的对象，并通过唯一键来引用这些对象。因此，与其使用数字标识符从 `List<T>` 中获取项（例如“给我第二个对象”），你可以使用唯一的文本键（例如“给我键为 Homer 的对象”）。

与其他集合对象类似，你可以手动调用泛型 `Add()` 方法来填充 `Dictionary<TKey, TValue>`。不过，你也可以使用集合初始化语法来填充 `Dictionary<TKey, TValue>`。请注意，在填充此集合对象时，键名必须唯一。如果你错误地多次指定相同的键，将会收到运行时异常。

考虑下面这个方法，它用各种对象填充 `Dictionary<K, V>`。注意，创建 `Dictionary<TKey, TValue>` 对象时，需要将键类型（TKey）和基础对象类型（TValue）作为构造函数参数指定。在本例中，你使用 `string` 数据类型作为键，使用 `Person` 类型作为值。另外请注意，你可以将对象初始化语法与集合初始化语法结合起来。

```
private static void UseDictionary()
{
    // Populate using Add() method
    Dictionary<string, Person> peopleA = new Dictionary<string, Person>();
    peopleA.Add("Homer", new Person { FirstName = "Homer", LastName = "Simpson", Age = 47 });
    peopleA.Add("Marge", new Person { FirstName = "Marge", LastName = "Simpson", Age = 45 });
    peopleA.Add("Lisa", new Person { FirstName = "Lisa", LastName = "Simpson", Age = 9 });

    // Get Homer.
    Person homer = peopleA["Homer"];
    Console.WriteLine(homer);

    // Populate with initialization syntax.
    Dictionary<string, Person> peopleB = new Dictionary<string, Person>()
    {
        { "Homer", new Person { FirstName = "Homer", LastName = "Simpson", Age = 47 } },
        { "Marge", new Person { FirstName = "Marge", LastName = "Simpson", Age = 45 } },
        { "Lisa", new Person { FirstName = "Lisa", LastName = "Simpson", Age = 9 } }
    };

    // Get Lisa.
    Person lisa = peopleB["Lisa"];
    Console.WriteLine(lisa);
}
```

还可以使用一种与此类容器相关的初始化语法来填充 `Dictionary<TKey, TValue>`（不出意料，这被称为字典初始化）。与上一个代码示例中填充 `personB` 对象的语法类似，你仍然需要为集合对象定义一个初始化范围；不过，你可以使用索引器指定键，并将其赋值给一个新对象，如下所示：

```
// Populate with dictionary initialization syntax.
Dictionary<string, Person> peopleC = new Dictionary<string, Person>()
{
    ["Homer"] = new Person { FirstName = "Homer", LastName = "Simpson", Age = 47 },
    ["Marge"] = new Person { FirstName = "Marge", LastName = "Simpson", Age = 45 },
    ["Lisa"] = new Person { FirstName = "Lisa", LastName = "Simpson", Age = 9 }
};
```

## System.Collections.ObjectModel 命名空间

现在你已经了解了如何使用主要的泛型类，接下来我们将简要介绍另一个以集合为中心的命名空间：`System.Collections.ObjectModel`。这是一个相对较小的命名空间，包含少数几个类。表 10-7 列出了你绝对应该了解的两个类。

### 表 10-7. System.Collections.ObjectModel 的有用成员

| System.Collections.ObjectModel 类型 | 含义 |
|---|---|
| ObservableCollection<T> | 表示一个动态数据集合，当添加项、移除项或刷新整个列表时提供通知 |
| ReadOnlyObservableCollection<T> | 表示 ObservableCollection<T> 的只读版本 |

`ObservableCollection<T>` 类的用处在于，它能够在内容以某种方式发生变化时通知外部对象（正如你可能猜到的，`ReadOnlyObservableCollection<T>` 的用法类似，但本质上是只读的）。

## 使用 ObservableCollection<T>

创建一个名为 `FunWithObservableCollections` 的新控制台应用程序项目，并将 `System.Collections.ObjectModel` 命名空间导入到你的初始 C# 代码文件中。在很多方面，使用 `ObservableCollection<T>` 与使用 `List<T>` 是相同的，因为这两个类都实现了相同的核心接口。`ObservableCollection<T>` 类的独特之处在于，该类支持一个名为 `CollectionChanged` 的事件。每当插入新项、移除（或重新定位）当前项或修改整个集合时，该事件都会触发。

与任何事件一样，`CollectionChanged` 是通过委托定义的，在本例中委托是 `NotifyCollectionChangedEventHandler`。该委托可以调用任何以 `object` 作为第一个参数、以 `NotifyCollectionChangedEventArgs` 作为第二个参数的方法。请考虑以下代码，它填充了一个包含 `Person` 对象的可观察集合，并连接了 `CollectionChanged` 事件：

```
using System;
using System.Collections.ObjectModel;
using System.Collections.Specialized;
using FunWithObservableCollections;

// Make a collection to observe
//and add a few Person objects.
ObservableCollection<Person> people = new ObservableCollection<Person>()
{
    new Person{ FirstName = "Peter", LastName = "Murphy", Age = 52 },
    new Person{ FirstName = "Kevin", LastName = "Key", Age = 48 },
};

// Wire up the CollectionChanged event.
people.CollectionChanged += people_CollectionChanged;
```
