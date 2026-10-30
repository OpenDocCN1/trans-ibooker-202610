
```
{
    get { return _empSSN; }
}
```

仅具有 getter 的属性也可以使用表达式体成员进行简化。以下行与前面的代码块等价：

```
public string SocialSecurityNumber => _empSSN;
```

现在假设你的类构造函数有一个新参数，让调用方设置对象的 SSN。由于 SocialSecurityNumber 属性是只读的，你不能像这样设置值：

```
public Employee(string name, int age, int id, float pay, string ssn)
{
    Name = name;
    Age = age;
    ID = id;
    Pay = pay;

    // OOPS! This is no longer possible if the property is read only.
    SocialSecurityNumber = ssn;
}
```

除非你愿意将属性重新设计为读写属性，否则对于只读属性，你唯一的选择是在构造函数逻辑中使用底层的 empSSN 成员变量，如下所示：

```
public Employee(string name, int age, int id, float pay, string ssn)
{
    ...
    // Check incoming ssn parameter as required and then set the value.
    empSSN = ssn;
}
```

## 属性 仅写属性

如果你想将属性配置为仅写属性，请省略 get 块，像这样：

```
public int Id
{
    set { _empId = value; }
}
```

## 混合属性上的私有和公共 Get/Set 方法

在定义属性时，get 和 set 方法的访问级别可以不同。重新审视社会安全号码，如果目标是防止类外部修改该号码，那么将 get 方法声明为 public 但将 set 方法声明为 private，像这样：

```
public string SocialSecurityNumber
{
    get => _empSSN;
    private set => _empSSN = value;
}
```

请注意，这将属性从只读更改为读写。区别在于写入对定义类之外的任何内容都是隐藏的。

## 重新审视 static 关键字：定义静态属性

本章前面你已经研究了 static 关键字的角色。现在你了解了 C# 属性语法的用法，你可以正式化静态属性。在本章前面创建的 StaticDataAndMembers 项目中，你的 SavingsAccount 类有两个公共静态方法来获取和设置利率。然而，更标准的做法是将此数据点包装在静态属性中。以下是一个示例，注意 static 关键字的用法：

```
// A simple savings account class.
class SavingsAccount
{
    // Instance-level data.
    public double currBalance;

    // A static point of data.
    private static double _currInterestRate = 0.04;

    // A static property.
    public static double InterestRate
    {
        get { return _currInterestRate; }
        set { _currInterestRate = value; }
    }
    ...
}
```

如果你想用此属性替换之前的静态方法，你可以像这样更新你的代码：

```
// Print the current interest rate via property.
Console.WriteLine("Interest Rate is: {0}", SavingsAccount.InterestRate);
```

## 使用属性模式进行模式匹配（新 8.0）

属性模式使你能够匹配对象的属性。为了设置示例，添加一个新文件 EmployeePayTypeEnum.cs 用于员工支付类型的枚举，如下所示：

```
namespace EmployeeApp
{
    public enum EmployeePayTypeEnum
    {
        Hourly,
        Salaried,
        Commission
    }
}
```

使用构造函数初始化它，为 Employee 类添加一个支付类型属性。相关的代码更改如下所示：

```
private EmployeePayTypeEnum _payType;
public EmployeePayTypeEnum PayType
{
    get => _payType;
    set => _payType = value;
}
public Employee(string name, int id, float pay, string empSsn)
    : this(name,0,id,pay, empSsn, EmployeePayTypeEnum.Salaried)
{
}
public Employee(string name, int age, int id,
    float pay, string empSsn, EmployeePayTypeEnum payType)
{
    Name = name;
    Id = id;
    Age = age;
    Pay = pay;
    SocialSecurityNumber = empSsn;
    PayType = payType;
}
```

现在所有组件都已就位，GiveBonus() 方法可以根据员工的支付类型进行更新。佣金员工获得奖金的 10%，小时工获得按比例计算奖金相当于 40 小时的金额，而薪资员工获得输入的金额。更新后的 GiveBonus() 方法如下所示：

```
public void GiveBonus(float amount)
{
    Pay = this switch
    {
        {PayType: EmployeePayTypeEnum.Commission } => Pay += .10F * amount,
        {PayType: EmployeePayTypeEnum.Hourly } => Pay += 40F * amount/2080F,
        {PayType: EmployeePayTypeEnum.Salaried } => Pay += amount,
        _ => Pay+=0
    };
}
```

与使用模式匹配的其他 switch 语句一样，要么必须有通配符 case 语句，要么如果没有 case 语句匹配，switch 语句必须抛出异常。

要测试此功能，将以下代码添加到顶层语句中：

```
Employee emp = new Employee("Marvin",45,123,1000,"111-11-1111",EmployeePayTypeEnum.Salaried);
Console.WriteLine(emp.Pay);
emp.GiveBonus(100);
Console.WriteLine(emp.Pay);
```

## 理解自动属性

当你构建属性来封装数据时，通常会发现 set 范围内有代码来强制执行程序的业务规则。然而，在某些情况下，你可能不需要任何超出简单获取和设置值的实现逻辑。这意味着你最终会得到大量看起来像以下的代码：

```
// An Employee Car type using standard property
// syntax.
class Car
{
    private string carName = "";
    public string PetName
    {
        get { return carName; }
        set { carName = value; }
    }
}
```

在这些情况下，多次定义私有支持字段和简单的属性定义会变得相当冗长。举例来说，如果你正在建模一个需要九个私有字段数据点的类，你最终会编写九个相关的属性，它们不过是封装服务的薄包装。

为了简化提供字段数据简单封装的过程，你可以使用自动属性语法。顾名思义，此功能将定义私有支持字段和相关的 C# 属性成员的工作交给编译器，使用新的语法片段来完成。为了说明这一点，创建一个名为 AutoProps 的新控制台应用程序项目并添加一个名为 Car.cs 的新类文件。现在，考虑使用此语法快速创建三个属性的 Car 类的重构：

```
using System;

namespace AutoProps
{
    class Car
    {
        // Automatic properties! No need to define backing fields.
        public string PetName { get; set; }
        public int Speed { get; set; }
        public string Color { get; set; }
    }
}
```

> 注意 Visual Studio 和 Visual Studio Code 提供 prop 代码片段。如果你在一个类定义中输入 prop 并按两次 Tab 键，IDE 将为新的自动属性生成起始代码。然后你可以使用 Tab 键在每个定义部分之间循环以填写详细信息。试试看！

在定义自动属性时，你只需指定访问修饰符、底层数据类型、属性名称以及空的 get/set 范围。在编译时，你的类型将获得一个自动生成的私有支持字段和一个合适的 get/set 逻辑实现。

> 注意 自动生成的私有支持字段的名称在你的 C# 代码库中不可见。唯一能看到它的方法是使用 ilasm.exe 等工具。

自 C# 版本 6 起，可以通过省略 set 范围来定义“只读自动属性”。只读自动属性只能在构造函数中设置。然而，不可能定义仅写属性。为了巩固，考虑以下内容：

```
// Read-only property? This is OK!
public int MyReadOnlyProp { get; }

// Write only property? Error!
public int MyWriteOnlyProp { set; }
```

## 与自动属性交互

由于编译器将在编译时定义私有支持字段，并且这些字段在 C# 代码中无法直接访问，因此定义自动属性的类始终需要使用属性语法来获取和设置底层值。这很重要，因为许多程序员会在类定义中直接使用私有字段，而在这种情况下这是不可能的。例如，如果 Car 类要提供一个 DisplayStats() 方法，它需要使用属性名称来实现此方法。

```
class Car
{
    // Automatic properties!
    public string PetName { get; set; }
    public int Speed { get; set; }
    public string Color { get; set; }

    public void DisplayStats()
    {
        Console.WriteLine("Car Name: {0}", PetName);
        Console.WriteLine("Speed: {0}", Speed);
        Console.WriteLine("Color: {0}", Color);
    }
}
```
