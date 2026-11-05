
.NET 中用于处理集合的最常见类型如下表所示：

| 命名空间 | 示例类型 | 描述 |
|---|---|---|
| System.Collections | IEnumerable, IEnumerable<T> | 用于集合的接口和基类。 |
| System.Collections.Generic | List<T>, Dictionary<T>, Queue<T>, Stack<T> | 在 C# 2.0 和 .NET Framework 2.0 中引入。这些集合允许您使用泛型类型参数指定要存储的类型（更安全、更快、更高效）。 |
| System.Collections.Concurrent | BlockingCollection, ConcurrentDictionary, ConcurrentQueue | 这些集合在多线程场景中是安全的。 |
| System.Collections.Immutable | ImmutableArray, ImmutableDictionary, ImmutableList, ImmutableQueue | 设计用于原始集合内容永远不会更改的场景，尽管它们可以作为新实例创建修改后的集合。 |

## 所有集合的通用功能

所有集合都实现 ICollection 接口；这意味着它们必须具有 Count 属性来告诉您其中包含多少个对象，如以下代码所示：

```
namespace System.Collections
{
    public interface ICollection : IEnumerable
    {
        int Count { get; }
        bool IsSynchronized { get; }
        object SyncRoot { get; }
        void CopyTo(Array array, int index);
    }
}
```

例如，如果我们有一个名为 passengers 的集合，我们可以这样做：

```
int howMany = passengers.Count;
```

所有集合都实现 IEnumerable 接口，这意味着它们可以使用 foreach 语句进行迭代。它们必须具有 GetEnumerator 方法，该方法返回实现 IEnumerator 的对象；这意味着返回的对象必须具有用于在集合中导航的 MoveNext 和 Reset 方法，以及包含集合中当前项的 Current 属性，如以下代码所示：

```
namespace System.Collections
{
    public interface IEnumerable
    {
        IEnumerator GetEnumerator();
    }
}
```
```
namespace System.Collections
{
    public interface IEnumerator
    {
        object Current { get; }
        bool MoveNext();
        void Reset();
    }
}
```

例如，要对 passengers 集合中的每个对象执行操作，我们可以编写以下代码：

```
foreach (Passenger p in passengers)
{
    // perform an action on each passenger
}
```

除了基于对象的集合接口外，还有泛型接口和类，其中泛型类型定义存储在集合中的类型，如以下代码所示：

```
namespace System.Collections.Generic
{
    public interface ICollection<T> : IEnumerable<T>, IEnumerable
    {
        int Count { get; }
        bool IsReadOnly { get; }
        void Add(T item);
        void Clear();
        bool Contains(T item);
        void CopyTo(T[] array, int index);
        bool Remove(T item);
    }
}
```

## 通过确保集合的容量来提高性能

自 .NET 1.1 以来，像 StringBuilder 这样的类型就有一个名为 EnsureCapacity 的方法，可以将内部存储数组预先调整为字符串的预期最终大小。这提高了性能，因为在追加更多字符时无需反复增加数组的大小。

自 .NET Core 2.1 以来，像 Dictionary<T> 和 HashSet<T> 这样的类型也具有 EnsureCapacity。

在 .NET 6 及更高版本中，像 List<T>、Queue<T> 和 Stack<T> 这样的集合现在也具有 EnsureCapacity 方法，如以下代码所示：

```
List<string> names = new();
names.EnsureCapacity(10_000);
// Load ten thousand names into the list
```

## 了解集合选择

根据不同的用途，您可以选择多种不同的集合：列表、字典、堆栈、队列、集合，以及许多其他更专业的集合。

## 列表

列表，即实现 IList<T> 的类型，是有序集合，如以下代码所示：

```
namespace System.Collections.Generic
{
    [DefaultMember("Item")] // aka this indexer
    public interface IList<T> : ICollection<T>, IEnumerable<T>, IEnumerable
    {
        T this[int index] { get; set; }
        int IndexOf(T item);
        void Insert(int index, T item);
        void RemoveAt(int index);
    }
}
```

IList<T> 派生自 ICollection<T>，因此它具有 Count 属性，以及一个 Add 方法用于将项目放在集合末尾，还有一个 Insert 方法用于将项目放在列表的指定位置，以及 RemoveAt 用于删除指定位置的项目。

当您想要手动控制集合中项目的顺序时，列表是一个不错的选择。列表中的每个项目都有一个唯一的索引（或位置），该索引会自动分配。项目可以是 T 定义的任何类型，并且项目可以重复。索引是 int 类型，从 0 开始，因此列表中的第一个项目位于索引 0，如下表所示：

| 索引 | 项目 |
|-------|------|
| 0 | London |
| 1 | Paris |
| 2 | London |
| 3 | Sydney |

如果在 London 和 Sydney 之间插入一个新项目（例如 Santiago），则 Sydney 的索引会自动递增。因此，您必须注意，在插入或删除项目后，项目的索引可能会发生变化，如下表所示：

| 索引 | 项目 |
|---|---|
| 0 | London |
| 1 | Paris |
| 2 | London |
| 3 | Santiago |
| 4 | Sydney |

## 字典

当每个值（或对象）都有一个唯一的子值（或人为创建的值）可作为键，以便以后快速在集合中找到该值时，字典是一个不错的选择。键必须是唯一的。例如，如果您存储人员列表，可以选择使用政府颁发的身份号码作为键。

将键想象成现实世界字典中的索引条目。它允许您快速找到单词的定义，因为单词（例如键）按排序顺序保存，如果我们知道要查找 manatee 的定义，我们会跳到字典中间开始查找，因为字母 M 在字母表的中间。

编程中的字典在查找时同样很智能。它们必须实现 IDictionary<TKey, TValue> 接口，如以下代码所示：

```
namespace System.Collections.Generic
{
    [DefaultMember("Item")] // aka this indexer
    public interface IDictionary<TKey, TValue>
        : ICollection<KeyValuePair<TKey, TValue>>,
          IEnumerable<KeyValuePair<TKey, TValue>>, IEnumerable
    {
        TValue this[TKey key] { get; set; }
        ICollection<TKey> Keys { get; }
        ICollection<TValue> Values { get; }
        void Add(TKey key, TValue value);
        bool ContainsKey(TKey key);
        bool Remove(TKey key);
        bool TryGetValue(TKey key, [MaybeNullWhen(false)] out TValue value);
    }
}
```

字典中的项目是 struct 的实例，即值类型 KeyValuePair<TKey, TValue>，其中 TKey 是键的类型，TValue 是值的类型，如以下代码所示：

```
namespace System.Collections.Generic
{
    public readonly struct KeyValuePair<TKey, TValue>
    {
        public KeyValuePair(TKey key, TValue value);
        public TKey Key { get; }
        public TValue Value { get; }
        [EditorBrowsable(EditorBrowsableState.Never)]
        public void Deconstruct(out TKey key, out TValue value);
        public override string ToString();
    }
}
```

例如，Dictionary<string, Person> 使用字符串作为键，使用 Person 实例作为值。Dictionary<string, string> 对两者都使用字符串值，如下表所示：

| 键 | 值 |
|---|---|
| BSA | Bob Smith |
| MW | Max Williams |
| BSB | Bob Smith |
| AM | Amir Mohammed |

## 堆栈

当您想要实现后进先出（LIFO）行为时，堆栈是一个不错的选择。使用堆栈，您只能直接访问或移除堆栈顶部的那一个项目，尽管您可以通过枚举读取整个堆栈中的项目。例如，您不能直接访问堆栈中的第二个项目。

例如，文字处理器使用堆栈来记住您最近执行的操作序列，然后当您按下 Ctrl + Z 时，它会撤消堆栈中的最后一个操作，然后是倒数第二个操作，依此类推。

## 队列

当您想要实现先进先出（FIFO）行为时，队列是一个不错的选择。使用队列，您只能直接访问或移除队列前端的那一个项目，尽管您可以通过枚举读取整个队列中的项目。例如，您不能直接访问队列中的第二个项目。

例如，后台进程使用队列按到达顺序处理工作项，就像人们在邮局排队一样。

.NET 6 引入了 PriorityQueue，其中队列中的每个项目除了其在队列中的位置外，还分配有一个优先级值。
