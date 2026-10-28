

### 编程挑战 18 – 默认方法

问题：什么是默认方法？

默认方法是在 Java 8 中添加的，主要用于为接口提供支持，以便它们能够超越抽象契约而演进（即仅包含抽象方法）。该功能对于编写库并希望以兼容的方式演进 API 的人员非常有用。通过默认方法，可以在不破坏现有实现的情况下丰富接口。

默认方法是在接口中直接实现的，并以 default 关键字识别。例如，以下接口定义了一个名为 area() 的抽象方法和一个名为 perimeter() 的默认方法：

```
public interface Polygon {
	public double area();
	default double perimeter(double... segments) {
		return Arrays.stream(segments)
			.sum();
	}
}
```

由于 Polygon 只有一个抽象方法，它也是一个函数式接口。因此，它可以用 @FunctionalInterface 进行注解。

### 编程挑战 19 – Iterator 与 Spliterator

问题：Iterator 和 Spliterator 的主要区别是什么？

Iterator 是为 Collection API 创建的，而 Spliterator 是为 Stream API 创建的。

通过分析它们的名称，我们注意到 Spliterator = Splittable Iterator。因此，Spliterator 可以分割给定的数据源，并且也可以迭代它。分割对于并行处理是必要的。换句话说，Iterator 可以顺序地迭代 Collection 中的元素，而 Spliterator 可以并行或顺序地迭代流的元素。

Iterator 只能通过 hasNext()/next() 来遍历集合的元素，因为它没有大小。另一方面，Spliterator 可以通过 estimateSize() 近似提供集合的大小，或通过 getExactSizeIfKnown() 精确提供。

Spliterator 可以使用多个标志来在内部禁用不必要的操作（例如，CONCURRENT、DISTINCT 和 IMMUTABLE）。Iterator 没有此类标志。

最后，你可以按如下方式在 Iterator 周围创建一个 Spliterator：

```
Spliterators.spliteratorUnknownSize(
    your_Iterator, your_Properties);
```

在书籍 Java Coding Problems (https://www.amazon.com/gp/product/B07Y9BPV4W/) 中，你可以找到有关此主题的更多详细信息，包括编写自定义 Spliterator 的完整指南。

### 编程挑战 20 – Optional

问题：Optional 类是什么？

解答：受 Haskell 和 Scala 的启发，Optional 类在 Java 8 中引入，主要目的是减轻/避免 NullPointerException。Java 语言架构师 Brian Goetz 的定义如下：

> Optional 旨在为库方法返回类型提供一种有限的机制，在其中需要一种明确表示无结果的方式，而使用 null 来表示这种情况极有可能导致错误。

简而言之，你可以将 Optional 视为一个单值容器，它包含一个值或为空。例如，一个空的 Optional 看起来像这样：

```
Optional<User> userOptional = Optional.empty();
```

而一个非空的 Optional 看起来像这样：

```
User user = new User();
Optional<User> userOptional = Optional.of(user);
```

在 Java Coding Problems (https://www.amazon.com/gp/product/B07Y9BPV4W/) 中，你可以找到一个专门介绍使用 Optional 的最佳实践的完整章节。这是任何 Java 开发者必读的章节。

### 编程挑战 21 - String::valueOf

**问题：** `String::valueOf` 的含义是什么？

**解答：** `String::valueOf` 是对 `String` 类的 `valueOf` 静态方法的引用。建议阅读 *Coding challenge 17* 以获取更多信息。

### 总结

在本章中，我们涵盖了有关 Java 中函数式编程风格的几个热门话题。虽然该主题相当广泛，有许多书籍专门介绍它，但这里涵盖的问题足以通过一次涵盖 Java 8 语言主要特性的常规 Java 面试。

在下一章中，我们将讨论与扩展相关的问答。

## 18 单元测试

作为一名开发者（或软件工程师），你也必须具备测试领域的技能。例如，开发者负责编写其代码的单元测试（例如，使用 JUnit 或 TestNG）。很可能，不包含单元测试的拉取请求也不会被接受。

在本章中，我们将介绍你在申请开发者或软件工程师职位时可能遇到的单元测试面试问题。当然，如果你正在寻找测试员（手动/自动化）职位，那么本章可能只是测试的另一个视角，因此不要期望在这里看到针对手动/自动化测试员职位特定问题的提问。本章将涵盖以下主题：

- 单元测试概览
- 问题和编程题

让我们开始吧！

### 技术要求

本章使用的代码可以在 GitHub 上找到：https://github.com/PacktPublishing/The-Complete-Coding-Interview-Guide-in-Java/tree/master/Chapter18

### 单元测试概览

测试应用程序的过程包含多个测试层。其中一层是 *单元测试* 层。

主要来说，一个应用程序是由称为单元的小功能部件构建的（例如，一个普通的 Java 方法可以被视为一个单元）。在特定输入/条件/约束下测试这些单元的功能和正确性称为单元测试。

这些单元测试由开发者使用源代码和测试计划编写。理想情况下，每个开发者都应该能够编写测试/验证其代码的单元测试。单元测试应有意义并提供可接受的代码覆盖率。

如果单元测试失败，则开发者负责修复问题并再次执行单元测试。以下图表描述了这一陈述：

![](img/6709a71136a6b327c831c6c15aff2f6a_734_0.png)

单元测试使用 **单元测试用例**。*单元测试用例* 是输入数据和预期输出的配对，用于为特定功能构建测试。

如果你参加的面试要求你了解单元测试，不要惊讶如果你被问到功能测试和/或集成测试的问题。因此，建议你也为这些问题准备好答案。

**功能测试** 是关于根据给定输入和产生的输出（行为）来测试功能需求，该输出（行为）需要与预期输出（行为）进行比较。每个功能测试都使用功能规范来验证代表该功能需求实现的组件（或一组组件）的正确性。这在以下图表中进行了说明：

![](img/6709a71136a6b327c831c6c15aff2f6a_734_1.png)

**集成测试** 的目标是在软件组件以迭代增量的方式集成时发现缺陷。换句话说，已经进行单元测试的模块被集成（分组或聚合）并按照集成计划进行测试。这在以下图表中有所体现：

![](img/6709a71136a6b327c831c6c15aff2f6a_735_0.png)

图 18.3 – 集成测试

经常向面试候选人提出的一个关于单元测试和集成测试的问题是关于突出这两者之间的主要区别。以下表格将帮助你准备对此问题的回答：

| 单元测试 | 集成测试 |
|---|---|
| 通常对开发者有用。 | 对 QA、DevOps 和 Help Desk 也很有用 |
| 结果仅取决于 Java 代码。 | 结果可能取决于外部系统。 |
| 相当容易编写。 | 设置起来可能相当复杂 |
| 单元被隔离测试。 | 测试一个或多个组件。 |
| 可以为依赖项使用模拟。 | 应避免模拟。 |
| 仅测试代码的实现。 | 测试组件的实现及其之间的互连行为 |
| 使用 JUnit/TestNG。 | 使用 Arquillian 和 DbUnit 等工具的真实环境。 |
| 测试失败是一个回归问题。 | 测试失败可能是由环境变化引起的 |
| 此类测试不应花费太长时间运行。 | 此类测试可能需要很长时间（例如，1 小时） |

图 18.4 – 单元测试与集成测试的比较

一个优秀的测试人员能够在不做任何关于输入的假设或约束的情况下，对测试对象进行压力测试和滥用测试。这同样适用于单元测试。现在我们已经涉及了单元测试，让我们来看一些关于单元测试的编程挑战和问题。

