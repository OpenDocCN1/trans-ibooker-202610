
###### 图 2A.49：变换矩阵相乘

我们也可以这样表示：

![图 2A.50：变换相乘的表达式](image/C14583_02A_50.jpg)

###### 图 2A.50：变换相乘的表达式

同样地，两个 **4 x 4** 矩阵相乘也可以这样做，**AxB=C**：

![图 2A.51 4x4 矩阵乘法的表达式：](image/C14583_02A_51.jpg)

###### 图 2A.51 4x4 矩阵乘法的表达式：

变换的矩阵如下：

![图 2A.52：变换矩阵列表](image/C14583_02A_52.jpg)

###### 图 2A.52：变换矩阵列表

#### 注

此活动的解答见第 635 页。

## 总结

在本章中，我们学习了 C++ 中的类型。首先，我们涉及了内置类型，然后学习了如何创建表现得像内置类型一样的自定义类型。我们学习了如何声明和初始化变量，窥探了编译器从源代码生成的内容、变量存放的位置、链接器如何将它们组合在一起，以及这些在计算机内存中的样子。我们学习了一些关于 `Rule of Zero`（零法则）和 `Rule of Five`（五法则）的 C++ 部落智慧。这些构成了 C++ 的基石。在下一章中，我们将探讨使用 C++ 模板创建函数和类，并进一步探索适用于模板的类型推导。

# 2B. 不允许鸭子类型 —— 模板与推导

## 学习目标

读完本章后，你将能够：

*   利用继承和多态更有效地开发自己的类
*   实现别名以使代码更易读
*   使用 `SFINAE` 和 `constexpr` 开发模板以简化代码
*   使用 `STL` 实现自己的解决方案以利用泛型编程
*   描述类型推导的上下文和基本规则

本章将展示如何通过继承、多态和模板来定义和扩展你的类型。

## 简介

在上一章中，我们学习了如何在单元测试的帮助下开发自己的类型（类），并使它们表现得像内置类型一样。我们接触了函数重载、`Rule of Three/Five`（三/五法则）和 `Rule of Zero`（零法则）。

在本章中，我们将学习如何进一步扩展类型系统。我们将学习如何使用模板创建函数和类，并重新审视函数重载，因为它受模板使用的影响。我们将接触一项新技术 **SFINAE**，并用它来控制模板中哪些部分包含在生成的代码中。

## 继承、多态与接口

到目前为止，在我们的面向对象设计和 C++ 之旅中，我们专注于抽象和数据封装。现在我们将把注意力转向 **继承** 和 **多态**。什么是继承？什么是多态？为什么我们需要它？请考虑以下三个对象：

![图 2B.1：车辆对象](image/C14583_02B_01.jpg)

###### 图 2B.1：车辆对象

在前面的图表中，我们可以看到有三个截然不同的对象。它们有一些共同点。它们都有轮子（数量不同）、发动机（大小、功率或配置不同）、`StartEngine()`、`Drive()`、`ApplyBrakes()`、`StopEngine()` 等，我们可以使用这些来做一些事情。

因此，我们可以将它们抽象为一种称为“车辆”的东西，它展示了这些属性和通用行为。如果用 C++ 类来表示，可能如下所示：

```cpp
class Vehicle
{
public:
    Vehicle() = default;
    Vehicle(int numberWheels, int engineSize) :
        m_numberOfWheels{numberWheels}, m_engineSizeCC{engineSize}
    {
    }
    bool StartEngine()
    {
        std::cout << "Vehicle::StartEngine " << m_engineSizeCC << " CC\n";
        return true;
    };
    void Drive()
    {
        std::cout << "Vehicle::Drive\n";
    };
    void ApplyBrakes()
    {
        std::cout << "Vehicle::ApplyBrakes to " << m_numberOfWheels << " wheels\n";
    };
    bool StopEngine()
    {
        std::cout << "Vehicle::StopEngine\n";
        return true;
    };

private:
    int m_numberOfWheels {4};
    int m_engineSizeCC{1000};
};
```

`Vehicle` 类是 `Motorcycle`（摩托车）、`Car`（汽车）和 `Truck`（卡车）更泛化（或抽象）的表达。现在我们可以通过复用 `Vehicle` 类中已有的内容来创建更特化的类型。我们将通过继承来复用 `Vehicle` 的属性和方法。继承的语法如下：

```cpp
class DerivedClassName : access_modifier BaseClassName
{
    // Body of DerivedClass
};
```

我们之前遇到过 `public`、`protected` 和 `private` 等访问修饰符。它们控制我们如何访问基类的成员。`Motorcycle` 类将按如下方式派生：

```cpp
class Motorcycle : public Vehicle
{
public:
    Motorcycle(int engineSize) : Vehicle(2, engineSize) {};
};
```

在这种情况下，`Vehicle` 类被称为 **基类** 或 **超类**，而 `Motorcycle` 类被称为 **派生类** 或 **子类**。图形上可以表示如下，箭头从派生类指向基类：

![图 2B.2：车辆类层次结构](image/C14583_02B_02.jpg)

###### 图 2B.2：车辆类层次结构

但摩托车的驾驶方式与通用车辆不同。因此，我们需要修改 `Motorcycle` 类，使其表现不同。更新后的代码如下：

```cpp
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

如果我们考虑面向对象设计，这是关于用协作的对象来建模问题空间。这些对象通过消息相互通信。现在，我们有两个类以不同的方式响应同一个消息（`Drive()` 方法）。消息发送者不知道会发生什么，也不关心，这就是多态的本质。

#### 注

多态源自希腊语 poly 和 morph，其中 **poly** 意为多，**morph** 意为形态。因此，多态意味着 **具有多种形态**。

我们现在可以使用这些类来尝试多态：

```cpp
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

如果我们编译并运行此程序，将得到以下输出：

![图 2B.3：车辆程序输出](image/C14583_02B_03.jpg)

###### 图 2B.3：车辆程序输出

在前面的截图中，**Vehicle::StartEngine 1500 cc** 这一行之后的行都与 `Motorcycle` 相关。但 `Drive` 行仍显示 **Vehicle::Drive**，而不是预期的 **Motorcycle::Drive**。这是怎么回事？问题在于我们没有告诉编译器 `Vehicle` 类中的 `Drive` 方法可以被派生类修改（或重写）。我们需要在代码中做一处更改：

```cpp
virtual void Drive()
{
    std::cout << "Vehicle::Drive\n";
};
```

通过在成员函数声明前添加 `virtual` 关键字，我们告诉编译器派生类可以（但不必）重写或替换该函数。如果我们做这个更改，然后编译并运行程序，将得到以下输出：

![图 2B.4：带有虚方法的车辆程序输出](image/C14583_02B_04.jpg)

###### 图 2B.4：带有虚方法的车辆程序输出
