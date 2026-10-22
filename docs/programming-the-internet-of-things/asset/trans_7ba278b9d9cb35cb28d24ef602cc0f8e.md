

你会注意到项目中已经有六个文件：其中一个是 `programmingtheiot.cda.app` 包中的 ConstrainedDeviceApp，另一个位于顶层名为 pom.xml。还有四个 `__init__.py` 文件，这些是空文件，Python 解释器用它们来确定要搜索 Python 文件的目录（你现在可以忽略这些）。与之前给出的 GDA 示例（用 Java 编写）类似，ConstrainedDeviceApp 只是一个占位符，供你开始使用。

还提供了一个 Maven pom.xml，这对于熟悉 Maven 的人来说可能显得奇怪，因为这是一个 Python 项目，而不是 Java，而 Maven 通常与构建、测试和打包 Java 应用程序相关联。那么，为什么要用 Maven 呢？它相当灵活，并不关心它管理的[content]

你会注意到项目中已经有六个文件：其中一个是 `programmingtheiot.cda.app` 包中的 ConstrainedDeviceApp，另一个位于顶层名为 pom.xml。还有四个 `__init__.py` 文件，这些是空文件，Python 解释器用它们来确定要搜索 Python 文件的目录（你现在可以忽略这些）。与之前给出的 GDA 示例（用 Java 编写）类似，ConstrainedDeviceApp 只是一个占位符，供你开始使用。

还提供了一个 Maven pom.xml，这对于熟悉 Maven 的人来说可能显得奇怪，因为这是一个 Python 项目，而不是 Java，而 Maven 通常与构建、测试和打包 Java 应用程序相关联。那么，为什么要用 Maven 呢？它相当灵活，并不关心它管理的文件类型，因此可用于测试和打包 Python 模块。你在下一章开始做练习之前不需要此功能，因此现在你只需要通过执行以下操作来验证你的 CDA 是否运行：

## NOTE

如果你曾广泛使用过 Python，你可能熟悉 PYTHONPATH 环境变量。由于我尝试让 GDA 和 CDA 的打包方案保持相似，你可能需要告诉 PyDev（以及你的 virtualenv 环境）如何导航此目录结构以运行你的应用程序。确保在 PYTHONPATH 中为 ‘main’ 和 ‘test’ 都设置 ‘python’，方法是执行以下操作：右键单击 ‘piot-python-components’，选择 ‘PyDev - PYTHONPATH’，然后单击 ‘Add source folder’，如 Figure 1-11 所示。选择 ‘main’ 下的 ‘python’ 文件夹，然后单击 ‘Apply’。对 ‘test’ 下的 ‘python’ 文件夹执行相同操作。单击 ‘Apply and Close’ 完成。

Figure 1-11. - Updating the PYTHONPATH environment variable within PyDev and Eclipse

- 1. Run your CDA application within Eclipse.
- 
- a. Right click on the project ‘piot-python-components’ again, scroll down to ‘Run As’, and this time click Python Run.
- 
- b. Check the output in the Console at the bottom of the Eclipse IDE screen. As with your GDA test run, there should be no errors, with the output similar to the following:

```
2020-07-06 17:15:39,654:INFO:Attempting to load configuration...
2020-07-06 17:15:39,655:INFO:Starting CDA...
2020-07-06 17:15:39,655:INFO:CDA ran successfully.
```

恭喜！你的 GDA 和 CDA 都已在 IDE 中设置并正常工作。此时，你在技术上已准备好开始为两个应用程序编写自己的代码。但在我们进入第 2 章的练习之前，我们需要先弄清楚接下来的两个步骤：测试和自动化。

## Step II: Define Your Testing Strategy

现在你的 GDA 和 CDA 的开发环境已经建立，我们可以讨论你将如何测试即将开发的代码。显然，良好的测试是任何工程工作中的至关重要的一部分，编程也不例外。你构建的每个应用程序都应经过彻底测试，无论它是否完全独立于其他应用程序运行，还是与其它系统紧密集成。此外，你编写的每一单元代码都应经过测试，以确保其按预期行为。究竟什么是单元？就我们的目的而言，单元始终以你想要测试的函数或方法的形式表示。

## NOTE

函数和方法有什么区别？粗略地过度简化一下，函数是一段执行任务的代码的命名分组（例如将两个数字相加）并返回结果。如果函数接受任何输入，它将作为一个或多个参数传递。方法几乎与函数相同，但附加到对象上。在面向对象的术语中，对象只是已被实例化的类，而类是组件的形式定义——其方法、参数、构造和析构逻辑。 本书中的所有 Java 示例都将以类形式表示，并在每个类中定义方法。Python 可以以脚本形式用函数编写，也可以作为带方法的类编写，但我更喜欢用带方法的 Python 类编写，并且本书记载的每个 Python 示例都将如此，只有少数例外。

## Unit, Integration, and Performance Testing

有许多方法可以测试软件应用程序和系统，并且对此主题有许多优秀的书籍、文章和博客。开发一个可行的物联网解决方案需要仔细关注测试——在应用程序内部以及不同应用程序和系统之间。就你将开发的解决方案而言，我将只关注三种：单元测试、集成测试和性能测试。

单元测试是编写的代码模块，用于测试对测试可访问的最小可能代码单元，例如函数或方法。这些测试编写用于验证给定函数或方法的一组输入是否返回预期结果。边界条件通常也会被测试，以确保函数或方法能够适当处理这些类型的条件。

## NOTE

代码单元在技术上可以是一个单行、多行代码，甚至整个代码库。就我们的目的而言，单元是指可通过本地系统上可用的单个接口访问的一行或多行代码，或整个代码库——即封装单元功能的函数或方法，可从你的测试应用程序调用。此功能例如可以是排序算法、计算，甚至是进入一个或多个附加函数或方法的入口点。

我使用 JUnit 进行 Java 代码的单元测试（随 Eclipse 提供），并使用 PyUnit 进行 Python 代码的单元测试（Python 解释器的标准部分，并在 PyDev 中可用）。如果你使用 Eclipse 和 PyDev，在 IDE 中编写和执行单元测试无需安装任何额外组件。

## NOTE

在你的 GDA 项目中，你可能注意到 Maven 为你的源代码创建了两个目录结构：一个用于位于 ./src/main/java 的 Java 源代码，另一个用于位于 ./src/test/java 的 Java 单元测试代码。由于这是 Maven 项目的默认约定，保持原样。

在你的 CDA 项目中，代码路径结构与 GDA 项目略有不同：./src/main/java 和 ./src/test/java 缺失了。这可能看起来很合理，因为这是一个 Python 项目而不是 Java！所有源代码都包含在 ./src/programmingtheiot 中，而所有测试都包含在 ./tests/programmingtheiot 中。

虽然我为你提供了大量单元测试供使用，但我们将在第 2 章深入研究创建你自己的测试。如果你是该开发领域的新手，我在下面提供了 Java 和 Python 中的几个简单代码片段，以帮助你更熟悉代码结构。以下是使用 JUnit ⁶ 的简单 Java 单元测试，验证方法 addTwoIntegers() 是否按预期行为：

```
@Test
public int testAddTwoIntegers(int a, int b)
{
MyClass mc = new MyClass();

// baseline test

assertTrue(mc.addTwoIntegers(0, 0) == 0);

assertTrue(mc.addTwoIntegers(1, 2) == 3);

assertTrue(mc.addTwoIntegers(-1, 1) == 0);

assertTrue(mc.addTwoIntegers(-1, -2) == -3);

assertFalse(mc.addTwoIntegers(1, 2) == 4);

assertFalse(mc.addTwoIntegers(-1, -2) == -4);

}
```

如果你的单个测试类有两个独立的单元测试，但你只想运行其中一个？只需在 @Test 注解前添加 @Ignore，JUnit 将跳过该特定测试。删除该注解即可重新启用测试。

让我们用 Python 3 内置的 PyUnit 框架和 unittest 库⁷ 查看相同的示例：

```
def testAddTwoIntegers(self, a, b):

MyClass mc = MyClass()

# baseline test
```

```
self.assertTrue(mc.addTwoIntegers(0, 0) == 0)
self.assertTrue(mc.addTwoIntegers(1, 2) == 3)
self.assertTrue(mc.addTwoIntegers(-1, 1) == 0)
self.assertTrue(mc.addTwoIntegers(-1, -2) == -3)
self.assertFalse(mc.addTwoIntegers(1, 2) == 4)
self.assertFalse(mc.addTwoIntegers(-1, -2) == -4)
```

PyUnit 与 JUnit 类似，允许你在需要时禁用特定测试。只需在方法声明前添加 @unittest.skip("Put your reason here.")，或仅添加 @unittest.skip 作为注解，框架将跳过该特定测试。

集成测试对于物联网至关重要，因为它们可用于验证系统和应用程序之间的连接和交互是否按预期工作。假设你想使用嵌入在测试类中的基本数据集测试一个简单的排序算法——你通常会编写一个或多个单元测试，执行每个测试并验证一切正常。简单，对吧？

但是，如果排序算法需要从可通过本地网络甚至互联网访问的数据存储库中提取数据呢？那么怎么样，你可能会问？好吧，现在你只是为了运行排序测试而又多了一个依赖。你将需要一个集成

