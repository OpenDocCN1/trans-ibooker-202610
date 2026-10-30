

因此，要将对象变量传递给 `GivePromotion()` 方法，可以编写以下代码：

```
// 正确！
GivePromotion((Manager)frank);
```

## 使用 C# 的 as 关键字

请注意，显式转换在运行时执行，而不是在编译时执行。作为示例，假设你的 Employees 项目中包含本章前面创建的 `Hexagon` 类副本。为简单起见，你可以将以下类添加到当前项目中：

```
class Hexagon
{
    public void Draw()
    {
        Console.WriteLine("Drawing a hexagon!");
    }
}
```

虽然将员工对象转换为形状对象完全没有意义，但类似下面的代码仍然可以在没有错误的情况下编译：

```
// 糟糕！不能将 frank 转换为 Hexagon，但这段代码可以正常编译！
object frank = new Manager();
Hexagon hex = (Hexagon)frank;
```

但是，你会收到运行时错误，或者更正式地说，会收到运行时异常。第 7 章将详细介绍结构化异常处理；不过，目前值得指出的是，在执行显式转换时，可以使用 `try` 和 `catch` 关键字捕获无效转换的可能性（完整细节同样请参阅第 7 章）。

```
// 捕获可能发生的无效转换。
object frank = new Manager();
Hexagon hex;
try
{
    hex = (Hexagon)frank;
}
catch (InvalidCastException ex)
{
    Console.WriteLine(ex.Message);
}
```

显然，这是一个人为设计的示例；在这种情况下，你绝不会费心在这些类型之间进行转换。不过，假设你有一个 `System.Object` 类型数组，其中只有少数元素包含与 `Employee` 兼容的对象。在这种情况下，你需要先确定数组中的某项是否兼容，如果兼容，再执行转换。

C# 提供了 `as` 关键字，用于在运行时快速确定给定类型是否与另一类型兼容。使用 `as` 关键字时，可以通过检查返回值是否为 `null` 来确定兼容性。请看下面的代码：

```
// 使用 "as" 测试兼容性。
object[] things = new object[4];
things[0] = new Hexagon();
things[1] = false;
things[2] = new Manager();
things[3] = "Last thing";

foreach (object item in things)
{
    Hexagon h = item as Hexagon;
    if (h == null)
    {
        Console.WriteLine("Item is not a hexagon");
    }
    else
    {
        h.Draw();
    }
}
```

这里，你遍历对象数组中的每一项，检查每一项是否与 `Hexagon` 类兼容。如果（且仅当！）找到与 `Hexagon` 兼容的对象，就调用 `Draw()` 方法。否则，只需报告该项不兼容。

## 使用 C# 的 is 关键字（C# 7.0、9.0 更新）

除了 `as` 关键字之外，C# 语言还提供了 `is` 关键字，用于确定两个对象是否兼容。不过，与 `as` 关键字不同的是，如果类型不兼容，`is` 关键字会返回 `false`，而不是返回 `null` 引用。目前，`GivePromotion()` 方法被设计为接受从 `Employee` 派生的任何类型。请看下面的更新版本，它现在会检查传入的具体“员工类型”：

```
static void GivePromotion(Employee emp)
{
    Console.WriteLine("{0} was promoted!", emp.Name);
    if (emp is SalesPerson)
    {
        Console.WriteLine("{0} made {1} sale(s)!", emp.Name,
            ((SalesPerson)emp).SalesNumber);
        Console.WriteLine();
    }
    else if (emp is Manager)
    {
        Console.WriteLine("{0} had {1} stock options…", emp.Name,
            ((Manager)emp).StockOptions);
        Console.WriteLine();
    }
}
```

这里，你执行了运行时检查，以确定传入的基类引用实际上指向内存中的什么对象。确定接收到的是 `SalesPerson` 类型还是 `Manager` 类型后，就可以执行显式转换，从而访问该类的专用成员。还要注意，你不需要将转换操作包装在 `try/catch` 结构中，因为根据条件检查，只要进入任意一个 `if` 作用域，就可以确定转换是安全的。

C# 7.0 新增了一个功能：如果转换成功，`is` 关键字还可以将转换后的类型赋值给一个变量。这可以避免“双重转换”问题，从而简化前面的方法。在前面的示例中，第一次转换发生在检查类型是否匹配时；如果匹配，就必须再次转换该变量。请看下面对前述方法的更新：

```
static void GivePromotion(Employee emp)
{
    Console.WriteLine("{0} was promoted!", emp.Name);
    //检查是否为 SalesPerson，并将其赋值给变量 s
    if (emp is SalesPerson s)
    {
        Console.WriteLine("{0} made {1} sale(s)!", s.Name,
            s.SalesNumber);
        Console.WriteLine();
    }
    //检查是否为 Manager，如果是，则将其赋值给变量 m
    else if (emp is Manager m)
    {
        Console.WriteLine("{0} had {1} stock options...",
            m.Name, m.StockOptions);
        Console.WriteLine();
    }
}
```

C# 9.0 引入了其他模式匹配功能（第 3 章将对此进行介绍）。这些更新后的模式匹配功能可以与 `is` 关键字结合使用。例如，要检查员工既不是 `Manager` 也不是 `SalesPerson`，可以使用以下代码：

```
if (emp is not Manager and not SalesPerson)
{
    Console.WriteLine("Unable to promote {0}. Wrong employee type", emp.Name);
    Console.WriteLine();
}
```

## 使用 is 关键字进行弃元（C# 7.0 新增）

`is` 关键字还可以与弃元变量占位符结合使用。如果你想在 `if` 或 `switch` 语句中创建一个通用匹配分支，可以按如下方式进行：

```
if (obj is var _)
{
    //执行某些操作
}
```

这会匹配所有内容，因此使用比较器与弃元时要特别注意顺序。更新后的 `GivePromotion()` 方法如下所示：

```
if (emp is SalesPerson s)
{
    Console.WriteLine("{0} made {1} sale(s)!", s.Name, s.SalesNumber);
    Console.WriteLine();
}
//检查是否为 Manager，如果是，则将其赋值给变量 m
else if (emp is Manager m)
{
    Console.WriteLine("{0} had {1} stock options...", m.Name, m.StockOptions);
    Console.WriteLine();
}
else if (emp is var _)
{
    Console.WriteLine("Unable to promote {0}. Wrong employee type", emp.Name);
    Console.WriteLine();
}
```

最后一个 `if` 语句会捕获所有不是 `Manager`、`SalesPerson` 或 `PtSalesPerson` 的 `Employee` 实例。请记住，可以向下转换为基类，因此 `PtSalesPerson` 会被识别为 `SalesPerson`。

## 再看模式匹配（C# 7.0 新增）

第 3 章介绍了 C# 7 的模式匹配功能，以及 C# 9.0 带来的更新。现在你已经牢固掌握了类型转换，是时候看一个更好的示例了。前面的示例现在可以简洁地更新为使用模式匹配 `switch` 语句，如下所示：

```
static void GivePromotion(Employee emp)
{
    Console.WriteLine("{0} was promoted!", emp.Name);
    switch (emp)
    {
        case SalesPerson s:
            Console.WriteLine("{0} made {1} sale(s)!", emp.Name,
                s.SalesNumber);
            break;
        case Manager m:
            Console.WriteLine("{0} had {1} stock options...",
                emp.Name, m.StockOptions);
            break;
    }
    Console.WriteLine();
}
```

向 `case` 语句添加 `when` 子句时，可以使用对象转换后完整定义中的所有内容。例如，`SalesNumber` 属性只存在于 `SalesPerson` 类中，而不存在于 `Employee` 类中。如果第一个 `case` 语句中的转换成功，变量 `s` 将保存一个 `SalesPerson` 类实例，因此可以将该 `case` 语句更新为：

```
case SalesPerson s when s.SalesNumber > 5:
```

这些针对 `is` 和 `switch` 语句的新功能带来了很好的改进，有助于减少执行匹配所需的代码量，正如前面的示例所示。

## switch 语句中的弃元（C# 7.0 新增）

弃元也可以用于 `switch` 语句中，如下面的代码所示：

```
switch (emp)
{
    case SalesPerson s when s.SalesNumber > 5:
        Console.WriteLine("{0} made {1} sale(s)!", emp.Name,
            s.SalesNumber);
        break;
    case Manager m:
        Console.WriteLine("{0} had {1} stock options...",
            emp.Name, m.StockOptions);
        break;
    case Employee _:
        Console.WriteLine("Unable to promote {0}. Wrong employee type", emp.Name);
        break;
}
```

传入的每一种类型本身都是 `Employee`，因此最后一个 `case` 语句始终为真。不过，正如第 3 章介绍模式匹配时所讨论的，一旦匹配成功，`switch` 语句就会退出。这说明了正确排列顺序的重要性。如果将最后一个语句移到最前面，就不会有任何 `Employee` 获得晋升。

## 理解超级父类：System.Object

本章最后，我想详细介绍超级父类：`Object`。在阅读前一节时，你可能注意到，层次结构中的基类（`Car`、`Shape`、`Employee`）从未显式指定它们的父类。

```
// Car 的父类是谁？
class Car
{...}
```

在 .NET Core 世界中，每种类型最终都派生自名为 `System.Object` 的基类，该类可以用 C# 的 `object` 关键字（小写的 o）表示。`Object` 类为框架中的每种类型定义了一组通用成员。事实上，当你创建一个未显式定义父类的类时，编译器会自动使该类型派生自 `Object`。如果你希望明确表达自己的意图，也可以定义派生自 `Object` 的类，如下所示（不过，同样没有必要这样做）：

```
// 这里显式派生自 System.Object。
class Car : object
{...}
```

