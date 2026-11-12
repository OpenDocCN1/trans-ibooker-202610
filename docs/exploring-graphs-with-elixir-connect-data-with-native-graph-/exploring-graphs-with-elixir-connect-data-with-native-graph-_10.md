## 附录 1

## 函数式接口入门集

The JDK has a number of functional interfaces. Here we review the starter set—the interfaces we frequently encounter and need to get familiar with. All the interfaces we see here are part of the java.util.function package.

### Consumer<T>

| Description | 表示一个接受输入且不返回任何内容的操作。要使其有用，它必须引起副作用。 |
| :--- | :--- |
| Abstract method | accept |
| default method(s) | andThen |
| Popular usage | 作为 `forEach` 方法的参数 |
| Primitive specializations | `IntConsumer`, `LongConsumer`, `DoubleConsumer`... |

### Supplier<T>

| Property | Value |
|---|---|
| Description | 一个预期返回新实例或预创建实例的工厂 |
| Abstract method | get |
| default method(s) | — |
| Popular usage | 用于创建惰性无限 Streams，并作为 Optional 类的 orElseGet 方法的参数 |
| Primitive specializations | IntSupplier, LongSupplier, DoubleSupplier... |





### Predicate<T>

| 描述 | 用于检查输入参数是否满足某个条件 |
|---|---|
| 抽象方法 | test |
| 默认方法 | and, negate, or |
| 常见用途 | 作为 Stream 方法的参数，如 filter 和 anyMatch |
| 基本类型特化 | IntPredicate, LongPredicate, DoublePredicate... |

### Function<T, R>

| 描述 | 一个转换式接口，表示旨在接受参数并返回适当结果的操作 |
|---|---|
| 抽象方法 | apply |
| 默认方法 | andThen, compose |
| 常见用途 | 作为 Stream map 方法的参数 |
| 基本类型特化 | IntFunction, LongFunction, DoubleFunction, IntToDoubleFunction, DoubleToIntFunction... |
