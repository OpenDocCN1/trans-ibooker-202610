
当一个不常用的代码路径突然被大量线程同时使用时，就会出现字段的同步问题。这种情况并非不可能，但也不是最常见的场景。

要解决这种同步瓶颈，只有当懒加载的变量本身是线程安全时才有可能。`DateFormat` 对象不是线程安全的，所以在当前示例中，锁是否包含 `Calendar` 对象其实并不重要：如果懒加载的对象突然被大量使用，那么无论怎样，围绕 `DateFormat` 对象所需的同步都会成为问题。线程安全的代码应当如下所示：

```
public class CalDateInitialization {
    private Calendar calendar;
    private DateFormat df;

    private void report(Writer w) {
        synchronizedCalendarInit();
        synchronized(df) {
            w.write("On " + df.format(calendar.getTime()) + ": " + this);
        }
    }
}
```

对于涉及非线程安全的实例变量的懒加载，可以始终围绕该变量进行同步（例如，使用前面展示的同步版本的方法）。

考虑一个稍微不同的示例，其中有一个大型 `ConcurrentHashMap` 被懒加载：

```
public class CHMInitialization {
    private ConcurrentHashMap chm;

    public void doOperation() {
        synchronized(this) {
            if (chm == null) {
                chm = new ConcurrentHashMap();
                ... code to populate the map ...
            }
        }
        ...use the chm...
    }
}
```

由于多个线程可以安全地访问 `ConcurrentHashMap`，本例中的额外同步属于少数会被正确使用的懒加载引入同步瓶颈的情况之一。（不过，这样的瓶颈仍然应该很少见；如果对哈希映射的访问如此频繁，请考虑懒加载是否真的节省了什么。）该瓶颈可以使用双重检查锁定的惯用法来解决：

```
public class CHMInitialization {
    private volatile ConcurrentHashMap instanceChm;

    public void doOperation() {
        ConcurrentHashMap chm = instanceChm;
        if (chm == null) {
            synchronized(this) {
                chm = instanceChm;
                if (chm == null) {
                    chm = new ConcurrentHashMap();
                    ... code to populate the map
                    instanceChm = chm;
                }
            }
        }
        ...use the chm...
    }
}
```

存在重要的线程问题：实例变量必须声明为 `volatile`，并且将实例变量赋值给局部变量会带来一点性能上的好处。更多细节将在第 9 章中给出；在那些对线程代码进行懒加载有意义的少数情况下，这就是应该遵循的设计模式。

## 急切去初始化

与懒加载变量相对应的是急切地去初始化它们，即将它们的值设置为 `null`。这样可以让相关对象更快地被垃圾收集器回收。虽然理论上这听起来是件好事，但仅适用于有限的情况。

一个适合懒加载的变量似乎也适合急切去初始化：在前面的示例中，`Calendar` 和 `DateFormat` 对象可以在 `report()` 方法完成后设置为 `null`。但是，如果该变量不会在后续方法调用（或类中的其他地方）中使用，那么一开始就没有理由将其设为实例变量。只需在方法中创建局部变量，当方法完成时，局部变量将超出作用域，垃圾收集器就可以释放它。

关于不需要急切去初始化变量的规则，常见的例外出现在像 Java 集合框架这样的类中：这些类长期持有对数据的引用，然后被告知所持有的数据不再需要。以 JDK 中 `ArrayList` 类的 `remove()` 方法实现为例（部分代码已简化）：

```
public E remove(int index) {
    E oldValue = elementData(index);
    int numMoved = size - index - 1;
    if (numMoved > 0)
```

因此，没有理由避免设计和使用的不可变对象，即使这些对象不能被修改而必须重新创建，看起来可能有点适得其反。但在处理这些对象时，一个经常可行的优化是避免创建同一对象的重复副本。

最好的例子是 `Boolean` 类。任何 Java 应用程序只需要两个 `Boolean` 实例：一个用于 `true`，一个用于 `false`。不幸的是，`Boolean` 类的设计很糟糕。因为它有一个公共构造函数，应用程序可以根据需要创建任意多的这类对象，尽管它们与两个规范的 `Boolean` 对象之一完全相同。更好的设计应该是让 `Boolean` 类只拥有私有构造函数，并通过静态方法根据参数返回 `Boolean.TRUE` 或 `Boolean.FALSE`。如果您的自定义不可变类也能遵循这种模型，就可以防止它们增加应用程序的堆使用量。（理想情况下，显然您永远不应该创建 `Boolean` 对象；只需要在必要时使用 `Boolean.TRUE` 或 `Boolean.FALSE` 即可。）

不可变对象的这种单一表示被称为对象的规范版本。

## 创建规范对象

即使某个特定类的对象世界实际上是无限的，使用规范值也常常能节省内存。JDK 为最常见的不可变对象提供了一个工具：字符串可以调用 `intern()` 方法来找到字符串的规范版本。字符串驻留的更多细节将在第 12 章中讨论；目前我们先来看如何为自定义类实现同样的事情。

要将对象规范化，需要创建一个存储规范版本对象的映射。为防止内存泄漏，请确保映射中的对象是弱引用的。这样的类骨架如下所示：

```
public class ImmutableObject {
    private static WeakHashMap<ImmutableObject, ImmutableObject>
        map = new WeakHashMap();

    public ImmutableObject canonicalVersion(ImmutableObject io) {
        synchronized(map) {
            ImmutableObject canonicalVersion = map.get(io);
            if (canonicalVersion == null) {
                map.put(io, new WeakReference(io));
                canonicalVersion = io;
            }
            return canonicalVersion;
        }
    }
}
```

在线程环境中，同步可能会成为瓶颈。如果坚持使用 JDK 类，没有简单的解决方案，因为它们没有提供支持弱引用的并发哈希映射。不过，已经有提案向 JDK 添加 `CustomConcurrentHashMap`——最初是作为 Java 规范请求（JSR）166 的一部分——并且您可以找到许多第三方实现。

![](img/1711f142ae3cfd740ef106afbbba2046_240_0.png)

## 快速总结

-   不可变对象提供了特殊生命周期管理的可能性：规范化。
-   通过规范化消除不可变对象的重复副本，可以大大减少应用程序使用的堆空间。

## 对象生命周期管理

本章讨论的内存管理的第二个广泛主题是对象生命周期管理。在大多数情况下，Java 试图最大限度地减少开发人员在管理对象生命周期方面所需投入的精力：开发人员在需要时创建对象，当不再需要时，对象超出作用域并被垃圾收集器释放。

有时这种正常的生命周期并非最优。某些对象的创建成本很高，管理这些对象的生命周期将提高应用程序的效率，即使这需要垃圾收集器付出额外的工作。本节探讨何时以及如何更改对象的正常生命周期，无论是通过重用对象还是通过维护对它们的特殊引用。

### 对象重用

对象重用通常通过两种方式实现：对象池和线程局部变量。世界各地的 GC 工程师现在都在抱怨，因为这两种技术都会妨碍 GC 的效率。特别是对象池，因此在 GC 圈子里广泛不受欢迎；不过，对象池在开发圈子里也因许多其他原因而广泛不受欢迎。

在某个层面上，这种立场的原因似乎很明显：被重用的对象会在堆中长时间存在。如果堆中有大量对象，那么创建新对象的空间就会减少，因此 GC 操作会更加频繁。但这只是故事的一部分。

