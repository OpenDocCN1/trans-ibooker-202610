

## 另请参阅

现在你已经创建了一个流，就必须处理其元素。本章中的所有配方都提供了有关如何处理流元素的的信息。

## 减少流的元素

**MapReduce** 是一种编程模型，用于在由集群中大量机器工作的分布式环境中使用非常大的数据集进行处理。该编程模型具有以下两种操作：

- **Map**：此操作过滤并转换原始元素，使其更适合归约操作
- **Reduce**：此操作从所有元素生成摘要结果，例如数值之和或平均值

该编程模型在函数式编程世界中已被广泛使用。在 Java 生态系统中，**Apache 软件基金会**的 **Hadoop** 项目提供了该模型的实现。`Stream` 类实现了两种不同的 reduce 操作：

- 纯 reduce 操作，以 `reduce()` 方法的不同版本实现，处理流元素以获得一个值
- 可变归约，以 `collect()` 方法的不同版本实现，处理流元素以生成可变数据结构，例如 `Collection` 或 `StringBuilder`。

在本配方中，你将学习如何使用 `reduce()` 方法的不同版本从值流中生成结果。正如你可能已经想象的那样，`reduce()` 方法是 `Stream` 中的一个终端操作。

## 准备工作

本配方的示例使用 Eclipse IDE 实现。如果使用 Eclipse 或其他 IDE，例如 NetBeans，请打开它并创建一个新的 Java 项目。

## 如何操作...

按照以下步骤实现示例：

- 1. 首先，我们将创建一些辅助类，我们将在示例后面使用它们。查看配方《从不同源创建流》，并在此示例中包含其中使用的 Person 和 PersonGenerator 类。
- 2. 然后，创建一个名为 DoubleGenerator 的类。实现一个名为 generateDoubleList() 的方法来生成双精度数字列表。它接收两个参数，即我们将生成的列表的大小和列表中的最高值。它将生成随机双精度数字列表：
- 3. 实现一个名为 generateStreamFromList() 的方法。此方法接收一个双精度数字的 List 作为参数，并生成一个包含列表元素的 DoubleStream 流。为此，我们将使用 DoubleStream.Builder 类来构造流：
- 4. 创建一个名为 Point 的类，具有两个 double 属性 x 和 y，以及获取和设置其值的 get() 和 set() 方法。此类的代码非常简单，因此不会包含。

```
public class DoubleGenerator {

    public static List<Double> generateDoubleList(int size,
                                                int max) {
        Random random=new Random();
        List<Double> numbers=new ArrayList<>();

        for (int i=0; i<size; i++) {
            double value=random.nextDouble()*max;
            numbers.add(value);
        }
        return numbers;
    }
}
```

```
public static DoubleStream generateStreamFromList(List<Double>
                                                    list) {
    DoubleStream.Builder builder=DoubleStream.builder();

    for (Double number : list) {
        builder.add(number);
    }
    return builder.build();
}
```

5. 创建一个名为 PointGenerator 的类，其中有一个名为 generatePointList() 的方法。此方法接收你想要生成的列表的大小，并返回一个随机 Point 对象的列表：

```
public class PointGenerator {
    public static List<Point> generatePointList (int size) {

        List<Point> ret = new ArrayList<>();
        Random randomGenerator=new Random();
        for (int i=0; i<size; i++) {
            Point point=new Point();
            point.setX(randomGenerator.nextDouble());
            point.setY(randomGenerator.nextDouble());
            ret.add(point);
        }
        return ret;
    }
}
```

6. 现在创建具有 main() 方法的 Main 类。首先，我们将使用 DoubleGenerator 类生成 10,000 个双精度数字的 List：

```
public class Main {
    public static void main(String args[]) {

        List<Double> numbers = DoubleGenerator.generateDoubleList
                                (10000, 1000);
```

7. `Stream` 类和专门的 DoubleStream、IntStream 和 LongStream 类实现了一些专门的 reduce 操作方法。在这种情况下，我们将使用 DoubleGenerator 类生成一个 DoubleStream，并使用 count()、sum()、average()、max() 和 min() 来获得元素的数量、所有元素的总和、所有元素的平均值、流中的最大值和最小值。由于我们只能处理流的元素一次，因此我们必须为每个操作创建一个新的流。请注意，这些方法仅存在于 DoubleStream、IntStream 和 LongStream 类中。`Stream` 类只有 count() 方法。其中一些方法返回一个 optional 对象。请注意该对象可能没有任何值，因此在获取值之前应进行检查：

```
DoubleStream doubleStream = DoubleGenerator
    .generateStreamFromList(numbers);
long numberOfElements = doubleStream.parallel().count();
System.out.printf("The list of numbers has %d elements.\n", numberOfElements);

doubleStream = DoubleGenerator.generateStreamFromList(numbers);
double sum = doubleStream.parallel().sum();
System.out.printf("Its numbers sum %f.\n", sum);

doubleStream = DoubleGenerator.generateStreamFromList(numbers);
double average = doubleStream.parallel().average()
    .getAsDouble();
System.out.printf("Its numbers have an average value of %f.\n", average);

doubleStream = DoubleGenerator.generateStreamFromList(numbers);
double max = doubleStream.parallel().max().getAsDouble();
System.out.printf("The maximum value in the list is %f.\n", max);

doubleStream = DoubleGenerator.generateStreamFromList(numbers);
double min = doubleStream.parallel().min().getAsDouble();
System.out.printf("The minimum value in the list is %f.\n", min);
```

8. 然后，我们将使用 reduce() 方法的第一个版本。此方法接收一个关联的 BinaryOperator 作为参数，该操作符接收两个相同类型的对象并返回该类型的对象。当操作处理完 Stream 的所有元素后，它返回一个参数化为相同类型的 Optional 对象。例如，我们将使用此版本来计算随机 Point 对象列表的两个坐标之和：

```
List<Point> points=PointGenerator.generatePointList(10000);
Optional<Point> point=points.parallelStream().reduce((p1,p2) -> {
    Point p=new Point();
    p.setX(p1.getX()+p2.getX());
    p.setY(p1.getY()+p2.getY());
    return p;
});
System.out.println(point.get().getX()+":"+point.get().getY());
```

9. 然后，我们将使用 reduce() 方法的第二个版本。它与前一个类似，但在这种情况下，除了关联的 BinaryOperator 对象外，它还接收该操作符的标识值（例如求和为 0，乘积为 1）并返回我们正在处理的类型的一个元素。如果流没有值，将返回标识值。在这种情况下，我们使用此版本的 reduce() 方法来计算我们在薪水中需要花费的总金额。我们使用 map() 方法将每个 Person 对象转换为 int 值（其薪水），因此我们的 Stream 对象在执行 reduce() 方法时将具有 int 值。你将在《转换流的元素》配方中获得有关 map() 方法的更多信息：

```
System.out.printf("Reduce, second version\n");
List<Person> persons = PersonGenerator.generatePersonList
(10000);
long totalSalary=persons.parallelStream().map
(p -> p.getSalary()).reduce(0, (s1,s2) -> s1+s2);
System.out.printf("Total salary: %d\n",totalSalary);
```

10. 最后，我们将使用 reduce() 方法的第三个版本。此版本用于 reduce 操作的结果类型与流元素类型不同时。我们必须提供返回类型的标识值、一个累加器，该累加器实现 BiFunction 接口并接收返回类型的对象、流的一个元素以生成返回类型的值，以及一个组合器函数，该函数实现 BinaryOperator 接口并接收两个返回类型的对象以生成该类型的对象。在这种情况下，我们已使用此版本的方法来计算随机人员列表中薪水高于 50,000 的人数：

```
Integer value=0;
value=persons.parallelStream().reduce(value, (n,p) -> {
if (p.getSalary() > 50000) {
return n+1;
} else {
return n;
}
}, (n1,n2) -> n1+n2);
System.out.printf("The number of people with a salary bigger
that 50,000 is %d\n",value);
```

