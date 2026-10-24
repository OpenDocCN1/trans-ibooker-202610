

在具体中介者的 Register 方法中，我们将人员添加到字典中或替换他们。在 Send 方法中，我们处理业务逻辑以将消息转发给相关的接收者。坦白说，过滤逻辑并不健壮，因为名字不是一个好的 ID，但这对于模式演示来说无关紧要。我们还将当前的具体中介者传递给 Person 对象，如下所示：

```
class Person{
    public string Name { get; }
    public Person(string name) => this.Name = name;
    public ConcreteMediator concreteMediator { set; get; }
    public void Send(string message, audience to = audience.everyone){
        concreteMediator.Send(Name,message, to);
    }
    public void Receive(
        string from, string message){
        Console.WriteLine("{0} - received {1}: from {2}",
            Name,message,from);
    }
}
```

图 5.37 – Person 对象

前述代码块中重要的部分在 Send 方法中，Person 类在其中回调具体中介者以处理消息的发送。Send 方法还具有一个可选的 audience 参数，这使我们在发送消息时能够过滤目标接收者。以下代码显示了两种人员的变体：

```
class Adult : Person{
    public Adult(string name): base(name){}
}
class Child : Person{
    public Child(string name): base(name){}
}
```

图 5.38 – Person 变体

现在，从主程序开始，我们可以按如下方式使用我们的类：

```
static void Main(string[] args){
    ConcreteMediator cm = new ConcreteMediator();
    Person a1 = new Adult("adult 1");
    Person a2 = new Adult("adult 2");
    Person c1 = new Child("child 1");
    cm.Register(a1);
    cm.Register(a2);
    cm.Register(c1);
    a1.Send("Hello adults",audience.adult);
    a1.Send("Hello everyone");
    Console.Read();
}
```

图 5.39 – 消费者代码

注意我们如何将所有人员实例注册到具体中介者。以下是程序执行时的输出：

![](img/78092c233296cbd7723546ae74eec12d_105_0.png)

图 5.40 – 中介者模式程序输出

图 5.40 显示只有 adult 2 接收到了来自 adult 1 的消息，因为他们明确针对成年人。他们的第二条消息是给所有人的，因此 child 1 也收到了它。

## 中介者模式总结

中介者有一些优点和缺点，概述如下：

- 它确保一对一和一对多的通信过程。
- 它解耦了相互通信的对象。
- 它将通信逻辑隔离在具体中介者中。
- 作为缺点，如果具体中介者实现随着时间的推移变得复杂，SRP 可能会受到损害。

让我们现在来处理结构型设计模式。

## 外观设计模式

外观模式是结构型类别的一部分，通过向消费者类公开简单干净的接口来隐藏具体类的复杂性。它充当消费者和具体实现之间的代理。我们将在最后一章重新回到外观设计模式，以了解一种无代码的方法来应用它。与此同时，让我们看看存储库设计模式，这是一种数据特定的外观实现。

## 存储库设计模式

存储库设计模式是外观设计模式的数据特定实现。你将其用作 API 控制器和 ORM 使用的数据传输对象（DTO）之间的机制。存储库是对对象集合的抽象。虽然关于存储库模式带来的附加价值存在一些争议，但其主要目的是解耦数据和业务域世界。关于存储库模式有用性的争论在于，有时它被认为与 ORM 是冗余的。

在 .NET 世界中，主要的 ORM 是 Entity Framework，它自带内置存储库。有些人认为在其中添加自己的存储库是过度设计，而另一些人则认为它是不可或缺的。我不会在这场争论中做出决定，但使用存储库的主要论点是为了提高可测试性并用其保存业务逻辑。你可以在这里看到存储库模式的图表：

![](img/78092c233296cbd7723546ae74eec12d_106_0.png)

图 5.41 – 存储库模式图

在图 5.41 的左侧，你可以看到一个由 IAsyncRepository 表示的通用存储库和一个由 IDemoRepository 表示的业务特定存储库。DemoController 类通过 DI 获取 IDemoRepository 的实例，其具体类是 DemoRepository，它又继承自 BaseRepository 类。

BaseRepository 和 DemoRepository 都利用 DemoContext（ORM）与底层数据存储交互。最后，DemoEntity 只是数据实体的表示。由于我们之前已经涉及过 DI 和控制器，我只会在下一节中列出最重要的代码块。你可以在本章的技术要求部分所述的 GitHub 上找到所有代码。

## 存储库模式实战

让我们首先从我们的通用存储库开始，如下所示：

```
public interface IAsyncRepository<T> where T : class{
    Task<T> GetByIdAsync(Guid id);
    Task<IReadOnlyList<T>> ListAllAsync();
    Task<T> AddAsync(T entity);
    Task UpdateAsync(T entity);
    Task DeleteAsync(T entity);
}
```

图 5.42 – 通用存储库

存储库是泛型的，因为它接受 T 作为输入，这是 .NET 处理泛型的方式。接口中声明的所有方法也是泛型的，因为它们不服务于任何特定的业务目的。它们只是代表典型的创建、读取、更新和删除（CRUD）操作。

这个通用存储库正是存储库模式备受争议的原因。如果你只坚持这种实现，你只是在添加冗余代码，因为默认情况下，ORM 也自带对对象集合的此类默认 CRUD 操作。

有些人可能会争辩说，通用存储库也给了你抽象掉 ORM 的机会，以防你在未来更改它。这种抽象级别也由 *整洁架构* 推荐，我们将在下一节讨论。这就是你必须运用良好判断并识别权衡以确保你不会过度设计解决方案并且它在你自己的上下文中是有意义的地方。

但当然，还有更多内容，那就是特定业务域存储库，其契约如下所示：

```
public interface IDemoRepository : IAsyncRepository<DemoEntity>{
    Task<IEnumerable<DemoEntity>> ListOnlyOddEntities();
}
```

图 5.43 – 领域级存储库

之所以它是业务特定的，是因为它指定了一个额外的非通用方法，看起来像一个特定查询。这种类型的存储库开始带来价值，因为它保存了业务逻辑。现在让我们看看 BaseRepository 类的截断（为简洁起见）版本，如下所示：

```
public class BaseRepository<T> : IAsyncRepository<T> where T : class{
    protected readonly DemoContext _dbContext;

    public BaseRepository(DemoContext dbContext) {
        _dbContext = dbContext;
    }

    public async Task<IReadOnlyList<T>> ListAllAsync(){
        return await _dbContext.Set<T>().ToListAsync();
    }
    other methods
}
```

图 5.44 – 基础存储库类

我们的 BaseRepository 类也是泛型的，可以接受任何实体。它实现了 IAsyncRepository<T>，我们的通用存储库契约。如你所见，它接受一个 DemoContext 对象的实例，这不过是我们 ORM 的入口点。现在来看看我们量身定制的存储库，如下所示：

```
public class DemoRepository : BaseRepository<DemoEntity>, IDemoRepository
{
    public DemoRepository(DemoContext dbContext) : base(dbContext) { }
    public async Task<IEnumerable<DemoEntity>> ListOnlyOddEntities(){
        return await Task<IEnumerable<DemoEntity>>.Run(() => {
            return _dbContext.DemoEntities.ToList().Where((c, i) => i % 2 != 0)
                as IEnumerable<DemoEntity>;
        });
    }
}
```

图 5.45 – 量身定制的存储库

DemoRepository 派生自 BaseRepository 并传入 DemoEntity 类型。它还通过 ListOnlyOddEntities 方法的具体实现实现了 IDemoRepository。如果你对 .NET 有一点了解，你可能已经在这里识别出一个代码异味。奇偶检查是在内存中完成的，导致 ORM 生成一个 SELECT * FROM ... 语句，这在性能上从来都不是好的。我故意这样做是为了向你展示编写非常可扩展的代码是多么容易，但也会留下这种糟糕的结构。在本章结束时，我会告诉你我认为在代码审查轮次中需要捕捉的最重要的事情。

## 存储库模式总结

使用存储库模式时需要考虑以下几点：

- 不要只坚持通用存储库。你应该努力将业务逻辑封装到更具体的存储库中。
- 不要抽象掉 ORM，它本身就是底层数据存储的抽象。可能 99.99% 的时间里，你将只在应用程序的生命周期内坚持使用单一 ORM。
- 存储库有助于提高可测试性。
- 保持抽象层的数量可控。我将留给你来评估什么是可控的。

让我们现在通过一个小的用例来看看你是否能应用你刚刚学到的关于设计模式的知识。

