

| 64 | 32 | 16 | 8 | 4 | 2 | 1 |
|---|---|---|---|---|---|---|
| 0 | 0 | 1 | 0 | 0 | 1 | 0 |

2. 在 Person 类中，添加以下语句到你的字段列表中，如以下代码所示：

```
public WondersOfTheAncientWorld BucketList;
```

3. 在 Program.cs 中，添加语句使用 | 运算符（按位逻辑或）来组合枚举值设置桶列表。我们也可以使用数字 18 强制转换为枚举类型来设置值，如注释所示，但我们不应该这样做，因为那样会使代码更难理解，如以下代码所示：

```
bob.BucketList =
    WondersOfTheAncientWorld.HangingGardensOfBabylon
  | WondersOfTheAncientWorld.MausoleumAtHalicarnassus;

// bob.BucketList = (WondersOfTheAncientWorld)18;

WriteLine($"{bob.Name}'s bucket list is {bob.BucketList}");
```

4. 运行代码并查看结果，如以下输出所示：

```
Bob Smith's bucket list is HangingGardensOfBabylon,
MausoleumAtHalicarnassus
```

![](img/c63b00dca7192cc58cfb9e1f1e64f483_219_0.png)

> **Good Practice:** 使用枚举值来存储离散选项的组合。如果选项最多为八个，则从 byte 派生枚举类型；如果最多为 16 个，则从 ushort 派生；如果最多为 32 个，则从 uint 派生；如果最多为 64 个，则从 ulong 派生。

## 使用集合存储多个值

现在让我们添加一个字段来存储一个人的孩子。这是一个聚合的示例，因为孩子是与当前人相关的类的实例，但不是人本身的一部分。我们将使用一个通用的 List<T> 集合类型，它可以存储任何类型的有序集合。你将在第 8 章《使用常见的 .NET 类型》中了解更多关于集合的内容。现在，只需跟随操作：

1. 在 Person.cs 中，导入 System.Collections.Generic 命名空间，如以下代码所示：

```
using System.Collections.Generic; // List<T>
```

2. 在 Person 类中声明一个新字段，如以下代码所示：

```
public List<Person> Children = new List<Person>();
```

List<Person> 读作“Person 的列表”，例如，“名为 Children 的属性的类型是 Person 实例的列表。”我们明确将类库更改为以 .NET Standard 2.0 为目标（使用 C# 7 编译器），因此我们不能使用目标类型 new 来初始化 Children 字段。如果我们将其保持为以 .NET 6.0 为目标，则可以使用目标类型 new，如以下代码所示：

```
public List<Person> Children = new();
```

我们必须确保在向其中添加项目之前将集合初始化为 Person 列表的新实例，否则该字段将为 null，并且在尝试使用其成员（如 Add）时将抛出运行时异常。

## 理解泛型集合

List<T> 类型中的尖括号是 C# 的一个特性，称为泛型，于 2005 年随 C# 2.0 引入。这是一个花哨的术语，用于使集合强类型化，即编译器知道可以存储在集合中的对象的具体类型。泛型可以提高代码的性能和正确性。

强类型与静态类型具有不同的含义。旧的 System.Collections 类型是静态类型化以包含弱类型的 System.Object 项。新的 System.Collections.Generic 类型是静态类型化以包含强类型的 <T> 实例。

具有讽刺意味的是，泛型这个术语意味着我们可以使用更具体的静态类型！

- 1. 在 Program.cs 中，添加语句为 Bob 添加两个孩子，然后显示他有多少个孩子以及他们的名字，如以下代码所示：

```
bob.Children.Add(new Person { Name = "Alfred" }); // C# 3.0 and Later
bob.Children.Add(new() { Name = "Zoe" }); // C# 9.0 and Later

WriteLine(
    $"{bob.Name} has {bob.Children.Count} children:");

for (int childIndex = 0; childIndex < bob.Children.Count; childIndex++)
{
    WriteLine($"    {bob.Children[childIndex].Name}");
}
```

我们也可以使用 foreach 语句来枚举集合。作为额外挑战，将 for 语句更改为使用 foreach 输出相同的信息。

- 2. 运行代码并查看结果，如以下输出所示：

```
Bob Smith has 2 children:
    Alfred
    Zoe
```

## 使字段为静态

到目前为止我们创建的所有字段都是实例成员，这意味着为创建的类的每个实例创建每个字段的不同值。alice 和 bob 变量具有不同的 Name 值。

有时，你希望定义一个字段，该字段只有一个值在所有实例之间共享。

这些称为静态成员，因为字段并不是唯一可以是静态的成员。让我们看看使用静态字段可以实现什么：

- 1. 在 PacktLibrary 项目中，添加一个名为 BankAccount.cs 的新类文件。
- 2. 修改类以赋予它三个字段，两个实例字段和一个静态字段，如以下代码所示：

```
namespace Packt.Shared
{
    public class BankAccount
    {
        public string AccountName; // instance member
        public decimal Balance; // instance member
        public static decimal InterestRate; // shared member
    }
}
```

BankAccount 的每个实例将拥有自己的 AccountName 和 Balance 值，但所有实例将共享单个 InterestRate 值。

- 3. 在 Program.cs 中，添加语句来设置共享利率，然后创建 BankAccount 类型的两个实例，如以下代码所示：

```
BankAccount.InterestRate = 0.012M; // store a shared value

BankAccount jonesAccount = new(); // C# 9.0 and later
jonesAccount.AccountName = "Mrs. Jones";
jonesAccount.Balance = 2400;

WriteLine(format: "{0} earned {1:C} interest.",
    arg0: jonesAccount.AccountName,
    arg1: jonesAccount.Balance * BankAccount.InterestRate);

BankAccount gerrierAccount = new();
gerrierAccount.AccountName = "Ms. Gerrier";
gerrierAccount.Balance = 98;

WriteLine(format: "{0} earned {1:C} interest.",
    arg0: gerrierAccount.AccountName,
    arg1: gerrierAccount.Balance * BankAccount.InterestRate);
```

:C 是一个格式代码，告诉 .NET 对数字使用货币格式。在第 8 章《使用常见的 .NET 类型》中，你将学习如何控制决定货币符号的区域性。对于现在，它将使用操作系统安装的默认值。我住在英国伦敦，因此我的输出显示英镑 (£)。

4. 运行代码并查看额外输出：

```
Mrs. Jones earned £28.80 interest.
Ms. Gerrier earned £1.18 interest.
```

> 字段并不是唯一可以是静态的成员。构造函数、方法、属性和其他成员也可以是静态的。

## 使字段成为常量

如果字段的值永远不会改变，你可以使用 const 关键字并在编译时分配字面值：

- 1. 在 Person.cs 中，添加以下代码：
- 2. 要获取常量字段的值，你必须写类的名称，而不是类的实例的名称。在 Program.cs 中，添加一条语句将 Bob 的姓名和物种写入控制台，如以下代码所示：
- 3. 运行代码并查看结果，如以下输出所示：

```
// constants
public const string Species = "Homo Sapien";
```

```
WriteLine($"{bob.Name} is a {Person.Species}");
```

```
Bob Smith is a Homo Sapien
```

Microsoft 类型中 const 字段的示例包括 System.Int32.MaxValue 和 System.Math.PI，因为这两个值永远不会改变，如图 5.2 所示：

![](img/c63b00dca7192cc58cfb9e1f1e64f483_223_0.png)

图 5.2：常量示例

![](img/c63b00dca7192cc58cfb9e1f1e64f483_223_1.png)

> Good Practice: 常量并不总是最佳选择，原因有两个重要原因：值必须在编译时已知，并且必须能够表示为字面字符串、布尔值或数值。const 字段的每个引用在编译时都会被字面值替换，因此，如果值在将来版本中发生变化，并且你没有重新编译引用它的任何程序集以获取新值，则不会反映该变化。

## 使字段只读

通常，更好的选择是为不应更改的字段标记为只读：

- 1. 在 Person.cs 中，添加一条语句来声明一个实例只读字段以存储个人的家乡星球，如以下代码所示：
- 2. 在 Program.cs 中，添加一条语句将 Bob 的姓名和家乡星球写入控制台，如以下代码所示：
- 3. 运行代码并查看结果，如以下输出所示：

```
// read-only fields
public readonly string HomePlanet = "Earth";
```

```
WriteLine($"{bob.Name} was born on {bob.HomePlanet}");
```

Bob Smith was born on Earth

