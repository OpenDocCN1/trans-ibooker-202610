
# 继承与多态的最佳实践

现在，我们已经学习了继承和多态。我们使用指向 `Vehicle` 类的指针来控制 `Motorcycle` 类。作为最佳实践，还应对代码进行另一项更改。我们还应按如下方式更改 `Motorcycle` 中 `Drive` 函数的声明：

```cpp
void Drive() override
{
  std::cout << "Motorcycle::Drive\n";
};
```

C++11 引入了 `override` 关键字作为给编译器的提示，表明特定方法应与其父类树中的某个方法具有相同的函数原型。如果找不到，编译器将报告错误。这是一个非常有用的功能，能为你节省数小时的调试时间。如果有办法让编译器报告错误，就使用它。缺陷发现得越早，修复起来就越容易。最后一项更改是，每当我们向类添加虚函数时，必须将其析构函数声明为 `virtual`：

```cpp
class Vehicle
{
public:
  // Constructors - hidden
  virtual ~Vehicle() = default;  // Virtual Destructor
  // Other methods and data -- hidden
};
```

我们在 `Drive()` 函数变为虚函数之前就看到了这种情况。当通过指向 `Vehicle` 的指针调用析构函数时，它需要知道该调用哪个析构函数。因此，将其设为 `virtual` 可实现此目的。如果未能这样做，可能会导致资源泄漏或对象切片。

## 继承与访问说明符

如前所述，从超类继承一个子类的一般形式如下：

`class DerivedClassName : access_modifier BaseClassName`

当我们从 `Vehicle` 类派生 `Motorcycle` 类时，使用以下代码：

`class Motorcycle : public Vehicle`

访问修饰符是可选的，是我们之前遇到的几种之一：`public`、`protected` 和 `private`。在下表中，你可以看到基类成员的可访问性。如果省略 `access_modifier`，则编译器假定指定了 `private`。

![图 2B.5：派生类中基类成员的可访问性](image/C14583_02B_05.jpg)

图 2B.5：派生类中基类成员的可访问性

## 抽象类与接口

到目前为止我们讨论的所有类都是 `concrete class`（具体类/实例化类） —— 它们可以作为变量的类型被实例化。还有另一种类型的类 —— `abstract class`（抽象类） —— 它至少包含一个 `pure virtual member function`（纯虚成员函数）。纯虚函数是在类中没有定义（或实现）的虚函数。因为它没有实现，所以该类是不完整的（或抽象的），无法被实例化。如果你尝试创建一个抽象类型的变量，编译器将生成错误。

要声明纯虚成员函数，请在函数原型声明末尾加上 `= 0`。要使 `Drive()` 成为 `Vehicle` 类中的纯虚函数，我们按如下方式声明：

`virtual void Drive() = 0;`

现在，要想将派生类用作变量类型（例如 `Motorcycle` 类），它必须定义 `Drive()` 函数的实现。

但是，你可以声明一个变量为指向抽象类的指针或指向抽象类的引用。无论哪种情况，它必须指向或引用某个从抽象类派生的非抽象类。

在 Java 中，有一个 `interface` 关键字，允许你定义一个全是纯虚函数的类。在 C++ 中，通过声明一个仅包含 `public` 纯虚函数（和一个 `virtual` 析构函数）的类来实现相同的效果。这样，我们就定义了一个接口。

### 注意

在解决本章中的任何实践练习之前，请下载本书的 GitHub 仓库（[https://github.com/TrainingByPackt/Advanced-CPlusPlus](https://github.com/TrainingByPackt/Advanced-CPlusPlus)），并在 Eclipse 中导入 Lesson 2B 的文件夹，以便查看每个练习和活动的代码。

## 练习 1：使用多态实现游戏角色

在本练习中，我们将演示继承、接口和多态。我们将从角色扮演游戏的临时实现开始，并将其演变为更通用、可扩展的版本。让我们开始吧：

1. 打开 Eclipse，使用 `Lesson2B` 示例文件夹中的文件创建一个名为 `Lesson2B` 的新项目。
2. 因为这是一个 `CMake-based project`（基于 CMake 的项目），将当前构建器更改为 `Cmake Build (portable)`。
3. 转到 `Project | Build All` 菜单以构建所有练习。默认情况下，屏幕底部的控制台将显示 `CMake console [Lesson2B]`。
4. 配置一个名为 `L2BExercise1` 的 `New Launch Configuration`（新启动配置），用于运行 `Exercise1` 二进制文件，然后点击 `Run` 以构建并运行 `Exercise 1`。你将获得以下输出：

   ![图 2B.6：练习 1 默认输出](image/C14583_02B_06.jpg)

   图 2B.6：练习 1 默认输出

5. 在编辑器中打开 `Exercise1.cpp` 并检查现有代码。你会注意到这三个角色被实现为单独的类，并且它们各自被实例化和操作，通过直接调用 `speak()` 和 `act()`。这对于小程序来说没问题。但随着游戏角色增加到几十或几百个，它将变得难以管理。因此，我们需要抽象所有角色。将以下接口声明添加到文件顶部：

   ```cpp
   class ICharacter
   {
   public:
       ~ICharacter() {
           std::cout << "Destroying Character\n";
       }
       virtual void speak() = 0;
       virtual void act() = 0;
   };
   ```

   通常析构函数应该是空的，但这里添加了日志记录以显示行为。

6. 从该接口类派生 `Wizard`、`Healer` 和 `Warrior` 类，并在每个类的 `speak()` 和 `act()` 函数声明末尾添加 `override` 关键字：

   `class Wizard : public ICharacter { ...`

7. 点击 `Run` 按钮以重新构建并运行练习。现在我们将看到基类析构函数也在派生类析构函数之后被调用：

   ![图 2B.7：修改后程序的输出](image/C14583_02B_07.jpg)

   图 2B.7：修改后程序的输出

8. 创建角色并在诸如 `vector` 之类的容器中管理它们。在文件中 `main()` 函数之前创建以下两个方法：

   ```cpp
   void createCharacters(std::vector<ICharacter*>& cast)
   {
       cast.push_back(new Wizard("Gandalf"));
       cast.push_back(new Healer("Glenda"));
       cast.push_back(new Warrior("Ben Grimm"));
   }

   void freeCharacters(std::vector<ICharacter*>& cast)
   {
       for(auto* character : cast)
       {
           delete character;
       }
       cast.clear();
   }
   ```

9. 用以下代码替换 `main()` 的内容：

   ```cpp
   int main(int argc, char**argv)
   {
       std::cout << "\n------ Exercise 1 ------\n";
       std::vector<ICharacter*> cast;
       createCharacters(cast);
       for(auto* character : cast)
       {
           character->speak();
       }
       for(auto* character : cast)
       {
           character->act();
       }
       freeCharacters(cast);
       std::cout << "Complete.\n";
       return 0;
   }
   ```

10. 点击 `Run` 按钮以重新构建并运行练习。以下是生成的输出：

    ![图 2B.8：多态版本的输出](image/C14583_02B_08.jpg)

    图 2B.8：多态版本的输出

    从前面的屏幕截图可以看出，`Destroying Wizard` 等日志记录消失了。问题在于容器持有指向基类的指针，并且不知道在每种情况下如何调用完整的析构函数。

11. 要修复此问题，只需将 `ICharacter` 的析构函数声明为 `virtual`：
