
第一个，但执行不同的操作或转换。最后，第三个参数 BiFunction 用于合并两个操作的结果，以产生组合的累积结果。让我们使用 teeing 来按需查找最小值和最大值：

```
record MinMax(String least, String most) {}

var leastAndMostEmailAddressPerson =
SAMPLE_DATA.stream()
.collect(
teeing(
minBy(comparing(person -> person.emailAddresses().size())),
maxBy(comparing(person -> person.emailAddresses().size())),
(min, max) ->
new MinMax(min.map(Person::fullName).orElse(""),
max.map(Person::fullName).orElse(""))));

System.out.println(leastAndMostEmailAddressPerson);
```

我们在需要的方法中创建了一个局部记录 MinMax。记录使意图清晰，代码也富有表现力。记录是在现代 Java 中创建元组的好方法，并且适合方便地处理小型任意数据集合。传递给 teeing 函数的第一个参数使用 Collectors 的 minBy 函数选择电子邮件地址数量最少的人。第二个参数使用 maxBy 方法选择电子邮件地址数量最多的人。如果流动的数据为空，那么可能不存在最小值或最大值。因此，minBy 和 maxBy 的结果是 Optional<T>。在将两个值合并为累积结果的函数中，我们使用两个组件或属性创建局部记录 MinMax 的实例。第一个 least 将保存由 Collector 的 minBy 选择的人的全名；如果没有值，则返回空字符串。同样，我们创建 most 的值。让我们执行代码并检查输出：

```
MinMax[least=John Doe, most=Nancy Xie]
```

代码识别出了电子邮件地址数量最少的人和数量最多的人。

我们已经看到了 Collectors 工具类中的许多函数。这些函数可以与 collect 方法单独使用。此外，我们还可以组合这些方法来解决更复杂的问题。由于 Collectors 函数的递归特性，大多数方法可以接受本身也是 Collectors 的额外参数，就像我们在 Counting Occurrences 中看到的那样。

## 总结

数据转换是编程中的常见操作，Java 的更新使我们能够优雅而轻松地应对这些挑战。执行转换的能力既包含在 *Streams API* 中，也特别包含在 *Collectors* 工具类中。在本章中，我们看到了可以使用 *Collectors* 中不同方法完成的多种转换。我们从一些简单的转换开始，转向更复杂的转换，最后总结了可用于在一次迭代中一次性执行多个转换的设施。花点时间熟悉 *Collectors* 中的函数，因为它是 JDK 中最全面的类之一。

在下一章中，我们将探讨使用函数式接口和 Lambda 表达式设计程序的方法。

## 使用 Lambda 表达式进行设计

OOP 已成为事实上的标准，但借助 Java 中的 Lambda 表达式，我们可以从设计技巧包中再掏出几招。在 Java 中，OOP 和函数式风格可以互相补充，并很好地协同工作。我们可以利用这些来创建轻量级设计，使其灵活易变且更易扩展。

我们可以使用简洁的代码来替代接口、类层次结构和匿名内部类。完成同样的工作需要更少的代码行数，并且我们可以快速尝试新想法。

在本章中，Lambda 表达式将一些精妙的设计理念付诸实践；在我们经常使用对象的地方，我们将改用轻量级函数。我们将使用 Lambda 表达式轻松地将逻辑与函数分离，使其更具可扩展性。然后，我们将应用它们来委派职责，并在仅几行代码中实现装饰器模式。最后，我们将使用它们将乏味的接口转化为流畅、直观的接口。

## 使用 Lambda 表达式分离关注点

我们通常创建类来复用代码；我们有良好的初衷，但这并不总是正确的选择。通过使用高阶函数，我们可以在不需要类层次结构的情况下实现相同的目标。

### 探索设计关注点

让我们以求和资产值为例，来说明关注点分离的设计理念。我们将通过迭代来构建它。我们最初创建的设计会在一个方法中混合多个关注点，但我们将快速重构代码，使方法更加内聚。让我们从 Asset 类开始。

```
designing/fpij/Asset.java

public class Asset {
    public enum AssetType { BOND, STOCK };

    private final AssetType type;
    private final int value;

    public Asset(final AssetType assetType, final int assetValue) {
        type = assetType;
        value = assetValue;
    }

    public AssetType getType() { return type; }

    public int getValue() { return value; }
}
```

Asset 是一个带有两个属性的简单 JavaBean：type 和 value。

假设我们需要计算给定所有资产的总值——让我们在 AssetUtil 类中为此编写一个方法。

```
designing/fpij/AssetUtil.java
public static int totalAssetValues(final List<Asset> assets) {
    return assets.stream()
        .mapToInt(Asset::getValue)
        .sum();
}
```

我们在此函数中使用了 Lambda 表达式的便利性。我们将 Asset 列表转换为 Stream，然后使用 mapToInt 方法将其映射为值的 Stream。最后，我们使用 sum 方法将此流中的值缩减或总计，以得到单个值。

让我们定义一些资产来试用该代码。

```
designing/fpij/AssetUtil.java
final List<Asset> assets = Arrays.asList(
    new Asset(Asset.AssetType.BOND, 1000),
    new Asset(Asset.AssetType.BOND, 2000),
    new Asset(Asset.AssetType.STOCK, 3000),
    new Asset(Asset.AssetType.STOCK, 4000)
);
```

这是使用这些资产调用 totalAssetValues 方法的示例。

```
designing/fpij/AssetUtil.java
System.out.println("Total of all assets: " + totalAssetValues(assets));
```

代码将报告所有给定资产的总值，如输出所示。

Total of all assets: 10000

使用 Lambda 表达式编写 totalAssetValues 方法是件好事——我们使用了流式迭代器并倾向于不可变性。但让我们将注意力转移到方法本身的设计上。它与三个关注点纠缠在一起：如何迭代、总计什么、如何总计。这种纠缠的逻辑会导致复用性差。让我们来看看。

### 与关注点纠缠

假设我们需要仅计算债券资产的总值。快速浏览 totalAssetValues 方法后，我们意识它几乎做了我们需要的一切。为什么不复制粘贴那段代码呢？毕竟，集成开发环境费劲地提供该功能的键盘快捷方式，肯定有其道理，对吧？

我们将保持 totalAssetValues 完好，但会复制它并修改新版本，如下所示：

```
designing/fpij/AssetUtil.java

public static int totalBondValues(final List<Asset> assets) {
    return assets.stream()
        .mapToInt(asset ->
            asset.getType() == AssetType.BOND ? asset.getValue() : 0)
        .sum();
}
```

totalBondValues 和 totalAssetValues 除了名称不同之外，唯一的区别在于我们传递给 mapToInt 函数的 Lambda 表达式。在较新的方法中，如果资产是债券，我们选取其价格；否则，价格用 0 表示。与其将逻辑挤在同一个 Lambda 表达式中，不如使用 filter 方法仅提取债券，并保持传递给 mapToInt 方法的 Lambda 表达式与从 totalAssetValues 方法复制的版本保持不变。

让我们调用这个版本的方法并确保它能正常工作。
