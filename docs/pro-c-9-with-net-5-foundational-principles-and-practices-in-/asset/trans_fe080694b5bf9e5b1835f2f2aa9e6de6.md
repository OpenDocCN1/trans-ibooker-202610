

如果没有歧义，在访问数据字段或成员时，你并不需要使用 `this` 关键字。例如，如果你将字符串数据成员从 `name` 重命名为 `driverName`（这也需要你更新顶层的语句），使用 `this` 是可选的，因为此时已不存在作用域歧义。

```
class Motorcycle
{
    public int driverIntensity;
    public string driverName;

    public void SetDriverName(string name)
    {
        // These two statements are functionally the same.
        driverName = name;
        this.driverName = name;
    }
    ...
}
```

即使在没有歧义的情况下使用 `this` 收获不大，你在实现类成员时仍可能会发现该关键字很有用，因为像 Visual Studio 和 Visual Studio Code 这样的 IDE 在指定 `this` 时会启用 IntelliSense。当你忘记了类成员的名称并想快速回忆其定义时，这会很有帮助。

注意 一个常见的命名约定是以下划线开头私有（或内部）类级变量名（例如，_driverName），这样 IntelliSense 会将你的所有变量显示在列表顶部。在我们这个简单的示例中，所有字段都是公开的，因此该命名约定不适用。在本书的其余部分，你会看到以前导下划线命名的私有和内部变量。

## 使用 this 链式调用构造函数

`this` 关键字的另一个用途是使用一种称为构造函数链式调用的技术来设计类。当一个类定义了多个构造函数时，这种设计模式很有帮助。鉴于构造函数通常会验证传入的参数以执行各种业务规则，在一个类的构造函数集中找到冗余的验证逻辑是很常见的。考虑以下更新后的 Motorcycle：

```
class Motorcycle
{
    public int driverIntensity;
    public string driverName;

    public Motorcycle() { }

    // Redundant constructor logic!
    public Motorcycle(int intensity)
    {
        if (intensity > 10)
        {
            intensity = 10;
        }
        driverIntensity = intensity;
    }

    public Motorcycle(int intensity, string name)
    {
        if (intensity > 10)
        {
            intensity = 10;
        }
        driverIntensity = intensity;
        driverName = name;
    }
    ...
}
```

这里（也许是为了确保骑手的 安全）每个构造函数都确保强度水平永远不会大于 10。虽然这很好，但你在两个构造函数中确实有冗余的代码语句。这并不理想，因为如果你的规则发生变化（例如，强度不应大于 5 而不是 10），你现在需要在多个位置更新代码。

改善当前情况的一种方法是定义 Motorcycle 类中的一个方法来验证传入的参数。如果你这样做，每个构造函数都可以在进行字段赋值之前调用此方法。虽然这种方法确实允许你在业务规则发生变化时隔离需要更新的代码，但你现在会遇到以下冗余：

```
class Motorcycle
{
    public int driverIntensity;
    public string driverName;

    // Constructors.
    public Motorcycle() { }

    public Motorcycle(int intensity)
    {
        SetIntensity(intensity);
    }

    public Motorcycle(int intensity, string name)
    {
        SetIntensity(intensity);
        driverName = name;
    }

    public void SetIntensity(int intensity)
    {
        if (intensity > 10)
        {
            intensity = 10;
        }
        driverIntensity = intensity;
    }
    ...
}
```

更简洁的方法是将接受最多参数的构造函数指定为“主构造函数”，并让其实现执行所需的验证逻辑。其余构造函数可以使用 `this` 关键字将传入的参数转发给主构造函数，并在必要时提供任何附加参数。这样，你只需要担心维护整个类的单个构造函数，而其余构造函数基本上是空的。

以下是 Motorcycle 类的最终迭代（为说明起见增加了一个额外的构造函数）。在链式调用构造函数时，注意 `this` 关键字如何通过冒号运算符“悬挂”在构造函数的声明之外，超出构造函数本身的作用域。

```
class Motorcycle
{
    public int driverIntensity;
    public string driverName;

    // Constructor chaining.
    public Motorcycle() {}
    public Motorcycle(int intensity)
        : this(intensity, "") {}
    public Motorcycle(string name)
        : this(0, name) {}

    // This is the 'master' constructor that does all the real work.
    public Motorcycle(int intensity, string name)
    {
        if (intensity > 10)
        {
            intensity = 10;
        }
        driverIntensity = intensity;
        driverName = name;
    }
    ...
}
```

要理解，使用 `this` 关键字链式调用构造函数永远不是强制性的。然而，当你使用这种技术时，你往往会得到更易维护和更简洁的类定义。再次使用这种技术，你可以简化你的编程任务，因为实际工作被委托给单个构造函数（通常是参数最多的构造函数），而其他构造函数只是简单地“推卸责任”。

注意 回顾第 4 章，C# 支持可选参数。如果你在使用类构造函数时使用可选参数，你可以用更少的代码实现与构造函数链式调用相同的优势。你将在稍后看到如何做到这一点。

## 观察构造函数流程

最后一点需要注意的是，一旦构造函数将参数传递给指定的主构造函数（并且该构造函数处理了数据），最初由调用者调用的构造函数将完成执行任何剩余的代码语句。为了澄清，请为 Motorcycle 类的每个构造函数添加一个合适的 `Console.WriteLine()` 调用。

```
class Motorcycle
{
    public int driverIntensity;
    public string driverName;

    // Constructor chaining.
    public Motorcycle()
    {
        Console.WriteLine("In default ctor");
    }

    public Motorcycle(int intensity)
        : this(intensity, "")
    {
        Console.WriteLine("In ctor taking an int");
    }

    public Motorcycle(string name)
        : this(0, name)
    {
        Console.WriteLine("In ctor taking a string");
    }

    // This is the 'master' constructor that does all the real work.
    public Motorcycle(int intensity, string name)
    {
        Console.WriteLine("In master ctor ");
        if (intensity > 10)
        {
            intensity = 10;
        }
        driverIntensity = intensity;
        driverName = name;
    }
    ...
}
```

现在，确保你的顶层语句按如下方式使用 Motorcycle 对象：

```
Console.WriteLine("***** Fun with class Types *****");

// Make a Motorcycle.
Motorcycle c = new Motorcycle(5);
c.SetDriverName("Tiny");
c.PopAWheely();
Console.WriteLine("Rider name is {0}", c.driverName);
Console.ReadLine();
```

有了这个，请思考前面代码的输出：

```
**** Fun with Motorcycles ****
In master ctor
In ctor taking an int
Yeeeeeee Haaaaaeeeww!
Yeeeeeee Haaaaaeeeww!
Yeeeeeee Haaaaaeeeww!
Yeeeeeee Haaaaaeeeww!
Yeeeeeee Haaaaaeeeww!
Yeeeeeee Haaaaaeeeww!
Rider name is Tiny
```

正如你所见，构造函数逻辑的流程如下：

- 你通过调用需要单个 int 的构造函数来创建对象。
- 该构造函数将提供的数据转发给主构造函数，并提供调用者未指定的任何额外启动参数。
- 主构造函数将传入的数据分配给对象的字段数据。
- 控制权返回到最初调用的构造函数并执行任何剩余的代码语句。

使用构造函数链式调用的好处是这种编程模式适用于任何版本的 C# 语言和 .NET 平台。然而，如果你以 .NET 4.0 及更高版本为目标，你可以通过使用可选参数作为传统构造函数链式调用的替代方案来进一步简化编程任务。

## 重新审视可选参数

在第 4 章中，你学习了可选参数和命名参数。回想一下，可选参数允许你为传入参数定义提供的默认值。如果调用者对这些默认值满意，他们无需指定唯一值；但是，他们可以这样做以向对象提供自定义数据。考虑以下 Motorcycle 版本，它现在使用单个构造函数定义提供了多种构造对象的方式：

```
class Motorcycle
{
    // Single constructor using optional args.
    public Motorcycle(int intensity = 0, string name = "")
    {
        if (intensity > 10)
        {
            intensity = 10;
        }
        driverIntensity = intensity;
```

