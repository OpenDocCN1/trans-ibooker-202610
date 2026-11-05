
这个新的 C# 语言特性要产生影响需要多年时间，因为成千上万的现有库包和应用程序仍期望旧的行为。即使是微软，也直到 .NET 6 才有时间在所有主要 .NET 包中完全实现此新特性。

过渡期间，您可以为自己的项目在以下几种方法中选择：

- **默认**：无需更改。不支持可空引用类型。
- **项目级选择加入，文件级选择退出**：在项目级别启用该功能，对于需要保持与旧行为兼容的任何文件，选择退出。微软在更新自己的包以使用此新功能时内部使用的方法就是这种方法。
- **文件级选择加入**：仅为单个文件启用该功能。

## 启用可空和不可空引用类型

要在项目级别启用该功能，请在项目文件中添加以下内容：

```xml
<PropertyGroup>
  ...
  <Nullable>enable</Nullable>
</PropertyGroup>
```

现在在以 .NET 6.0 为目标的项目模板中默认执行此操作。

要在文件级别禁用该功能，请在代码文件顶部添加以下内容：

```
#nullable disable
```

要在文件级别启用该功能，请在代码文件顶部添加以下内容：

```
#nullable enable
```

## 声明不可空变量和参数

如果启用可空引用类型并且您希望引用类型被赋值为 null，则必须使用与使值类型可空相同的语法，即在类型声明后添加 ? 符号。

那么，可空引用类型是如何工作的？让我们看一个例子。在存储地址信息时，您可能希望强制为街道、城市和地区提供值，但建筑物可以留空，即为 null：

1. 在 NullHandling.csproj 中，在 Program.cs 文件底部，添加语句声明一个包含四个字段的 Address 类，如以下代码所示：

```csharp
class Address
{
    public string? Building;
    public string Street;
    public string City;
    public string Region;
}
```

2. 经过几秒钟，注意关于不可空字段的警告，例如 Street 未初始化，如图 6.2 所示：

![](img/c63b00dca7192cc58cfb9e1f1e64f483_279_0.png)

图 6.2：PROBLEMS 窗口中关于不可空字段的警告消息

3. 将空字符串值分配给这三个不可空字段，如以下代码所示：

```csharp
public string Street = string.Empty;
public string City = string.Empty;
public string Region = string.Empty;
```

4. 在 Program.cs 文件顶部静态导入 Console，然后添加语句实例化 Address 并设置其属性，如以下代码所示：

```csharp
Address address = new();
address.Building = null;
address.Street = null;
address.City = "London";
address.Region = null;
```

5. 注意警告，如图 6.3 所示：

![](img/c63b00dca7192cc58cfb9e1f1e64f483_280_0.png)

图 6.3：关于将 null 分配给不可空字段的警告消息

因此，这个新语言特性被称为可空引用类型。从 C# 8.0 开始，未修饰的引用类型可以变为不可空，并且使引用类型可空使用的语法与用于值类型的语法相同。

## 检查 null

检查可空引用类型或可空值类型变量当前是否包含 null 很重要，因为如果不检查，可能会抛出 NullReferenceException，导致错误。在使用可空变量之前，您应该检查 null 值，如以下代码所示：

```csharp
// check that the variable is not null before using it
if (thisCouldBeNull != null)
{
    // access a member of thisCouldBeNull
    int length = thisCouldBeNull.Length; // could throw exception
    ...
}
```

C# 7 引入了 is 与 !（非）运算符结合作为 != 的替代方案，如以下代码所示：

```csharp
if (!(thisCouldBeNull is null))
{
```

C# 9 引入了 is not 作为更清晰的替代方案，如以下代码所示：

```csharp
if (thisCouldBeNull is not null)
{
```

如果您尝试使用可能为 null 的变量的成员，请使用空条件运算符 ?., 如以下代码所示：

```csharp
string authorName = null;

// the following throws a NullReferenceException
int x = authorName.Length;

// instead of throwing an exception, null is assigned to y
int? y = authorName?.Length;
```

有时您希望将变量赋值为结果，或者如果变量为 null 则使用替代值，例如 3。您使用空合并运算符 ?? 来实现此操作，如以下代码所示：

```csharp
// result will be 3 if authorName?.Length is null
int result = authorName?.Length ?? 3;
Console.WriteLine(result);
```

> 良好实践：即使启用可空引用类型，您仍应检查不可空参数是否为 null 并抛出 ArgumentNullException。

## 在方法参数中检查 null

在定义带参数的方法时，检查 null 值是良好的实践。

在 C# 的早期版本中，您必须编写 if 语句来检查 null 参数值，然后为任何为 null 的参数抛出 ArgumentNullException，如以下代码所示：

```csharp
public void Hire(Person manager, Person employee)
{
    if (manager == null)
    {
        throw new ArgumentNullException(nameof(manager));
    }
    if (employee == null)
    {
        throw new ArgumentNullException(nameof(employee));
    }
    ...
}
```

C# 11 可能会引入新的 !! 后缀为您执行此操作，如以下代码所示：

```csharp
public void Hire(Person manager!!, Person employee!!)
{
    ...
}
```

if 语句和异常抛出由系统为您完成。

## 从类继承

我们之前创建的 Person 类型派生（继承）自 object，即 System.Object 的别名。现在，我们将创建一个继承自 Person 的子类：

1. 在 PacktLibrary 项目中，添加一个名为 Employee.cs 的新类文件。
2. 修改其内容以定义一个名为 Employee 的类，该类派生自 Person，如以下代码所示：
3. 在 Program.cs 中，添加语句以创建 Employee 类的实例，如以下代码所示：
4. 运行代码并查看结果，如以下输出所示：

```csharp
using System;

namespace Packt.Shared;

public class Employee : Person
{
}
```

```csharp
Employee john = new()
{
    Name = "John Jones",
    DateOfBirth = new(year: 1990, month: 7, day: 28)
};
john.WriteToConsole();
```

John Jones 出生于星期六。

请注意，Employee 类已继承 Person 的所有成员。

## 扩展类以添加功能

现在，我们将添加一些特定于员工的成员来扩展该类。

1. 在 Employee.cs 中，添加语句定义员工代码和入职日期的两个属性，如以下代码所示：

```csharp
public string? EmployeeCode { get; set; }
public DateTime HireDate { get; set; }
```

2. 在 Program.cs 中，添加语句设置 John 的员工代码和入职日期，如以下代码所示：

```csharp
john.EmployeeCode = "JJ001";
john.HireDate = new(year: 2014, month: 11, day: 23);
WriteLine($"{john.Name} was hired on {john.HireDate:dd/MM/yy}");
```

3. 运行代码并查看结果，如以下输出所示：

```
John Jones was hired on 23/11/14
```

## 隐藏成员

到目前为止，WriteToConsole 方法是从 Person 继承的，它仅输出员工的姓名和出生日期。我们可能希望更改此方法对员工的操作：

1. 在 Employee.cs 中，添加语句重新定义 WriteToConsole 方法，如以下代码高亮所示：

```csharp
using static System.Console;

namespace Packt.Shared;

public class Employee : Person
{
    public string? EmployeeCode { get; set; }
    public DateTime HireDate { get; set; }

    public void WriteToConsole()
    {
        WriteLine(format:
            "{0} was born on {1:dd/MM/yy} and hired on {2:dd/MM/yy}",
            arg0: Name,
            arg1: DateOfBirth,
            arg2: HireDate);
    }
}
```

2. 运行代码并查看结果，如以下输出所示：

```
John Jones was born on 28/07/90 and hired on 01/01/01
John Jones was hired on 23/11/14
```
