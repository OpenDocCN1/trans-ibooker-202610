
如果我们将 4×1 矩阵（点）乘以 4×4 矩阵（变换），就可以操纵该点。如果 Ti 表示一个变换，那么我们可以将这些变换相乘，以实现对该点的某种操纵：

```
T₁ × T₂ × T₃ × ⋯ × Tₙ = Tₜ
```

## 图 2A.48：变换的相乘

要乘以变换矩阵，A × P = B，我们执行如下操作：

```
[a₁₁ a₁₂ a₁₃ a₁₄; a₂₁ a₂₂ a₂₃ a₂₄; a₃₁ a₃₂ a₃₃ a₃₄; a₄₁ a₄₂ a₄₃ a₄₄] × [x; y; z; 1] = [a₁₁x + a₁₂y + a₁₃z + a₁₄; a₂₁x + a₂₂y + a₂₃z + a₂₄; a₃₁x + a₃₂y + a₃₃z + a₃₄; a₄₁x + a₄₂y + a₄₃z + a₄₄]
```

## 图 2A.49：变换矩阵的相乘

我们还可以这样表示：

```
bᵢ = Σⱼ=1⁴ aᵢⱼ pⱼ
```

## 图 2A.50：变换相乘的表达式

同样，对于两个 4×4 矩阵相乘，AxB=C，也可以这样做：

```
cᵢⱼ = Σₖ=1⁴ aᵢₖ bₖⱼ
```

对于 i = 1 到 4，j = 1 到 4

```
cᵢⱼ = Σₖ=1⁴ aᵢₖ bₖⱼ
```

## 图 2A.51：4x4 矩阵相乘的表达式：

变换的矩阵如下所示：

![](img/6507d435e2edfa3c00c220d0ccfcaf94_159_0.png)

图 2A.52：变换矩阵列表

> **注意**
> 
> 本活动的解决方案可在第 635 页找到。

## 总结

在本章中，我们学习了 C++ 中的类型。首先，我们讨论了内置类型，然后学习了如何创建我们自己的、行为类似内置类型的类型。我们学习了如何声明和初始化变量，初步了解了编译器从源代码生成了什么、变量放在哪里、链接器如何将其组合在一起，以及这在计算机内存中是什么样子。我们学习了 C++ 社区中关于零规则和五规则的一些经验法则。这些构成了 C++ 的构建块。在下一章中，我们将研究如何使用 C++ 模板创建函数和类，并进一步探索适用于模板的类型推导。

## 2B

禁止鸭子
– 模板与
推导

## 学习目标

学完本章后，你将能够：

- 使用继承和多态更有效地开发你自己的类
- 实现别名，使你的代码更易读
- 使用 SFINAE 和 constexpr 开发模板以简化代码
- 使用 STL 实现你自己的解决方案，以利用泛型编程
- 描述类型推导的上下文和基本规则

本章将向你展示如何通过继承、多态和模板来定义和扩展你的类型。

## 简介

在上一章中，我们学习了如何借助单元测试开发我们自己的类型（类），并使它们表现得像内置类型一样。我们还了解了函数重载、三/五规则和零规则。

在本章中，我们将学习如何进一步扩展类型系统。我们将学习如何使用模板创建函数和类，并重新审视函数重载，因为它受到模板使用的影响。我们将接触到一项新技术 SFINAE，并使用它来控制模板中那些被包含在生成代码中的部分。

## 继承、多态和接口

到目前为止，在我们学习面向对象设计和 C++ 的旅程中，我们一直专注于抽象和数据封装。现在，我们将把注意力转向继承和多态。什么是继承？什么是多态？为什么我们需要它们？考虑以下三个对象：

![](img/6507d435e2edfa3c00c220d0ccfcaf94_162_0.png)

在上面的图中，我们可以看到三个非常不同的对象。它们有一些共同点。它们都有轮子（数量不同）、发动机（尺寸、功率或配置不同），以及启动发动机、驾驶、刹车、停止发动机等操作，我们可以利用这些共同点来做些事情。

因此，我们可以将它们抽象为一种称为“交通工具”的事物，它展示了这些属性和一般行为。如果我们将其表示为 C++ 类，可能如下所示：

```
class Vehicle
{
public:
    Vehicle() = default;
    Vehicle(int numberWheels, int engineSize) :
        m_numberOfWheels{numberWheels}, m_engineSizeCC{engineSize}
    {
    }

```

我们之前遇到过访问修饰符，例如 **public**、**protected** 和 **private**。它们控制我们如何访问基类的成员。Motorcycle 类将如下派生：

```
class Motorcycle : public Vehicle
{
public:
    Motorcycle(int engineSize) : Vehicle(2, engineSize) {};
};
```

在这种情况下，Vehicle 类被称为**基类**或**超类**，而 Motorcycle 类被称为**派生类**或**子类**。我们可以用图形表示如下，其中箭头从派生类指向基类：

![](img/6507d435e2edfa3c00c220d0ccfcaf94_164_0.png)

图 2B.2：Vehicle 类层次结构

但是摩托车的驾驶方式与普通交通工具不同。因此，我们需要修改 **Motorcycle** 类，使其行为不同。更新后的代码如下：

```
class Motorcycle : public Vehicle
{
public:
    Motorcycle(int engineSize) : Vehicle(2, engineSize) {};

    void Drive()
    {
        std::cout << "Motorcycle::Drive\n";
    };
};
```

如果我们思考面向对象设计，这就是用协作的对象来建模问题空间。这些对象通过消息相互通信。现在，我们有两个类以不同的方式响应同一条消息（Drive() 方法）。消息的发送者不知道会发生什么，也并不真正关心，这正是多态的本质。

> **注意**
> 
> 多态性来自希腊语 poly 和 morph，其中 **poly** 意为“多”，**morph** 意为“形式”。因此，多态性意味着**具有多种形式。**

我们现在可以使用这些类来尝试多态：

```
#include <iostream>

int main()
{
    Vehicle vehicle;
    Motorcycle cycle{1500};
    Vehicle* myVehicle{&vehicle};

    myVehicle->StartEngine();
    myVehicle->Drive();
    myVehicle->ApplyBrakes();
    myVehicle->StopEngine();

    myVehicle = &cycle;
    myVehicle->StartEngine();
    myVehicle->Drive();
    myVehicle->ApplyBrakes();
    myVehicle->StopEngine();

    return 0;
}
```

如果我们编译并运行这个程序，会得到以下输出：

```
```text
$ ./vehicle.exe
Vehicle::StartEngine 1000 CC
Vehicle::Drive
Vehicle::ApplyBrakes to 4 wheels
Vehicle::StopEngine
Vehicle::StartEngine 1500 CC
Vehicle::Drive
Vehicle::ApplyBrakes to 2 wheels
Vehicle::StopEngine
```
```

图 2B.3：Vehicle 程序输出

在上面的截图中，**Vehicle::StartEngine 1500 CC** 之后的几行都与 **Motorcycle** 有关。但 Drive 行仍然显示 **Vehicle::Drive**，而不是预期的 **Motorcycle::Drive**。这是怎么回事？问题是，我们没有告诉编译器 **Vehicle** 类中的 **Drive** 方法可以被派生类修改（或重写）。我们需要在代码中做一处改动：

```
```cpp
virtual void Drive()
{
    std::cout << "Vehicle::Drive\n";
};
```
```

通过在成员函数声明前添加 **virtual** 关键字，我们告诉编译器：派生类可以（但不是必须）重写或替换这个函数。如果我们做出这一改动，然后编译并运行程序，会得到以下输出：

```
```text
$ ./vehicle.exe
Vehicle::StartEngine 1000 CC
Vehicle::Drive
Vehicle::ApplyBrakes to 4 wheels
Vehicle::StopEngine
Vehicle::StartEngine 1500 CC
Motorcycle::Drive
Vehicle::ApplyBrakes to 2 wheels
Vehicle::StopEngine
```
```

图 2B.4：使用虚方法的 Vehicle 程序输出

## 继承和访问说明符

如前所述，从超类继承子类的通用形式如下：

```
class DerivedClassName : access_modifier BaseClassName
```

当我们从 Vehicle 类派生 Motorcycle 类时，我们使用了以下代码：

```
class Motorcycle : public Vehicle
```

访问修饰符是可选的，并且是我们之前见过的修饰符之一：public、protected 和 private。在下表中，你可以看到基类成员的可访问性。如果省略了 access_modifier，那么编译器会假定指定了 private。

| 基类中的访问说明符 | 派生类中的有效访问说明符（当基类为） |  |  |
|---|---|---|---|
|  | public 基类 | protected 基类 | private 基类 |
| public | public | protected | private |
| protected | protected | protected | private |
| private | 不可访问 | 不可访问 | 不可访问 |

图 2B.5：派生类中基类成员的可访问性

## 抽象类和接口

到目前为止，我们讨论过的所有类都是具体类——它们可以被实例化为变量类型。还有另一种类——抽象类——它包含至少一个纯虚成员函数。纯虚函数是一种在类中没有定义（或实现）的虚函数。正因为它没有实现，这个类是不完整的（或者是抽象的），不能被实例化。如果你试图创建抽象类型的变量，编译器将生成错误。

要声明纯虚成员函数，需要在函数原型声明的末尾加上 = 0。要使 Drive() 成为 Vehicle 类中的纯虚函数，我们将它声明如下：

```
virtual void Drive() = 0;
```

现在，要能够将派生类用作变量类型（例如 Motorcycle 类），它必须定义 Drive() 函数的实现。

但是，你可以将变量声明为指向抽象类的指针或对抽象类的引用。无论哪种情况，它都必须指向或引用某个从抽象类派生的非抽象类。
