

与任何类一样，System.Object 定义了一组成员。在下面的正式 C# 定义中，请注意其中一些成员被声明为 virtual，这表示某个成员可能被子类重写，而另一些则被标记为 static（因此是在类级别调用的）：

```
public class Object
{
    // Virtual members.
    public virtual bool Equals(object obj);
    protected virtual void Finalize();
    public virtual int GetHashCode();
    public virtual string ToString();

    // Instance-level, nonvirtual members.
    public Type GetType();
    protected object MemberwiseClone();

    // Static members.
    public static bool Equals(object objA, object objB);
    public static bool ReferenceEquals(object objA, object objB);
}
```

表 6-1 概述了其中一些你最可能用到的方法所提供的功能。

## 表 6-1. System.Object 的核心成员

| Object 类的实例方法 | 含义 |
|---|---|
| Equals() | 默认情况下，仅当被比较的项目引用内存中的同一项目时，此方法才返回 true。因此，Equals() 用于比较对象引用，而不是对象的状态。通常，重写此方法后，仅当被比较的对象具有相同的内部状态值（即基于值的语义）时才返回 true。<br><br>请注意，如果你重写了 Equals()，还应该重写 GetHashCode()，因为这些方法在内部被 Hashtable 类型用来从容器中检索子对象。<br><br>还要记得第 4 章提到过，ValueType 类为所有结构重写了此方法，因此它们可以进行基于值的比较。 |
| Finalize() | 目前，你可以将此方法（在被重写时）理解为在对象被销毁之前被调用，以释放所有已分配的资源。我将在第 9 章中详细介绍 CoreCLR 垃圾回收服务。 |
| GetHashCode() | 此方法返回一个 int，用于标识特定的对象实例。 |
| ToString() | 此方法使用 `<namespace>.<type name>` 格式（称为完全限定名）返回此对象的字符串表示形式。子类通常会重写此方法，以返回表示对象内部状态的名称-值对标记字符串，而不是其完全限定名。 |
| GetType() | 此方法返回一个 Type 对象，该对象完整描述了你当前引用的对象。简而言之，这是一个适用于所有对象的运行时类型标识（RTTI）方法（将在第 16 章中详细讨论）。 |
| MemberwiseClone() | 此方法用于返回当前对象的逐成员副本，通常在克隆对象时使用（参见第 8 章）。 |

为了演示 Object 基类提供的某些默认行为，最后，请创建一个名为 ObjectOverrides 的 C# 控制台应用程序项目。插入一个新的 C# 类类型，其中包含针对名为 Person 的类型的以下空类定义：

```
// Remember! Person extends Object.
class Person {}
```

现在，更新你的顶级语句，以与 System.Object 的继承成员进行交互，如下所示：

```
Console.WriteLine("***** Fun with System.Object *****
");
Person p1 = new Person();

// Use inherited members of System.Object.
Console.WriteLine("ToString: {0}", p1.ToString());
Console.WriteLine("Hash code: {0}", p1.GetHashCode());
Console.WriteLine("Type: {0}", p1.GetType());

// Make some other references to p1.
Person p2 = p1;
object o = p2;
// Are the references pointing to the same object in memory?
if (o.Equals(p1) && p2.Equals(o))
{
    Console.WriteLine("Same instance!");
}
Console.ReadLine();
}
```

以下是当前代码的输出：

```
***** Fun with System.Object *****
ToString: ObjectOverrides.Person
Hash code: 58225482
Type: ObjectOverrides.Person
Same instance!
```

请注意，ToString() 的默认实现返回当前类型的完全限定名（ObjectOverrides.Person）。在第 15 章研究构建自定义命名空间时，你将看到，每个 C# 项目都定义一个“根命名空间”，它与项目本身的名称相同。在这里，你创建了一个名为 ObjectOverrides 的项目；因此，Person 类型和 Program 类都被放在 ObjectOverrides 命名空间内。

Equals() 的默认行为是测试两个变量是否指向内存中的同一个对象。在这里，你创建了一个名为 p1 的新 Person 变量。此时，一个新的 Person 对象被放置在托管堆上。p2 也是 Person 类型。但是，你并不是在创建新实例，而是将此变量赋值为引用 p1。因此，p1 和 p2 都指向内存中的同一个对象，变量 o（object 类型，是为了保险起见而加上的）也是如此。鉴于 p1、p2 和 o 都指向同一个内存位置，相等性测试成功。

尽管 System.Object 的预设行为在许多情况下能够满足要求，但自定义类型重写某些继承方法也是很常见的。为了说明这一点，请更新 Person 类，使其支持一些表示个人名字、姓氏和年龄的属性，每个属性都可以通过自定义构造函数进行设置。

```
// Remember! Person extends Object.
class Person
{
    public string FirstName { get; set; } = "";
    public string LastName { get; set; } = "";
    public int Age { get; set; }

    public Person(string fName, string lName, int personAge)
    {
        FirstName = fName;
        LastName = lName;
        Age = personAge;
    }
    public Person(){}
}
```

## 重写 System.Object.ToString()

你创建的许多类（和结构）都可以通过重写 ToString() 来返回类型当前状态的字符串文本表示形式，从而受益。这对于调试（以及其他原因）非常有帮助。如何构造此字符串是个人选择的问题；但是，推荐的方法是用分号分隔每个名称-值对，并将整个字符串放在方括号内（.NET Core 基类库中的许多类型都遵循这种方法）。请考虑为 Person 类重写 ToString()，如下所示：

```
public override string ToString() => $"[First Name: {FirstName}; Last Name: {LastName}; Age: {Age}]";
```

鉴于 Person 类只有三项状态数据，这个 ToString() 的实现相当简单直接。但是，请始终记住，一个正确的 ToString() 重写还应该考虑继承链上定义的任何数据。

当你为一个继承自定义基类的类重写 ToString() 时，首要任务是通过 base 关键字从父类获取 ToString() 值。获得父类的字符串数据后，你可以追加派生类的自定义信息。

## 重写 System.Object.Equals()

让我们也重写 Object.Equals() 的行为，使其基于值的语义进行工作。回想一下，默认情况下，Equals() 仅当被比较的两个对象引用内存中的同一个对象实例时才返回 true。对于 Person 类，如果被比较的两个变量包含相同的状态值（例如名字、姓氏和年龄），那么实现 Equals() 使其返回 true 可能会很有帮助。

首先，请注意 Equals() 方法的传入参数是一个通用的 System.Object。因此，你的首要任务是确保调用者确实传入了一个 Person 对象，并且作为额外的保障，确保传入的参数不是空引用。

在确认调用者已向你传入一个已分配的 Person 后，实现 Equals() 的一种方法是对传入对象的数据与当前对象的数据进行逐字段比较。

```
public override bool Equals(object obj)
{
    if (!(obj is Person temp))
    {
        return false;
    }
    if (temp.FirstName == this.FirstName
        && temp.LastName == this.LastName
        && temp.Age == this.Age)
    {
        return true;
    }
    return false;
}
```

在这里，你正在将传入对象的值与内部的值进行比较（注意 this 关键字的使用）。如果两者的名字和年龄完全相同，那么你就有两个具有相同状态数据的对象，因此返回 true。任何其他可能性都会导致返回 false。

虽然这种方法确实有效，但你肯定可以想象，对于可能包含数十个数据字段的非平凡类型，实现自定义 Equals() 方法将多么耗费人力。一种常见的捷径是利用你自己对 ToString() 的实现。如果一个类有一个恰当的 ToString() 实现，并且考虑了继承链上的所有字段数据，那么你可以直接比较对象的字符串数据（检查是否为空）。

```
// No need to cast "obj" to a Person anymore,
// as everything has a ToString() method.
public override bool Equals(object obj)
    => obj?.ToString() == ToString();
```

请注意，在这种情况下，你不再需要检查传入参数的类型是否正确（在此示例中是 Person），因为 .NET 中的所有内容都支持 ToString() 方法。更好的是，你不再需要逐属性进行相等性检查，因为你现在只需测试 ToString() 返回的值。

## 重写 System.Object.GetHashCode()

当类重写 Equals() 方法时，你还应该重写 GetHashCode() 的默认实现。简而言之，哈希码是一个数值，表示处于特定状态的对象。例如，如果你创建两个保存值 Hello 的字符串变量，你将获得相同的哈希码。但是，如果其中一个字符串对象全是小写（hello），你将获得不同的哈希码。

默认情况下，System.Object.GetHashCode() 使用对象在内存中的当前位置来生成哈希值。但是，如果你要构建一个打算存储在 Hashtable 类型（在 System.Collections 命名空间内）中的自定义类型，则应该始终重写此成员，因为 Hashtable 将在内部调用 Equals() 和 GetHashCode() 来检索正确的对象。

注意：更具体地说，System.Collections.Hashtable 类在内部调用 GetHashCode() 来大致了解对象所在的位置，但随后（在内部）调用 Equals() 来确定精确匹配。

